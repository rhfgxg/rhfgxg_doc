## 安装 Windows+Ubuntu 双系统
1. [22.04版本](ubuntu 22.04.4安装 (Ubuntu, windows双系统, 实体安装).md)

## 初始配置
### 初始化 root 用户密码
root 用户密码需要独立进行设置， 在未设置 root 用户之前，可以正常使用 sudo 指令提权，但不能使用 su 指令切换 root 用户

```base
su root        # 切换到 root 用户
未初始化 root 用户时，使用 su 指令的报错如下
su: Authentication failure 

初始化 root 用户密码：
sudo passwd root
```

### 添加用户

```base
# 添加用户 adduser 或者 useradd
# 删除用户 deluser 或者 userdel

# adduser：会自动为创建的用户指定主目录、系统shell版本，会在创建时输入用户密码。
# useradd：需要使用参数选项指定上述基本设置，如果不使用任何参数，则创建的用户无密码、无主目录、没有指定shell版本。

## 使用 adduser 添加用户
sudo adduser tt
## 输出显示：
# [sudo] password for mqk: 
# 正在添加用户"tt"...
# 正在添加新组"tt" (1006)...
# 正在添加新用户"tt" (1006) 到组"tt"...
# 创建主目录"/home/tt"...
# 正在从"/etc/skel"复制文件...
#     输入新的 UNIX 密码： 
#     重新输入新的 UNIX 密码： 
# passwd：已成功更新密码
# 正在改变 tt 的用户信息
# 请输入新值，或直接敲回车键以使用默认值
#    全名 []: 
#     房间号码 []: 
#     工作电话 []: 
#     家庭电话 []: 
#     其它 []: 
# 这些信息是否正确？ [Y/n] y

## adduser 常用参数选项为：
# –home： 指定创建主目录的路径，默认是在/home目录下创建用户名同名的目录，这里可以指定；如果主目录同名目录存在，则不再创建，仅在登录时进入主目录。
# –quiet： 即只打印警告和错误信息，忽略其他信息。
# –debug： 定位错误信息。
# –conf： 在创建用户时使用指定的configuration文件。
# –force-badname： 默认在创建用户时会进行/etc/adduser.conf中的正则表达式检查用户名是否合法，如果想使用弱检查，则使用这个选项，如果不想检查，可以将/etc/adduser.conf中相关选项屏蔽。如：

## useradd
sudo useradd tt
# 在使用useradd命令创建新用户时，不会为用户创建主目录，不会为用户指定shell版本，不会为用户创建密码。

## 为用户 初始化或修改 登录密码：
sudo passwd tt
## 输出显示：
# 输入新的 UNIX 密码： 
# 重新输入新的 UNIX 密码： 
# passwd：已成功更新密码

## 为用户指定命令解释程序(通常为/bin/bash)：
sudo usermod -s /bin/bash tt
## 为用户指定用户主目录：
sudo usermod -d /home/tt tt

## useradd 常用参数选项：
# -d： 指定用户的主目录
# -m： 如果存在不再创建，但是此目录并不属于新创建用户；如果主目录不存在，则强制创建； -m和-d一块使用。
# -s： 指定用户登录时的shell版本
# -M： 不创建主目录
```

### 删除用户

```base
## deluser 方法
## 只删除用户：
sudo deluser tt
# 输出显示：
# 正在删除用户 'tt'...
# 警告：组"tt"没有其他成员了。
# 完成。

## 连同用户的主目录和邮箱一起删除：
sudo deluser --remove-home tt
# 输出显示：
# 正在寻找要备份或删除的文件...
# 正在删除文件...
# 正在删除用户 'tt'...
# 警告：组"tt"没有其他成员了。
# 完成。

## 连同用户拥有的所有文件删除：
sudo deluser --remove-all-files tt

## userdel 方法 只删除用户：
sudo userdel tt

## 连同用户主目录一起删除：
sudo derlser -r tt
# 如果创建时主目录已经存在，即主目录不属于当前要删除的用户，则无法删除主目录。
```

### 添加中文输入法
安装 fcitx （这是百度和搜狗输入法的依赖）
```base
# 安装Fcitx输入框架
sudo apt-get install fcitx

# 在 设置-区域与语言-管理已安装语言 中的 键盘输入法系统 选择 fcitx
# 设置 fcitx 开机自启动
sudo cp /usr/share/applications/fcitx.desktop /etc/xdg/autostart/

# 安装 fcitx 依赖
sudo apt install libqt5qml5 libqt5quick5 libqt5quickwidgets5 qml-module-qtquick2
sudo apt install libgsettings-qt1

# 卸载系统ibus输入法框架
sudo apt purge ibus

```
#### 安装搜狗输入法
```base
# 在官网下载 x86 的安装包，下载后为 .deb 格式

# 进入下载目录，进行安装
# 安装过程中如果有错，运行 sudo apt --fix-broken install 删除安装，重新开始
sudo dpkg -i sougou的文件名.deb

# 如果安装报错：dpkg: error processing package sogoupinyin (–install) :dependency problems - leaving unconfigured
# 安装依赖即可解决
sudo apt-get install -f

# 注销用户，重新进入即可使用
```

#### 使用百度输入法
```base
# 在官网下载安装包，下载后为 .zip 格式 的压缩包
# 进入下载目录，解压
unzip 压缩包.zip
# 得到 .deb 安装包

# 安装
# 安装过程中如果有错，运行 sudo apt --fix-broken install 删除安装，重新开始
sudo dpkg -i 百度的文件名.deb

# 如果安装报错：dpkg: error processing package baidupinyin (–install) :dependency problems - leaving unconfigured
# 安装依赖即可解决
sudo apt-get install -f

# 注销用户，重新进入会跳出引导界面
```

#### 如果进入后依然不能使用
<img width="243" height="543" alt="image" src="https://github.com/user-attachments/assets/aba4b572-79fa-43df-bfb2-f4a5098d2799" />
界面右上角-配置
<img width="594" height="543" alt="image" src="https://github.com/user-attachments/assets/969dc0d2-7eec-4c43-a8b7-3e48d986f176" />
左下角添加 搜狗（百度）输入法即可

### 卸载中文输入法
```base
# 下面只是卸载输入法的命令，并不会卸载 fcitx
# 卸载搜狗输入法
sudo apt-get  purge  sogoupinyin

# 卸载百度输入法
sudo dpkg --purge remove fcitx-baidupinyin:amd64

# 卸载fcitx
sudo apt-get remove fcitx
sudo apt-get remove fcitx-module*
sudo apt-get remove fcitx-frontend*
sudo apt-get purge fcitx* 
```

### 卸载火狐
这里是为了文章分区明确，才把卸载放在在前面，请优先移步软件安装--下载谷歌浏览器

打开 ubuntu Software （ubuntu 应用商店）
右上角搜索火狐
然后卸载
<img width="720" height="165" alt="image" src="https://github.com/user-attachments/assets/fad6c2fe-250c-47d8-8d30-4e5976af5bbf" />
<img width="1226" height="281" alt="image" src="https://github.com/user-attachments/assets/7a48d80d-6af4-4f8e-bc37-1aa61fca5ea8" />

### 更换下载源
打开软件与更新，将下载源切换为国内的阿里云
<img width="227" height="205" alt="image" src="https://github.com/user-attachments/assets/997c9104-8bb6-4f7f-9778-7cc1ed1d60c0" />

### 软件与更新
<img width="720" height="385" alt="image" src="https://github.com/user-attachments/assets/d92c0ea8-e92c-4022-8f49-f35f8be77ff8" />
勾选 installable from CD-ROM/DVD 在 Download from 选择 other
然后找到 china http://mirror.aliyun.com
右下角 添加（ Choose Server ）
然后更新即可

## 软件下载安装
### 下载谷歌浏览器
谷歌浏览器官方页：https://www.google.cn/chrome/
选择下载 .deb 文件
```base
# 安装
sudo dpkg -i 压缩包名.deb
```
<img width="1480" height="350" alt="image" src="https://github.com/user-attachments/assets/1602b1f0-7693-47c5-b717-235c954610ae" />
设置地址栏使用的搜索引擎
<img width="1226" height="281" alt="image" src="https://github.com/user-attachments/assets/41aa38aa-1518-4582-914f-9f9c0ecd9519" />
设置启动页
<img width="1065" height="419" alt="image" src="https://github.com/user-attachments/assets/ffeec7a0-3fd2-493d-bbb6-47e1fb4534ba" />
设置默认下载路径

### 下载油猴插件（谷歌）
油猴插件安装包：https://wmhl.lanzoui.com/ib8glab/
选择普通下载
```base
# 使用 zip 解压安装包
sudo apt-get install unzip                   # 下载 zip 的解压软件
unzip ~/Downloads/Tampermonkey\ V4.9.zip     # 解压安装包
```
打开谷歌浏览器，设置（Settings）打开扩展程序（Extensions）
右上角，打开开发者模式（Developer mode）
解压后，选择 .crx 的文件，直接拖到谷歌浏览器，等待安装
<img width="720" height="147" alt="image" src="https://github.com/user-attachments/assets/8f921b7c-f2a5-46d4-8f7d-ffa0dbc14a0f" />
扩展程序

### 下载和使用 git
git 的使用放在了另一篇文章
github使用与报错
安装 git
```base
# 检查
git --version
# 无输出表示没有安装

sudo apt-get update
sudo apt-get install git
```

### 下载 MySql
```base
sudo apt update 
sudo apt install -y  mysql-server    # 安装 MySql

sudo mysql -u root   # 进入MySql  只使用这一次，以后使用 mysql -u root -p 然后输入密码 进入Mysql

# 更新 Mysql用户密码，这就是以后登陆 Mysql 的密码
# 注意，需要在 Mysql 中执行
# > 表示在 Mysql 中使用的指令
> alter user 'root'@'localhost' identified with mysql_native_password by 'your_new _password';

# 在任何用户下 登陆 Mysql
mysql -u root -p
# 输入密码

# 退出 Mysql
> exit
```
以后可以在终端直接进入 MySql，没有 可视化窗口
[MySql介绍]()

### 下载 MySql 可视化软件（可选）
```base
sudo apt-get install MySQL-workbench   
# 这个指令现在不能安装了
# 会报错：找不到软件包
```
官网：https://dev.mysql.com/downloads/workbench/
选择自己电脑的版本，ubuntu 下载 .deb 软件包
```base
# 这个是我选择的安装包：mysql-workbench-community_8.0.29-1ubuntu20.04_amd64.deb

# 使用 dpkg 进行安装
sudo dpkg -i mysql-workbench-community_8.0.29-1ubuntu20.04_amd64.deb

# 如果报错 mysql-workbench-community
# 输入以下命令解决问题，然后重复执行上述安装命令
sudo apt -f install
sudo dpkg -i mysql-workbench-community_8.0.29-1ubuntu20.04_amd64.deb
```

### 下载终端文本编辑工具 vim
[vim 的安装与使用]()

### 下载gcc，g++，make
```base
sudo apt update
sudo apt install build-essential

# build-essential 是一个内置的包含gcc，g++，make的工具包
```

### 下载 .deb包管理：dpkg
```base
# 安装软件
cd Downloads   # 进入安装包的下载目录
sudo dpkg -i 软件包名.deb   # 安装

# 卸载软件
# 输入安装后的软件名即可
sudo dpkg -r 软件名
```

### 压缩和解压文件（文件夹）相关工具
#### 打包 tar（不进行压缩）
```base
tar -xvf FileName.tar                    # 解包
tar -cvf FileName.tar DirName           # 将DirName和其下所有文件（夹）打包
```

####  压缩：.gz文件
```base
gunzip FileName.gz                      # 解压1
gzip -d FileName.gz                     # 解压2
gzip FileName                      # 压缩，只能压缩文件

# 使用tar -zxvf 解压某些 .gz文件
# 报错：
# gzip: stdin: not in gzip format
# tar: Child returned status 1
# tar: Error is not recoverable: exiting now
# 原因：不支持 gzip的解压方式，去掉z
tar -xvf FileName
```

#### 压缩：.tar.gz 和 .tgz
```base
tar -zxvf FileName.tar.gz                       # 解压
tar -zcvf FileName.tar.gz DirName               # 压缩文件夹（文件）
tar -C DesDirName -zxvf FileName.tar.gz         # 解压到目标路径
```

#### 压缩：.rar文件
```base
# 下载 rar 解压软件
wget http://www.rarlab.com/rar/rarlinux-x64-5.0.0.tar.gz
tar -zxvf rarlinux-x64-5.0.0.tar.gz
mv rar /opt/
cd /opt/rar/
make && make install

rar x FileName.rar              # 解压
rar a FileName.rar DirName        # 压缩
```

#### 压缩：.zip文件
```base
sudo apt-get install unzip              # 下载 zip 的解压软件
unzip 被解压.zip                  # 解压
zip 解压后.zip 被压缩.txt           # 压缩文件
zip -r FileName.zip DirName         # 压缩文件夹
```

### 文件检索：locate
```base
# 安装工具
sudo apt-get install mlocate
# 创建或更新 slocate/locate 命令所必需的数据库文件
sudo updatedb
```

## 环境配置
### 下载 python 环境
python 源码：https://www.python.org/ftp/python/
```base
wget -c https://www.python.org/ftp/python/版本      # 下载 python 源码
tar xzvf Python-3.9.13.tgz     # 解压源码包
cd Python-3.9.13/
LDFLAGS="-L/usr/lib/x86_64-linux-gnu" ./configure

# 编译 python
make
sudo make install

# 检测安装
python3 --version
```

### 下载 nginx 服务器
```base
# 1、安装
sudo apt update
sudo apt install nginx

# 2、安装完，nginx就默认被启动，通过下面命令查看
sudo systemctl status nginx

# 3、配置防火墙，允许流量通过 HTTP（80）和 HTTPS（443）端口。假设你正在使用UFW,你可以做的是启用 ‘Nginx Full’ profile，它包含了这两个端口：
sudo ufw allow 'Nginx Full'
sudo ufw status（验证是否成功）

# 4、验证nginx是否安装成功
curl http://127.0.0.1

将 nginx启动器 添加到环境变量:
进入 profile文件
# vim /etc/profile 
添加变量如:
   PATH=$PATH:/usr/local/webserver/nginx/sbin  export PATH  
注：PATH 的作用就是定义 nginx 的环境变量，export 则表示启用这个变量的含义  
重新加载配置文件即可:
# source /etc/profile

nginx 中的 rewrite 需要依赖到 pcre 包，所以如果需要使用此功能，则可以安装 pcre 包，步骤如下:  
1、检查是否存在 pcre 环境:  
# pcre-config --version
如果出现 command not found 则开始执行第 2 步，否则跳过  
2、切换到需要存储的路径，如:
# cd /usr/local/src  
3、执行: 
# wget https://zenlayer.dl.sourceforge.net/project/pcre/pcre/8.45/pcre-8.45.tar.gz
4、解压 pcre 包: 
# tar -zxvf pcre-8.45.tar.gz
5、进入解压后的文件夹: 
# cd ./pcre-8.45
6、编译安装: 
# ./configure  
7、执行: 
# make && make install
```

## 其他
### 使用 ssh 上传文件到其他服务器
```base
ssh -l root IP   远程登陆

scp -r 待传文件 root@IP:/home/name
输入服务器密码

上传完成
```

### 使用 ubuntu SSH 连接另一台 ubuntu 服务器
首先在Ubuntu上安装openssh-server
```base
sudo apt install openssh-server
```
安装好以后，ssh server应该已经开始运行了，可以用下面的命令检查ssh server的状态
```base
systemctl status sshd

返回状态是active(running）
```
另外，需要的时候，还可以利用systemctl命令打开(start)/关闭(stop)/重启(restart)ssh server，例如下面的命令就可以用来重启ssh server服务
```base
sudo systemctl restart ssh
```
2. 利用Ubuntu自带的ufw 修改防火墙状态
首先开启防火墙
```base
sudo ufw enable
```
打开传输ssh的端口（默认22）
```base
sudo ufw allow ssh
```
设置ssh server开机启动
```base
sudo systemctl enable ssh
```
3.现在就可以用 ssh username@IP远程连接电脑了
```base
查询IP地址  ： ip a
```
所以ssh连接这台电脑的命令为
```base
ssh root@xx.xxx.184.211
```
4.到上一步为止，其实已经可以实现连接功能了，但是为了安全着想，最好将ssh的端口从默认的22改为另一个大于1024的数字
编辑ssh server配置文件
```base
sudo vim /etc/ssh/sshd_config
Port 2222   #设置ssh的端口号
PermitRootLogin yes   # 可以root远程登录
```
打开设定的ssh端口
```base
sudo ufw allow 2222/tcp
```
重启ssh server
```base
sudo systemctl restart ssh
```
现在，可以连接你的电脑了
```base
ssh -p 2222 root@47.113.184.211
```

## ubuntu 笔记本合盖后再打开会黑屏
修改电源模式
```base
修改配置文件
sudo vim /etc/systemd/logind.conf

关注下面三行内容
HandleLidSwitch=suspend：当笔记本电脑使用电池供电时，合盖挂起
HandleLidSwitchExternalPower=suspend：当笔记本电脑插入电源插座时，合盖挂起
HandleLidSwitchDocked=ignore：当笔记本电脑连接到扩展坞时，合盖忽略

如果需要，你可以根据自己的喜好将这些参数的值更改为其中之一：
suspend：合盖时挂起
lock：合盖时锁定
ignore：什么都不做
poweroff：关机
hibernate：合盖时休眠

# 重启服务
service systemd-logind restart
# 这里会注销用户
# 打开后，我的电脑会进入无界面的 终端
# 强制重启后就能正常使用了

你可以编辑 /etc/systemd/logind.conf 文件，
或者在 /etc/systemd/logind.conf.d 目录中创建一个新文件，
并取消注释上述设置并更改其值。
如果此目录不存在，请创建此目录。
```


