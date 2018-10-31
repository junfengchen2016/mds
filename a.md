# tensorflow_install


## ubuntu
```
sudo apt-get install python-pip python-dev
```
Ubuntu/Linux 64-bit, CPU only, Python 2.7
```
download https://storage.googleapis.com/tensorflow/linux/cpu/tensorflow-0.10.0rc0-cp27-none-linux_x86_64.whl
```
Ubuntu/Linux 64-bit, GPU enabled, Python 2.7
Requires CUDA toolkit 7.5 and CuDNN v4. For other versions, see "Install from sources" below.
```
download https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.10.0rc0-cp27-none-linux_x86_64.whl
```
```
sudo pip install --upgrade gpu/tensorflow-0.10.0rc0-cp27-none-linux_x86_64.whl
```
test
```
python -m tensorflow.models.image.mnist.convolutional
```

### gpu
```
sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64.deb
sudo apt-get update
sudo apt-get install cuda

sudo dpkg -i cuda-repo-ubuntu1604-8-0-local-cublas-performance-update_8.0.61-1_amd64.deb.61-1_amd64-deb
sudo apt-get update
sudo apt-get install cuda

sudo dpkg -i libcudnn7_7.0.3.11-1+cuda9.0_amd64.deb
sudo dpkg -i libcudnn7-dev_7.0.3.11-1+cuda9.0_amd64.deb
sudo dpkg -i libcudnn7-doc_7.0.3.11-1+cuda9.0_amd64.deb

//tar xvzf cudnn-8.0-linux-x64-v6.0-tgz
//sudo cp -P cuda/include/cudnn.h /usr/local/cuda/include
//sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64
//sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

## ubuntu
### bazel
[https://docs.bazel.build/versions/master/install-ubuntu.html](https://docs.bazel.build/versions/master/install-ubuntu.html)
[https://github.com/bazelbuild/bazel/releases](https://github.com/bazelbuild/bazel/releases)

```
sudo apt-get install pkg-config zip g++ zlib1g-dev unzip python

chmod +x bazel-0.5.2-installer-linux-x86_64.sh
./bazel-0.5.2-installer-linux-x86_64.sh --user

export PATH="$PATH:$HOME/bin"
```

### tensorflow-cpu
```
sudo apt-get install python3-numpy python3-dev python3-pip python3-wheel

cd tensorflow
./configure
Please specify the location of python. [Default is /usr/bin/python]: /usr/bin/python3
Do you wish to build TensorFlow with CUDA support? [y/N] N

bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package

bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
```

### tensorflow-gpu


## windows
host
```
#TensorFlow start 
64.233.188.121 www.tensorflow.org 
#TensorFlow end
```

Anaconda3-4.4.0-Windows-x86_64.exe
cuda_8.0.61_win10.exe
cuda_8.0.61.2_windows.exe
cudnn-8.0-windows10-x64-v6.0.zip
```
pip install tensorflow-1.2.1-cp36-cp36m-win_amd64.whl
pip install tensorflow_gpu-1.2.1-cp36-cp36m-win_amd64.whl
```

```
pip install opencv-python
```

```
pip install numpy --upgrade
```

## serving
### setup
[https://github.com/tensorflow/serving/blob/master/tensorflow_serving/g3doc/setup.md](https://github.com/tensorflow/serving/blob/master/tensorflow_serving/g3doc/setup.md)
```
sudo apt-get install python-pip
sudo pip install --upgrade pip

sudo pip install grpcio

sudo apt-get update && sudo apt-get install -y \
        build-essential \
        curl \
        libcurl3-dev \
        git \
        libfreetype6-dev \
        libpng12-dev \
        libzmq3-dev \
        pkg-config \
        python-dev \
        python-numpy \
        python-pip \
        software-properties-common \
        swig \
        zip \
        zlib1g-dev
sudo pip install tensorflow-serving-api

echo "deb [arch=amd64] http://storage.googleapis.com/tensorflow-serving-apt stable tensorflow-model-server tensorflow-model-server-universal" | sudo tee /etc/apt/sources.list.d/tensorflow-serving.list
curl https://storage.googleapis.com/tensorflow-serving-apt/tensorflow-serving.release.pub.gpg | sudo apt-key add -

sudo apt-get update && sudo apt-get install tensorflow-model-server
sudo apt-get upgrade tensorflow-model-server
```
### run
basic
[https://github.com/tensorflow/serving/blob/master/tensorflow_serving/g3doc/serving_basic.md](https://github.com/tensorflow/serving/blob/master/tensorflow_serving/g3doc/serving_basic.md)
```
$>rm -rf /tmp/mnist_model
python tensorflow_serving/example/mnist_saved_model.py /tmp/mnist_model
tensorflow_model_server --port=9000 --model_name=mnist --model_base_path=/tmp/mnist_model/
python tensorflow_serving/example/mnist_client.py --num_tests=1000 --server=localhost:9000
```
advanced
[https://github.com/tensorflow/serving/blob/master/tensorflow_serving/g3doc/serving_advanced.md](https://github.com/tensorflow/serving/blob/master/tensorflow_serving/g3doc/serving_advanced.md)
```
$>rm -rf /tmp/mnist_model
python tensorflow_serving/example/mnist_saved_model.py --training_iteration=100 --model_version=1 /tmp/mnist_model
python tensorflow_serving/example/mnist_saved_model.py --training_iteration=2000 --model_version=2 /tmp/mnist_model
tensorflow_model_server --enable_batching --port=9000 --model_name=mnist --model_base_path=/tmp/monitored
python tensorflow_serving/example/mnist_client.py --num_tests=1000 --server=localhost:9000 --concurrency=10
```
### from source
Bazel(only if compiling source code)
```
chmod +x bazel-0.4.5-installer-linux-x86_64.sh
./bazel-0.4.5-installer-linux-x86_64.sh --user

export PATH="$PATH:$HOME/bin"
```
Clone the TensorFlow Serving repository
```
git clone --recurse-submodules https://github.com/tensorflow/serving
cd serving
```
compile
```
cd tensorflow
./configure
cd ..
bazel build -c opt tensorflow_serving/...
bazel test -c opt tensorflow_serving/...
```
basic
```
$>rm -rf /tmp/mnist_model

$>bazel build -c opt //tensorflow_serving/example:mnist_saved_model
$>bazel-bin/tensorflow_serving/example/mnist_saved_model /tmp/mnist_model

$>bazel build -c opt //tensorflow_serving/model_servers:tensorflow_model_server
$>bazel-bin/tensorflow_serving/model_servers/tensorflow_model_server --port=9000 --model_name=mnist --model_base_path=/tmp/mnist_model/

$>bazel build -c opt //tensorflow_serving/example:mnist_client
$>bazel-bin/tensorflow_serving/example/mnist_client --num_tests=1000 --server=localhost:9000
```
advanced
```
$>rm -rf /tmp/mnist_model

$>bazel build -c opt //tensorflow_serving/example:mnist_saved_model
$>bazel-bin/tensorflow_serving/example/mnist_saved_model --training_iteration=100 --model_version=1 /tmp/mnist_model

$>bazel-bin/tensorflow_serving/example/mnist_saved_model --training_iteration=2000 --model_version=2 /tmp/mnist_model
```