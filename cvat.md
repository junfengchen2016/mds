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
