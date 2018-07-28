# redhat-centos

## centos 7
Initial setup of CentOS Linux 7 (core)     
```
输入“1”，按Enter键 
输入“2”，按Enter键 
输入“q”，按Enter键 
输入“yes”，按Enter键 
```

### virtualbox
```
sudo yum install gcc make perl
sudo yum install kernel-devel-3.10.0-327.el7.x86_64
```

### 换源
[https://www.cnblogs.com/dadong616/p/5586697.html](https://www.cnblogs.com/dadong616/p/5586697.html)

* 配置本地yum源
```
sudo mkdir /mnt/cdrom
sudo mount -o loop /dev/cdrom /mnt/cdrom
cd /etc/yum.repos.d
sudo gedit local.repo
```
```
...
[local]
name=local
baseurl=file:///mnt/cdrom
gpgcheck=0
enabled=1
```
这时候本地yum源就完成了。可以试下，yum install pip 是否成功。

* 配置163yum源
/etc/yum.repos.d下新建一个163.repo文件，编辑，内容如下：
```
[163]
name=163
baseurl=http://mirrors.163.com/centos/6/os/x86_64/
gpgcheck=0
enabled=1
```
这里要注意的是，baseurl这一项，你要到http://mirrors.163.com/centos这里，去找到你对应的redhat版本的目录，然后点os、再点x86_64（一般都有）,然后用你地址栏上显示的网址替换上面的baseurl就行了，保存退出。

* 配置epel源
```
rpm -vih http://dl.fedoraproject.org/pub/epel/6/x86_64/Packages/e/epel-release-6-8.noarch.rpm
```
注意，我的redhat是7，所以如果你的版本不是7，那你要到http://dl.fedoraproject.org/pub/epel找到你对应版本的rpm包，然后用上面的命令下载，这条命令的作用就是在/etc/yum.repos.d目录下生成epel源

## 更新GCC版本
编译时提示“error: unknown option after ‘#pragma GCC xxx’”等信息就需要升级GCC
```
#查看当前版本
gcc --version #显示4.7
cd /
wget ftp.gnu.org/gnu/gcc/gcc-7.3.0/gcc-7.3.0.tar.gz
tar -zxvf gcc-7.3.0.tar.gz
cd gcc-7.3.0
#检测和安装相关依赖包，这个过程需要耐心等待(此步骤会将依赖包下载到gcc-7.3.0目录，如果因网络原因无法完成请自行使用wget下载)
./contrib/download_prerequisites
mkdir build
cd build
../configure -enable-checking=release -enable-languages=c,c++ -disable-multilib
#编译过程漫长，请耐心等待
make -j4
make install
#查看版本
gcc --version
```

## sudoers
```
su root
xxxxxx
chmod u+w /etc/sudoers
gedit /etc/sudoers
xxxxxxxx
chmod u-w /etc/sudoers
```
/etc/sudoers
```
...
root	ALL=(ALL) 	ALL
chenj	ALL=(ALL) 	ALL
...
```

## 自动登录
/etc/gdm/custom.conf
```
...
[daemon]
AutomaticLoginEnable=true
AutomaticLogin=chenj
...
```




