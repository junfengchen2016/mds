# Computer Vision Annotation Tool (CVAT)

* [opencv/cvat](https://github.com/opencv/cvat)
* [User's guide](https://github.com/opencv/cvat/blob/develop/cvat/apps/documentation/user_guide.md)
* [opencv官方标注工具cvat的安装和使用](https://blog.csdn.net/minstyrain/article/details/82454991)

## INSTALLATION
Install Docker CE or Docker EE from official site
```bash
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -  
sudo apt-key fingerprint 0EBFCD88  
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update   
sudo apt-get install docker-ce
sudo docker run hello-world
```
Install docker-compose (1.19.0 or newer)
```bash
sudo pip install docker-compose
```
Build docker images
```bash
# /home/chenj/tools/opencv_cvat/cvat-0.2.0
sudo docker-compose build
```
Run docker containers
```bash
docker-compose up -d
```
Go to localhost:8080.

## installation
首先克隆仓库
```bash
git clone https://github.com/opencv/cvat
```
安装必须的环境
```bash
sudo apt-get install -y curl redis-server python3-dev python3-pip python3-venv libldap2-dev libsasl2-dev
```
新建两个文件夹logs和keys
```bash
mkdir logs keys
```
安装必须的依赖库
```bash
sudo pip3 install -r cvat/requirements/development.txt
```
准备
```bash
python3 manage.py migrate
python3 manage.py collectstatic
```
新建超级管理员账户，根据提示输入将要新建的管理员账户名、邮箱和密码，注意密码至少8位且不能太简单
```bash
python3 manage.py createsuperuser
```
开启服务器
```bash
python3 manage.py runserver --noreload --nothreading --insecure 127.0.0.1:7000
```
使用谷歌浏览器打开网址(火狐等其他浏览器暂不支持):
```bash
127.0.0.1:7000
```
