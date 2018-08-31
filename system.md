# system

## centos
### centos 7
Initial setup of CentOS Linux 7 (core)     
```
输入“1”，按Enter键 
输入“2”，按Enter键 
输入“q”，按Enter键 
输入“yes”，按Enter键 
```

## windows server 2016
### 远程桌面
* 首先需要先安装远程桌面服务 
```
服务器角色->远程桌面服务->远程桌面会话主机
gpedit.msc->计算机配置–>管理模板—>windows组件—>远程桌面服务->远程桌面会话主机->连接->限制连接的数量->已启用->5 
```

* 120天后修改注册表
```
HEKY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\GracePeriod
Only leave the default and reboot
右键“权限”—>“高级”->更改所有者，权限改为完全控制即可。
```
## win10
远程桌面"发生身份验证错误。要求的函数不受支持"
```
gpedit.msc->本地计算机策略->计算机配置->管理模板->系统->凭据分配->加密Oracle修正->已启用->保护级别->易受攻击
```

## win10 ubuntu
### install
启用或关闭Windows功能->适用于Linux的Windows子系统
microsoft store->ubuntu
### xming
xlaunch->multiple windows->display number:1
```
echo "export DISPLAY=:1.0" >> ~/.bashrc
export DISPLAY=:1.0
sudo apt-get install xfce4-terminal
xfce4-ternminal
```
```
sudo apt-get install ttf-wqy-microhei
sudo apt-get install dbus
sudo /etc/init.d/dbus start
sudo apt-get install software-properties-gtk
```
```
sudo apt-get install nautilus
sudo apt-get install gedit
sudo apt-get install firefox
```
## windows
老毛桃
```
虚拟光驱载入ISO
装机工具->install.wim->选路径
```
[用软碟同(UtroISO)制作install.win大于4G的启动盘](https://blog.csdn.net/he_qiao/article/details/44571339)
```
【解决 U盘安装Windows Server 2012 R2 报错 Windows 无法打开所需的文件 Sources\install.wim】
报错原因：
使用UltraISO等软件刻录镜像时默认使用FAT32文件系统，该系统不支持大于4G的文件，
而Server 2012 R2的安装文件install.wim为5.12G，固安装失败。

解决方法：
按照以前的方法刻录镜像到U盘；
更改U盘文件系统：
进入命令行模式，输入 convert f: /fs:NTFS （F盘为我的U盘所在盘符）

然后打开镜像文件，找到Sources目录下的install.wim文件，复制到对应的U盘目录下。
```

```
wintogo
VHDX
UEFI+GPT
```


### cmd
CMD获取当前目录的绝对路径
```
@echo off
echo 当前盘符：%~d0
echo 当前盘符和路径：%~dp0
echo 当前批处理全路径：%~f0
echo 当前盘符和路径的短文件名格式：%~sdp0
echo 当前CMD默认目录：%cd%
echo 目录中有空格也可以加入""避免找不到路径
echo 当前盘符："%~d0"
echo 当前盘符和路径："%~dp0"
echo 当前批处理全路径："%~f0"
echo 当前盘符和路径的短文件名格式："%~sdp0"
echo 当前CMD默认目录："%cd%"
pause
```

### 开机启动
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp

### FAQ

[如何用UltraISO制作大于4G文件的光盘映像可启动U盘](https://www.jb51.net/softjc/315081.html)

## ubuntu
### 网络配置
```
$ sudo gedit /etc/network/interfaces

# The loopback network interface
auto lo   
iface lo inet loopback

# The primary network interface 
auto eth0
iface eth0 inet static
address 192.168.101.166
netmask 255.255.255.0
gateway 192.168.101.1

$ sudo gedit /etc/resolv.conf

nameserver 114.114.114.114
nameserver 192.168.4.5

$ sudo /etc/init.d/networking restart
```
换源
```
sudo vim /etc/apt/sources.list
```
a start job is running for Raise network interface (5min 3s)
```
sudo vim /etc/systemd/system/network-online.targets.wants/networking.service
TimeoutStartSec=5min
to:
TimeoutStartSec=30sec
```
### 桌面系统
```
sudo apt-get install ubuntu-desktop
```
### 远程桌面
安装xrdp
```
sudo apt-get install xrdp
```
安装vnc4server
```
sudo apt-get install vnc4server
```
安装xfce4
```
sudo apt-get install xubuntu-desktop
echo "xfce4-session" >~/.xsession
sudo service xrdp restart
```

### 安装盘
ultraiso
```
文件->打开(iso)->启动->写入硬盘镜像->写入方式(USB-HDD)->便捷启动(写入新的驱动器引导扇区|Syslinux)->写入
```

### Boot Repair
[Ubuntu 14.04 引导修复（Boot Repair）（双系统修复一）](https://blog.csdn.net/piaocoder/article/details/50589667)
```bash
# try ubuntu
sudo add-apt-repository ppa:yannubuntu/boot-repair && sudo apt-get update
sudo apt-get install -y boot-repair && boot-repair
# Recommended repair
```


