# Tesseract OCR

* [APIExample](https://github.com/tesseract-ocr/tesseract/wiki/APIExample)
* [TrainingTesseract 4.00](https://github.com/tesseract-ocr/tesseract/wiki/TrainingTesseract-4.00)
* [pytesseract · PyPI](https://pypi.org/project/pytesseract/)
* [训练Tesseract4.0](https://ivanzz1001.github.io/records/post/ocr/2017/09/18/tesseract-training)
* [Tesseract中英文正体斜体混合训练](https://www.jianshu.com/p/b219ea55f130)
* [Tess4.0手动合并输入数据及模型训练流程](https://www.jianshu.com/p/28aabd574d3a)
* [特殊字符语言包训练流程（新）](https://www.jianshu.com/p/7a2c40dd6560)
* [Need a free call to match GetUTF8Text(); #1030](https://github.com/tesseract-ocr/tesseract/issues/1030)


## compile
* linux
```bash
sudo apt-get install libicu-dev
sudo apt-get install libpango1.0-dev
sudo apt-get install libcairo2-dev
sudo apt-get install libleptonica-dev
```
```bash
# from tesseract-ocr_tesseract/tesseract-4.0.0
./autogen.sh
configure
make
sudo make install
make training
sudo make training-install
sudo ldconfig
```
* windows
```
1 下载最新的CPPAN，将cppan.exe所在的文件路径作为环境变量的值。
2 下载最新的cmake，解压文件压缩包，将解压后目录下的bin文件夹的目录地址加载至系统环境变量PATH中。
3 下载Tesseract最新版的源码：git clone https://github.com/tesseract-ocr/tesseract tesseract
4 执行以下命令，编译：
cd tesseract
mkdir win64 && cd win64
cppan ..
cmake .. -G "Visual Studio 14 2015 Win64"
cmake .. -G "Visual Studio 15 2017 Win64"
5 在win64目录下找到tesseract.sln文件，用VS2015打开该文件，点击【生成】按钮，根据相应错误进行改错。(带签名的utf-8编码)
```

## train
```bash
sudo cp simsun.ttc /usr/share/fonts
```
```bash
git clone https://github.com/tesseract-ocr/tesseract.git
git clone https://github.com/tesseract-ocr/langdata.git
git clone https://github.com/tesseract-ocr/tessdata_best.git
cp ./tessdata_best/eng.traineddata ./tesseract/tessdata
cp ./tessdata_best/chi_sim.traineddata ./tesseract/tessdata
cp ./tessdata_best/chi_sim_vert.traineddata ./tesseract/tessdata
```
* 生成新的训练数据
```bash
tesstrain.sh --fonts_dir /usr/share/fonts --lang chi_sim --linedata_only \
--noextract_font_properties --langdata_dir ../langdata \
--fontlist "SIMSUN" --tessdata_dir ./tessdata --output_dir ~/tesstutorial/trainspecial

tesstrain.sh --fonts_dir /usr/share/fonts --lang chi_sim --linedata_only \
--noextract_font_properties --langdata_dir ../langdata \
--tessdata_dir ./tessdata \
--fontlist "SIMSUN" --output_dir ~/tesstutorial/evalspecial
```

* scratch训练
```bash
export SCROLLVIEW_PATH=$PWD/java

mkdir -p ~/tesstutorial/specialoutput

lstmtraining --debug_interval 100 \
--traineddata ~/tesstutorial/trainspecial/chi_sim/chi_sim.traineddata \
--net_spec '[1,0,0,1 Ct5,5,16 Mp3,3 Lfys64 Lfx128 Lrx128 Lfx384 O1c5000]' \
--model_output ~/tesstutorial/specialoutput/base --learning_rate 20e-4 \
--train_listfile ~/tesstutorial/trainspecial/chi_sim.training_files.txt \
--eval_listfile ~/tesstutorial/evalspecial/chi_sim.training_files.txt \
--max_iterations 3600 &>~/tesstutorial/specialoutput/basetrain.log
```
* finetune训练
```bash
combine_tessdata -e ./tessdata/chi_sim.traineddata \
  ~/tesstutorial/trainspecial/chi_sim.lstm

lstmtraining --model_output ~/tesstutorial/trainspecial/special \
  --continue_from ~/tesstutorial/trainspecial/chi_sim.lstm \
  --traineddata ~/tesstutorial/trainspecial/chi_sim/chi_sim.traineddata \
  --old_traineddata ./tessdata/chi_sim.traineddata \
  --train_listfile ~/tesstutorial/trainspecial/chi_sim.training_files.txt \
  --max_iterations 3600
```
合并训练结果
* finetune训练合并
```bash
lstmtraining --stop_training \
  --continue_from ~/tesstutorial/trainspecial/special_checkpoint \
  --traineddata ~/tesstutorial/trainspecial/chi_sim/chi_sim.traineddata \
  --model_output ~/tesstutorial/trainspecial/chi_sim_special.traineddata
```
新生成的chi_sim_special.traineddata在~/tesstutorial/trainspecial目录下。

* scratch训练合并
```bash
lstmtraining --stop_training \
--continue_from ~/tesstutorial/trainspecial/special_checkpoint \
--traineddata ~/tesstutorial/trainspecial/chi_sim/chi_sim.traineddata \
--model_output ~/tesstutorial/specialoutput/chi_sim_special.traineddata
```
继续训练
如果合并后测试的结果不够理想，可以利用以下命令继续训练

* fine tuning继续训练
```bash
lstmtraining --model_output ~/tesstutorial/trainspecial/special \
  --continue_from ~/tesstutorial/trainspecial/special_checkpoint \
  --traineddata ~/tesstutorial/trainspecial/chi_sim/chi_sim.traineddata \
  --old_traineddata tessdata/best/chi_sim.traineddata \
  --train_listfile ~/tesstutorial/trainspecial/chi_sim.training_files.txt \
  --max_iterations 10000
```
* scratch 继续训练
```bash
export SCROLLVIEW_PATH=$PWD/java

lstmtraining --debug_interval 100 \
--traineddata ~/tesstutorial/trainspecial/chi_sim/chi_sim.traineddata \
--net_spec '[1,36,0,1 Ct3,3,16 Mp3,3 Lfys48 Lfx96 Lrx96 Lfx256 O1c111]' \
--model_output ~/tesstutorial/specialoutput/base --learning_rate 20e-4 \
--train_listfile ~/tesstutorial/trainspecial/chi_sim.training_files.txt \
--eval_listfile ~/tesstutorial/evalspecial/chi_sim.training_files.txt \
--continue_from ~/tesstutorial/specialoutput/base_checkpoint \
--max_iterations 10000 &>~/tesstutorial/specialoutput/basetrain.log
```
注意：这里的max_iterations的取值要大于第一次的训练值。例如，本次的max_iterations 10000大于3600。


