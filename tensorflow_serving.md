# tensorflow_serving

## Using TensorFlow Serving via Docker
### Installing Docker
docker for windows

### Serving with Docker
```bash
# pull the serving image:
docker pull tensorflow/serving

# run the TensorFlow Serving container pointing it to this model and opening the REST API port (8501):
docker run -p 8501:8501 \
  --mount type=bind, \
  source=/tmp/tfserving/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_cpu,\
  target=/models/half_plus_two \
  -e MODEL_NAME=half_plus_two -t tensorflow/serving &

# query the model using the predict API
curl -d '{"instances": [1.0, 2.0, 5.0]}' \
  -X POST http://localhost:8501/v1/models/half_plus_two:predict

curl -d '{"signature_name": "tensorflow/serving/regress", "examples": [{"x": 1.0}, {"x": 2.0}]}' \
 -X POST http://localhost:8501/v1/models/half_plus_two:regress  
```

### Serving with Docker using your GPU
```bash
# pull the serving image:
docker pull tensorflow/serving:latest-gpu

# run the TensorFlow Serving container pointing it to this model and opening the REST API port (8501):
docker run --runtime=nvidia -p 8501:8501 \
  --mount type=bind,\
  source=/tmp/tfserving/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_gpu,\
  target=/models/half_plus_two \
  -e MODEL_NAME=half_plus_two -t tensorflow/serving:latest-gpu &

# query the model using the predict API
curl -d '{"instances": [1.0, 2.0, 5.0]}' \
  -X POST http://localhost:8501/v1/models/half_plus_two:predict
```

## Serving a TensorFlow Model

```bash
# Train and export TensorFlow model
python3 tensorflow_serving/example/mnist_saved_model.py models/mnist

# Load exported model with standard TensorFlow ModelServer
docker run -p 8500:8500 \
--mount type=bind,source=$(pwd)/models/mnist,target=/models/mnist \
-e MODEL_NAME=mnist -t tensorflow/serving &  

# Test the server
python3 tensorflow_serving/example/mnist_client.py --num_tests=1000 --server=127.0.0.1:8500
```

## Building Standard TensorFlow ModelServer

```bash
python3 tensorflow_serving/example/mnist_saved_model.py --training_iteration=2000 --model_version=2 models/mnist 

mkdir models/monitored
cp -r models/mnist/1 models/monitored

docker run -p 8500:8500 \
  --mount type=bind,source=$(pwd)/models/monitored,target=/models/mnist \
  -t --entrypoint=tensorflow_model_server tensorflow/serving  --enable_batching \
  --port=8500 --model_name=mnist --model_base_path=/models/mnist &

python3 tensorflow_serving/example/mnist_client.py --num_tests=1000 --server=127.0.0.1:8500 --concurrency=10

cp -r models/mnist/2 models/monitored

python3 tensorflow_serving/example/mnist_client.py --num_tests=1000 --server=127.0.0.1:8500 --concurrency=10
```

## Serving Inception Model with TensorFlow Serving and Kubernetes
```bash
curl -O http://download.tensorflow.org/models/image/imagenet/inception-v3-2016-03-01.tar.gz
tar xzf inception-v3-2016-03-01.tar.gz
python3 tensorflow_serving/example/inception_saved_model.py --checkpoint_dir=inception-v3 --output_dir=/tmp/inception-export

docker run -p 8500:8500 \
  --mount type=bind,source=/tmp/inception-export,target=/models/inception \
  -t --entrypoint=tensorflow_model_server tensorflow/serving  --enable_batching --port=8500 --model_name=inception --model_base_path=/models/inception &

python3 tensorflow_serving/example/inception_client.py --server=localhost:8500 --image=/tmp/my_cat_image.jpg  
```
```bash
docker run -p 8500:8500 -p 8501:8501 --mount type=bind,source=e:/tmp/inception-export,target=/models/inception -t --entrypoint=tensorflow_model_server tensorflow/serving  --enable_batching --port=8500 --rest_api_port=8501 --model_name=inception --model_base_path=/models/inception &
```
rest 
```bash
import requests
import base64
import json

image = r"e:/tmp/my_cat_image.jpg"
URL = "http://localhost:8501/v1/models/inception/versions/1:classify" 
headers = {"content-type": "application/json"}
image_content = base64.b64encode(open(image,'rb').read()).decode("utf-8")
body = {
    "signature_name": "serving_default",
    "examples": [
                {"image/encoded": {"b64":image_content}}
                ]
    }
r = requests.post(URL, data=json.dumps(body), headers = headers) 
print(r.text)
```
```bash
curl -X POST 
http://localhost:8501/v1/models/inception/versions/1:classify 
-d '{"signature_name": <signature_name>,
"examples": [
{
"image": { "b64": "<use a converted base 64 encoding image> or $( base64 <image_path> )" },
}
]
}'
```