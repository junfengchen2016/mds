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