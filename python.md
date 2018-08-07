# python

## install
### python 3.6-ubuntu1604
添加python3.6安装包,并且安装
```bash
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:jonathonf/python-3.6
sudo apt-get update
sudo apt-get install python3.6

cd /usr/bin
sudo rm python3
sudo ln -s python3.6m python3
```
[gnome-terminal 出错 ImportError: cannot import name '_gi'](https://blog.csdn.net/jaket5219999/article/details/78465251)
```bash
gnome-terminal
ls /usr/lib/python3/dist-packages/gi/
cd /usr/lib/python3/dist-packages/gi/
$ sudo cp _gi.cpython-35m-x86_64-linux-gnu.so _gi.cpython-36m-x86_64-linux-gnu.so
$ sudo cp _gi_cairo.cpython-35m-x86_64-linux-gnu.so _gi_cairo.cpython-36m-x86_64-linux-gnu.so
```


安装pip3
```bash
sudo apt-get install curl
curl https://bootstrap.pypa.io/get-pip.py | sudo python3.6
```

## python
```
beautifulsoup4
matplotlib
opencv-python
pandas
pillow
python-gflags
requests
scipy
sklearn
tensorflow
tenforflow-gpu
```

## pip
[http://mirrors.aliyun.com/pypi/simple/](http://mirrors.aliyun.com/pypi/simple/)

pip源
linux的文件在~/.pip/pip.conf
windows在%HOMEPATH%\pip\pip.ini
```
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host=mirrors.aliyun.com
```

## argparse
```
import argparse
if __name__ == '__main__':
    parser = argparse.ArgumentParser()
    parser.add_argument('--text', type=str, default='Test', help='Text')
    parser.add_argument('--port', type=int, default=80, help='Port number')
    parser.add_argument('--enable', type=bool, default=False, help='Enable or not')
    FLAGS, unparsed = parser.parse_known_args()
    print(FLAGS.path_src)
    print(FLAGS.port)
    print(FLAGS.enable)
```

## samples
bytes/string
```
a = b'test'
b = a.decode('utf-8')
c = b.encode('utf-8')
print(a,b,c)
```
string/list
```
a = 'abcde'
b = list(a)
c = ''.join(b)
print(a,b,c)
```
transpose
```
a = [[1, 2, 3], [4, 5, 6]]
b = [[row[i] for row in a] for i in range(len(a[0]))]
```

## os
```
if not os.path.exists(path_dst):
    os.makedirs(path_dst)
filenames = os.listdir(path_src)
for i, filename in enumerate(filenames):
    stem, extension = os.path.splitext(filename)
    if (extension.lower() != '.jpg'):
        continue
```

## base64

## cv2
[http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_tutorials.html](http://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_tutorials.html)
```
img = cv2.imread('bad.jpg', flags) 

r = open('bad.jpg','rb').read() 
img_array = np.asarray(bytearray(r), dtype=np.uint8) 
img = cv2.imdecode(img_array, cv2.CV_LOAD_IMAGE_COLOR) 
```

## PIL
```
import PIL.Image
import PIL.ImageDraw
import PIL.ImageFont
from io import BytesIO

pimg = PIL.Image.open('a.jpg')
pimg.save('a.jpg')

pimg = PIL.Image.open(BytesIO(base64.b64decode(jpg_base64)))
ttfont = PIL.ImageFont.truetype('simsun.ttc', 24)
draw = PIL.ImageDraw.Draw(pimg)
draw.text((10, 10), u'测试', fill=(0xff, 0, 0), font=ttfont)
buffer = BytesIO()
pimg.save(buffer, format='JPEG')
new_jpg_base64 = base64.b64encode(buffer.getvalue())
```

PIL.Image转换成OpenCV格式：
```
import cv2  
from PIL import Image  
import numpy  
  
image = Image.open("plane.jpg")  
image.show()  
img = cv2.cvtColor(numpy.asarray(image),cv2.COLOR_RGB2BGR)  
cv2.imshow("OpenCV",img)  
cv2.waitKey()  
```

OpenCV转换成PIL.Image格式：
```
import cv2  
from PIL import Image  
import numpy  
  
img = cv2.imread("plane.jpg")  
cv2.imshow("OpenCV",img)  
image = Image.fromarray(cv2.cvtColor(img,cv2.COLOR_BGR2RGB))  
image.show()  
cv2.waitKey()  
```

## selenium
```
import cv2
from selenium import webdriver

option = webdriver.ChromeOptions()
option.add_argument('--headless')   

browser = webdriver.Chrome(options=option)    
browser.set_window_size(1200, 4900)   

browser.get(url_html)
bytes = np.asarray(bytearray(browser.get_screenshot_as_png()), dtype="uint8")
mat_image = cv2.imdecode(bytes, cv2.IMREAD_COLOR)
```

## c dll
注意32位/64位的问题
```
import ctypes  

dll = ctypes.cdll.LoadLibrary('test.dll')
dll = ctypes.CDll('test.dll')

dll = ctypes.windll.LoadLibrary('test.dll') 
dll = ctypes.WinDll( 'test.dll' )

a = dll.add(1, 2)  
```

## debug
### visual studio 2017



## FAQ
ModuleNotFoundError: No module named 'markupsafe._compat'
```
重新安装markupsafe这个模块：先不要直接pip install markupsafe安装，因为会发现错误：UnicodeDecodeError: 'utf-8' codec can't decode byte 0xb6。但是这个时候你在pip list就会发现这个markupsafe已经安装好了，所以先pip uninstall markupsafe，确保，确保，确保这个包卸载。 打开python位置:C:\Python36\Lib\site-packages\pip\compat，用文本编辑器（如记事本）打开__init__.py，在第75行return s.decode('utf_8')，把这一行替换为return s.decode('cp936')。这个是pip安装模块经常碰到的错误。改完后保存，退出。再pip install markupsafe就可以正常安装这个包了。
```



