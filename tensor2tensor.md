# Tensor2Tensor
[https://github.com/tensorflow/tensor2tensor](https://github.com/tensorflow/tensor2tensor)

## 
```
# Data generator
python -m tensor2tensor.bin.t2t_datagen

# Trainer
python -m tensor2tensor.bin.t2t_trainer --registry_help
```

## Suggested Datasets and Models
### Image Classification
|dataset|problem|model|hparams_set|
|-|-|-|-|
|ImageNet|image_imagenet<br>image_imagenet224<br>image_imagenet64<br>image_imagenet32|resnet<br>xception|resnet_50<br>xception_base|
|CIFAR-10|image_cifar10<br>image_cifar10_plain|shake_shake|shakeshake_big|
|CIFAR-100|image_cifar100|shake_shake|shakeshake_big|
|MNIST|image_mnist|shake_shake|shake_shake_quick<br>shakeshake_big|

### Language Modeling
|dataset|problem|model|hparams_set|
|-|-|-|-|
|PTB|languagemodel_ptb10k<br>language_ptb_characters|transformer|transformer_small|
|LM1B|languagemodel_lm1b32<br>language_lm2b_characters|transformer|transformer_base|

### Sentiment Analysis
|dataset|problem|model|hparams_set|
|-|-|-|-|
|IMDB|sentiment_imdb|transformer_encoder|transformer_tiny|

### Speech Recognition
|dataset|problem|model|hparams_set|
|-|-|-|-|
|Librispeech|librispeech<br>librispeech_clean|
|Mozilla Common Voice|common_voice<br>common_voice_clean|

### Summarization
|dataset|problem|model|hparams_set|
|-|-|-|-|
|CNN/DailyMail articles summarized into a few sentences|summarize_cnn_dailymail32k|transformer|transformer_prepend|

### Translation
|dataset|problem|model|hparams_set|
|-|-|-|-|
|English-German|translate_ende_wmt32k|transformer|transformer_base<br>transformer_base_single_gpu<br>transformer_big|
|English-French|translate_enfr_wmt32k|transformer|transformer_base<br>transformer_base_single_gpu<br>transformer_big|
|English-Czech|translate_encs_wmt32k|transformer|transformer_base<br>transformer_base_single_gpu<br>transformer_big|
|English-Chinese|translate_enzh_wmt32k|transformer|transformer_base<br>transformer_base_single_gpu<br>transformer_big|
|English-Vietnamese|translate_envi_iwslt32k|transformer|transformer_base<br>transformer_base_single_gpu<br>transformer_big|

## Examples
image_mnist
```
python -m tensor2tensor.bin.t2t_trainer --generate_data --data_dir=/t2t_data --output_dir=/t2t_train/mnist --problem=image_mnist --model=shake_shake --hparams_set=shake_shake_quick --train_steps=1000 --eval_steps=100
```
image_fashion_mnist
```
python -m tensor2tensor.bin.t2t_trainer --generate_data --data_dir=/t2t_data --output_dir=/t2t_train/fashion_mnist --problem=image_fashion_mnist --model=shake_shake --hparams_set=shake_shake_quick --train_steps=1000 --eval_steps=100
```
image_cifar10
```
python3 -m tensor2tensor.bin.t2t_trainer --generate_data --data_dir=/t2t_data --output_dir=/t2t_train/cifar10 --problem=image_cifar10 --model=shake_shake --hparams_set=shakeshake_big --train_steps=1000 --eval_steps=100
```
image_cifar100
```
python3 -m tensor2tensor.bin.t2t_trainer --generate_data --data_dir=/t2t_data --output_dir=/t2t_train/cifar100 --problem=image_cifar100 --model=shake_shake --hparams_set=shakeshake_big --train_steps=1000 --eval_steps=100
```
image_imagenet
```
python3 -m tensor2tensor.bin.t2t_trainer --generate_data --data_dir=/t2t_data --output_dir=/t2t_train/imagenet --problem=image_imagenet224 --model=resnet --hparams_set=resnet_50 --train_steps=1000 --eval_steps=100
```


