# object_detection

## Installation
COCO API installation
```bash
# For CPU
pip2 install tensorflow
# For GPU
pip2 install tensorflow-gpu

pip2 install Cython
pip2 install pillow
pip2 install lxml
pip2 install jupyter
pip2 install matplotlib
```
* COCO API installation
```bash
# git clone https://github.com/cocodataset/cocoapi.git
git clone https://github.com/philferriere/cocoapi.git
cd cocoapi/PythonAPI
python2 setup.py install
```
* Protobuf Compilation
```bash
# From tensorflow/models/research/
protoc360 object_detection/protos/anchor_generator.proto --python_out=.
protoc360 object_detection/protos/argmax_matcher.proto --python_out=.
protoc360 object_detection/protos/bipartite_matcher.proto --python_out=.
protoc360 object_detection/protos/box_coder.proto --python_out=.
protoc360 object_detection/protos/box_predictor.proto --python_out=.
protoc360 object_detection/protos/eval.proto --python_out=.
protoc360 object_detection/protos/faster_rcnn.proto --python_out=.
protoc360 object_detection/protos/faster_rcnn_box_coder.proto --python_out=.
protoc360 object_detection/protos/graph_rewriter.proto --python_out=.
protoc360 object_detection/protos/grid_anchor_generator.proto --python_out=.
protoc360 object_detection/protos/hyperparams.proto --python_out=.
protoc360 object_detection/protos/image_resizer.proto --python_out=.
protoc360 object_detection/protos/input_reader.proto --python_out=.
protoc360 object_detection/protos/keypoint_box_coder.proto --python_out=.
protoc360 object_detection/protos/losses.proto --python_out=.
protoc360 object_detection/protos/matcher.proto --python_out=.
protoc360 object_detection/protos/mean_stddev_box_coder.proto --python_out=.
protoc360 object_detection/protos/model.proto --python_out=.
protoc360 object_detection/protos/multiscale_anchor_generator.proto --python_out=.
protoc360 object_detection/protos/optimizer.proto --python_out=.
protoc360 object_detection/protos/pipeline.proto --python_out=.
protoc360 object_detection/protos/post_processing.proto --python_out=.
protoc360 object_detection/protos/preprocessor.proto --python_out=.
protoc360 object_detection/protos/region_similarity_calculator.proto --python_out=.
protoc360 object_detection/protos/square_box_coder.proto --python_out=.
protoc360 object_detection/protos/ssd.proto --python_out=.
protoc360 object_detection/protos/ssd_anchor_generator.proto --python_out=.
protoc360 object_detection/protos/string_int_label_map.proto --python_out=.
protoc360 object_detection/protos/train.proto --python_out=.
```
* Testing the Installation
```bash
python3 object_detection/builders/model_builder_test.py
```

## Preparing Inputs
* Generating the PASCAL VOC TFRecord files.
```bash
# From tensorflow/models/research/
wget http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
tar -xvf VOCtrainval_11-May-2012.tar
python2 object_detection/dataset_tools/create_pascal_tf_record.py \
    --label_map_path=object_detection/data/pascal_label_map.pbtxt \
    --data_dir=VOCdevkit --year=VOC2012 --set=train \
    --output_path=pascal_train.record
python2 object_detection/dataset_tools/create_pascal_tf_record.py \
    --label_map_path=object_detection/data/pascal_label_map.pbtxt \
    --data_dir=VOCdevkit --year=VOC2012 --set=val \
    --output_path=pascal_val.record
```    

### Generating the Oxford-IIIT Pet TFRecord files
```bash
# From tensorflow/models/research/
wget http://www.robots.ox.ac.uk/~vgg/data/pets/data/images.tar.gz
wget http://www.robots.ox.ac.uk/~vgg/data/pets/data/annotations.tar.gz
tar -xvf annotations.tar.gz
tar -xvf images.tar.gz
# fix image with png/gif format
python2 object_detection/dataset_tools/preprocess_pet.py
tar -xvf annotations.tar.gz
tar -xvf images.tar.gz
python2 object_detection/dataset_tools/create_pet_tf_record.py \
    --label_map_path=object_detection/data/pet_label_map.pbtxt \
    --data_dir=`pwd` \
    --output_dir=`pwd`
```
## running_locally
* Recommended Directory Structure for Training and Evaluation
```
+mine
    +detection_model_zoo/
        +faster_rcnn_resnet101_coco_2018_01_28/
            -model.ckpt.index
            -model.ckpt.meta
            -model.ckpt.data-00000-of-00001
    +pascal/
        +data/
            -pascal_label_map.pbtxt
            -pascal_train.record
            -pascal_val.record
        +faster_rcnn_resnet101_coco/
            -faster_rcnn_resnet101_voc07.config
            +train/
            +eval/
    -pascal_tensorboard.sh
    -pascal_train.sh
    -pascal_val.sh
```
* Running the Training Job
```bash
# From the tensorflow/models/research/ directory
PATH_TO_YOUR_PIPELINE_CONFIG=./faster_rcnn_resnet101_coco/faster_rcnn_resnet101_voc07.config
PATH_TO_TRAIN_DIR=./faster_rcnn_resnet101_coco/train
python2 -m object_detection.train \
    --logtostderr \
    --pipeline_config_path=${PATH_TO_YOUR_PIPELINE_CONFIG} \
    --train_dir=${PATH_TO_TRAIN_DIR}
```    
* Running the Evaluation Job
```bash
# From the tensorflow/models/research/ directory
PATH_TO_YOUR_PIPELINE_CONFIG=./faster_rcnn_resnet101_coco/faster_rcnn_resnet101_voc07.config
PATH_TO_TRAIN_DIR=./faster_rcnn_resnet101_coco/train
PATH_TO_EVAL_DIR=./faster_rcnn_resnet101_coco/eval
python2 -m object_detection.eval \
  --logtostderr \
  --pipeline_config_path=${PATH_TO_YOUR_PIPELINE_CONFIG} \
  --checkpoint_dir=${PATH_TO_TRAIN_DIR} \
  --eval_dir=${PATH_TO_EVAL_DIR}
```
* Running Tensorboard
```bash
PATH_TO_MODEL_DIRECTORY=./faster_rcnn_resnet101_coco
tensorboard --logdir=${PATH_TO_MODEL_DIRECTORY}
```

## Exporting a trained model for inference
```bash
# From tensorflow/models/research/
PIPELINE_CONFIG_PATH=./faster_rcnn_resnet101_coco/faster_rcnn_resnet101_voc07.config
TRAIN_PATH=./faster_rcnn_resnet101_coco/train/model.ckpt-5183
EXPORT_DIR=./faster_rcnn_resnet101_coco/exported_graphs
python2 -m object_detection.export_inference_graph \
  --input_type image_tensor \
  --pipeline_config_path ${PIPELINE_CONFIG_PATH} \
  --trained_checkpoint_prefix ${PIPELINE_CONFIG_PATH} \
  --output_directory ${EXPORT_DIR}
```

## issues
* [Corrupt JPEG data: 245 extraneous bytes before marker 0xd9 ](https://github.com/tensorflow/models/issues/2194)
* [TypeError: Expected int32, got range(0, 3) of type 'range' instead.](https://github.com/tensorflow/models/issues/3443)
