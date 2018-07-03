# object_detection

## Installation
COCO API installation
```
git clone https://github.com/cocodataset/cocoapi.git
cd cocoapi/PythonAPI
python setup.py install
```
Protobuf Compilation
```
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
Add Libraries to PYTHONPATH
```
# From tensorflow/models/research/slim
python setup.py build
python setup.py install
```
Testing the Installation
```
python -m object_detection.builders.model_builder_test
```
## Preparing Inputs
### Generating the PASCAL VOC TFRecord files.
```
# From tensorflow/models/research/
wget http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
tar -xvf VOCtrainval_11-May-2012.tar
python -m object_detection.dataset_tools.create_pascal_tf_record --label_map_path=object_detection/data/pascal_label_map.pbtxt --data_dir=VOCdevkit --year=VOC2012 --set=train -output_path=pascal_train.record
python -m object_detection.dataset_tools.create_pascal_tf_record --label_map_path=object_detection/data/pascal_label_map.pbtxt --data_dir=VOCdevkit --year=VOC2012 --set=val --output_path=pascal_val.record
```    

### Generating the Oxford-IIIT Pet TFRecord files
```
# From tensorflow/models/research/
wget http://www.robots.ox.ac.uk/~vgg/data/pets/data/images.tar.gz
wget http://www.robots.ox.ac.uk/~vgg/data/pets/data/annotations.tar.gz
tar -xvf annotations.tar.gz
tar -xvf images.tar.gz
# fix image with png/gif format
python -m object_detection.dataset_tools.preprocess_pet
python -m object_detection.dataset_tools.create_pet_tf_record --label_map_path=object_detection/data/pet_label_map.pbtxt --data_dir=. --output_dir=. --faces_only=False
```
## running_locally
### Recommended Directory Structure for Training and Evaluation
```
+detection_model_zoo/
    +faster_rcnn_resnet101_coco_11_06_2017/
        -model.ckpt.index
        -model.ckpt.meta
        -model.ckpt.data-00000-of-00001
+mine
    +pet/
        +data/
            -pet_label_map.pbtxt
            -pet_train.record
            -pet_val.record
        +faster_rcnn_resnet101_coco/
            -faster_rcnn_resnet101_pets.config
            +train/
            +eval/
```
### Running the Training Job
```
# From the tensorflow/models/research/ directory
python -m object_detection.train --logtostderr --pipeline_config_path=${PATH_TO_YOUR_PIPELINE_CONFIG} --train_dir=${PATH_TO_TRAIN_DIR}

python -m object_detection.train --logtostderr --pipeline_config_path=./object_detection/mine/pet/data/faster_rcnn_resnet101_coco/faster_rcnn_resnet101_pets.config --train_dir=./object_detection/mine/pet/faster_rcnn_resnet101_coco/train
```    
### Running the Evaluation Job
```
# From the tensorflow/models/research/ directory
python -m object_detection.eval --logtostderr --pipeline_config_path=${PATH_TO_YOUR_PIPELINE_CONFIG} --checkpoint_dir=${PATH_TO_TRAIN_DIR} --eval_dir=${PATH_TO_EVAL_DIR}

python -m object_detection.eval --logtostderr --pipeline_config_path=./object_detection/mine/pet/faster_rcnn_resnet101_coco/faster_rcnn_resnet101_pets.config --checkpoint_dir=./object_detection/mine/pet/faster_rcnn_resnet101_coco/train --eval_dir=./object_detection/mine/pet/faster_rcnn_resnet101_coco/eval
```
### Running Tensorboard
```
tensorboard --logdir=${PATH_TO_MODEL_DIRECTORY}

tensorboard --logdir=./object_detection/mine/pet/faster_rcnn_resnet101_coco
```

## Exporting a trained model for inference
```
# From tensorflow/models/research/
python -m object_detection.export_inference_graph --input_type image_tensor --pipeline_config_path ${PIPELINE_CONFIG_PATH} --trained_checkpoint_prefix ${TRAIN_PATH} --output_directory ${EXPORT_DIR}

python -m object_detection.export_inference_graph --input_type image_tensor --pipeline_config_path ./object_detection/mine/pet/faster_rcnn_resnet101_coco/faster_rcnn_resnet101_pets.config --trained_checkpoint_prefix ./object_detection/mine/pet/faster_rcnn_resnet101_coco/train/model.ckpt-18389 --output_directory ./object_detection/mine/pet/faster_rcnn_resnet101_coco/exported_graphs
```

## issues
* [Corrupt JPEG data: 245 extraneous bytes before marker 0xd9 ](https://github.com/tensorflow/models/issues/2194)
* [TypeError: Expected int32, got range(0, 3) of type 'range' instead.](https://github.com/tensorflow/models/issues/3443)
