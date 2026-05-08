下文中，将安装的系统（wsl，linux，虚拟机）统一称为 wsl
为防止歧义，wsl 终端称为 linux终端

## 安装wsl
### 安装
```bash
wsl --install # 使用这个指令会列出可用的linux系统
wsl --install -d ubuntu-22.04    # 这个指令会安装lts（稳定版）ubuntu22.04
```
安装完成后会自动打开linux终端

### 配置用户名和密码
安装后，第一次打开，会提示需要输入用户名和密码
<img width="960" height="480" alt="image" src="https://github.com/user-attachments/assets/d5da84c7-03b4-4c0d-b718-c0878f61fd43" />

### 初始化 root 用户密码
使用 vs 自动安装的 wsl 可能不会在启动时要求配置用户和密码
此时使用 su 指令切换 root 用户时，会提示 su: Authentication failure 验证错误

使用管理员权限打开 终端，执行指令
```bash
ubuntu config --default-user root

ubuntu

输入两次 自定义密码，即可初始化 root 密码
```
<img width="1628" height="879" alt="image" src="https://github.com/user-attachments/assets/1ac6d699-73d4-4bab-ba6e-9d6109c9d41e" />

## 安装过程中遇到的错误
### 0x80070003
适用于 Linux 的 Windows 子系统只能在系统驱动器（通常是 C: 驱动器）中运行。 
请确保分发版存储在系统驱动器上：打开“wsl设置”->“系统”-->“存储”-> “更多存储设置”： 更改新内容的保存位置”

### 0x80070003 或错误 0x80370102
请确保在计算机的 BIOS 内已启用虚拟化。
有关如何执行此操作的说明因计算机而异，并且很可能在 CPU 相关选项下。
WSL2 要求 CPU 支持二级地址转换 (SLAT) 功能，后者已在 Intel Nehalem 处理器（Intel Core 第一代）和 AMD Opteron 中引入。 
即使成功安装了虚拟机平台，旧版 CPU（例如 Intel Core 2 Duo）也无法运行 WSL2。

### WslRegisterDistribution failed with error: 0x800701bc

#### 报错原因：
WSL版本由原来的WSL1升级到WSL2后，内核没有升级

#### 解决：
在下面链接中安装适用于 x64 计算机的最新 WSL2 Linux 内核更新包即可
https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

### 0x8000000d
可能是重复安装，卸载所有 wsl 与安装的系统（参考下面：卸载）
WslRegisterDistribution failed with error: 0x8007019e

#### 报错原因：
windows功能没有启用

#### 解决方法：
在电脑设置中，搜索 控制面板
<img width="1387" height="705" alt="image" src="https://github.com/user-attachments/assets/f8244bb6-4c60-432b-9cdd-a3627ed7bf32" />
程序和功能-启用或关闭Windows功能
<img width="1012" height="609" alt="image" src="https://github.com/user-attachments/assets/da7c78f0-b116-43c0-a932-1369b01761ed" />
勾选：适用于 linux的 windows子系统
然后重启电脑
<img width="618" height="579" alt="image" src="https://github.com/user-attachments/assets/52a3394a-34c6-4808-8fed-7ba3bd4d45da" />

### 尝试升级时出错：Invalid command line option: wsl --set-version Ubuntu 2
请确保已启用适用于 Linux 的 Windows 子系统，
并且你使用的是 Windows 内部版本 18362 或更高版本。 
若要启用 WSL，请在 PowerShell 提示符下以具有管理员权限的身份运行此命令：Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux。

### 由于虚拟磁盘系统的某个限制，无法完成所请求的操作。虚拟硬盘文件必须是解压缩的且未加密的，并且不能是稀疏的
取消选中“压缩内容”（如果已选中“加密内容”，请一并取消选中），方法是打开 Linux 发行版的配置文件文件夹。 它应位于 Windows 文件系统上的一个文件夹中，类似于：USERPROFILE%\AppData\Local\Packages\CanonicalGroupLimited...
在此 Linux 发行版配置文件中，应存在一个 LocalState 文件夹。 右键单击此文件夹可显示选项的菜单。 选择“属性”>“高级”，然后确保未选择（未勾选）“压缩内容以节省磁盘空间”和“加密内容以保护数据”复选框。
果系统询问是要将此应用到当前文件夹还是应用到所有子文件夹和文件，请选择“仅此文件夹”，因为你只是要清除压缩标志。 完成此操作后，wsl --set-version 命令应正常工作。

### 无法将词语“wsl”识别为 cmdlet、函数、脚本文件或可运行程序的名称
请确保已安装“适用于 Linux 的 Windows 子系统”可选组件。 此外，如果你使用的是 ARM64 设备，并从 PowerShell 运行此命令，则会收到此错误。 请改为从 PowerShell Core 或从命令提示符运行 wsl.exe。

### 错误：此更新仅适用于装有适用于 Linux 的 Windows 子系统的计算机
若要安装 Linux 内核更新 MSI 包，需要 WSL，应先启用它。 如果失败，将看到以下消息：This update only applies to machines with the Windows Subsystem for Linux。
出现此消息有三个可能的原因：
你仍使用旧版 Windows，不支持 WSL 2。 有关版本要求和要更新的链接，请参阅步骤 #2。
未启用 WSL。 需要返回到步骤 #1，并确保在计算机上启用了可选的 WSL 功能。
启用 WSL 后，需要重新启动才能使其生效，请重新启动计算机，然后重试。

### 错误：WSL 2 要求对其内核组件进行更新
如果 %SystemRoot%\system32\lxss\tools 文件夹中缺少 Linux 内核包，会遇到此错误。 若要解决此问题，请在安装说明的步骤 #4 中安装 Linux 内核更新 MSI 包。 可能会需要从添加或删除程序卸载 MSI，然后重新安装。

## 配置wsl
镜像源等设置

### 配置镜像源（可选）
```bash
cp /etc/apt/sources.list /etc/apt/sources.list.cp    # 备份配置文件
vim /etc/apt/sources.list # 编辑配置文件（vim的使用请百度）
```
将文件内容修改为：（清华镜像）
```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ jammy-proposed main restricted universe multiverse
```
也可以不改镜像源，但是每次使用wsl 都需要翻墙
```bash
# 更新安装工具
sudo apt update
sudo apt-get update

# 如果这里报错，请自行百度镜像，或改回原镜像，使用梯子继续
```

## VS 连接 wsl
VS2022在启动时，其实是默认连接到wsl的（不需要手动连接）
但是没有任何提示
可以查看目标系统，如果出现 wsl:linux 表示连接成功（可能）
<img width="285" height="219" alt="image" src="https://github.com/user-attachments/assets/fc2b3996-dd53-41ed-88bf-63b10fd4964b" />

### 配置 cmake
<img width="343" height="179" alt="image" src="https://github.com/user-attachments/assets/5fd2d50d-9f77-4793-99ee-54148cd4a721" />
打开管理配置，子页面右上角有编辑json
添加下面一段
```bash
{
    "name": "linux-debug",
    "displayName": "Linux Debug",
    "generator": "Ninja",
    "binaryDir": "${sourceDir}/out/build/${presetName}",
    "installDir": "${sourceDir}/out/install/${presetName}",
    "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Debug"
    },
    "condition": {
        "type": "equals",
        "lhs": "${hostSystemName}",
        "rhs": "Linux"
    },
    "vendor": {
      "microsoft.com/VisualStudioRemoteSettings/CMake/1.0": {
        "sourceDir": "$env{HOME}/project/cpp/poor/$ms{projectDirName}",

      }
    }
}
```
•    name: 预设的名称。
•    displayName: 预设的显示名称。
•    generator: 使用的生成器，这里是 Ninja。
•    binaryDir: 构建输出目录。
•    installDir: 安装目录。
•    cacheVariables: CMake 缓存变量，这里设置了 CMAKE_BUILD_TYPE 为 Debug。
•    condition: 条件，只有在 hostSystemName 为 Linux 时才使用此预设。
•    vendor: 特定于供应商的设置，这里是 Visual Studio 的远程设置。
•    sourceDir: 远程主机上的源代码目录。

## 配置过程中遇到的错误
### 检测到 localhost 代理配置，但未镜像到 WSL。NAT 模式下的 WSL 不支持 localhost 代理。

#### 错误现象：
启动终端后，提示上面的错误，但是可以正常使用

#### 解决：
在 C盘/user/[user_name]/ 文件夹中寻找 .wslconfig.txt 文件
如果没有这个文件，可以自己创建一个
将文件内容修改为
```bash
[experimental]
autoMemoryReclaim=gradual  # gradual  | dropcache | disabled
networkingMode=mirrored
dnsTunneling=true
firewall=true
autoProxy=true
```
使用管理员权限，打开windows power shell，执行
```bash
wsl --shutdown
```
然后重新打开 linux终端 即可

如果是自己新建 .wslconfig.txt 文件，然后打开linux终端发现没有生效。
可以在 wsl settings 软件中修改网络模式为 mirrored
重新打开 wsl 即可

### VS连接不到 wsl
首先可以明确，VS自动会连接到 wsl（不通过ssh）进行编译。执着于 ssh连接主要是因为 可以在VS的终端下直接启用 linux 终端（所以下面的内容可以跳过）
其他教程一般会让在 工具-选项-跨平台 中使用 进行连接
下面是我的 wsl启动提示，可以看到我的 IP地址为 192.168.43.74，端口为22（ssh默认端口），用户名和密码为前面设置的 用户名和密码
<img width="1210" height="712" alt="image" src="https://github.com/user-attachments/assets/e084cac0-64c0-4a86-a14f-f6cda6a21457" />
但是我在连接时，总是提示 ip地址 或 端口错误

#### 排查连接错误
百度后排查了一通防火墙，配置文件（无用步骤，仅供参考）
配置 wsl中 /etc/ssh/sshd_config 文件：
```bash
sudo vim /etc/ssh/sshd_config
sudo 

# 将文件中的下面三项注释取消
port 22    # 设置连接端口为22，如果注释掉，表示使用默认端口22
AllowTcpForwarding yes    # 允许所有tcp转发
PermitRootLogin yes     #允许root认证登录
PasswordAuthentication yes     #允许密码认证
```
配置防火墙：
windows防火墙：
在 设置-windows安全中心-防火墙和网络保护-高级设置
入站规则-新建规则
<img width="1558" height="393" alt="image" src="https://github.com/user-attachments/assets/46ae0ff9-bb1b-48b4-a3c4-58198faec5fa" />
端口
<img width="1067" height="807" alt="image" src="https://github.com/user-attachments/assets/8ddf1f2a-68bf-45a4-949e-13be5bf897a3" />
添加 ssh端口：22
<img width="1058" height="513" alt="image" src="https://github.com/user-attachments/assets/147ae4c8-3e32-4536-8282-b1015f6330ca" />
运行连接
<img width="1035" height="507" alt="image" src="https://github.com/user-attachments/assets/20aa38c9-afbc-4460-b0bf-684f848ce14e" />
配置文件默认，名称和备注自定义即可
wsl linux防火墙配置
```bash
# 开放端口
sudo ufw allow 22/tcp

# 重启防火墙
sudo ufw reload

# 删除防火墙规则（如果无效，建议删除规则）
sudo ufw delete allow 22/tcp

# 检查防火墙状态（规则）
sudo ufw status
```
防火墙关闭以后，其实是可以在 powerShell中 使用 下面的指令连接到wsl的
```bash
ssh rhfgxg@127.0.0.1 -p 22
```
而且VS 中 工具-选项-跨平台，也可以连接成功
但是感觉不太靠谱，就没用这种方法

## 安装与配置工具
一些选装的工具会标出
```bash
# 网络测试 curl
# git
sudo apt install curl wget git ca-certificates build-essential net-tools -y
# g++，gdb，make，zip
# rsync 远程文件传输工具
sudo apt install g++ gdb make ninja-build rsync zip -y
# cmake
sudo apt install cmake

# docker容器（选）
```
### 配置 rsync 远程文件传输工具
VS在使用wsl编译时，需要用到 此工具
如果没有此工具，VS 编译时提示：
```bash
1> 正在将文件复制到远程计算机。 
1> 开始将文件复制到远程计算机。 
1> [rsync] rsync -t --delete -v -r --exclude=.vs --exclude=.git --exclude=out --exclude=build -8  "." rsync://rhfgxg@localhost:40826/-home-rhfgxg-.vs-CMakeProject1 
1> [rsync] rsync: [sender] failed to connect to localhost (::1): Connection timed out (116) 
1> [rsync] rsync: [sender] failed to connect to localhost (127.0.0.1): Connection timed out (116) 
1> [rsync] rsync error: error in socket IO (code 10) at clientserver.c(139) [sender=3.3.0] 
1> 复制时出错。有关疑难解答，请参阅 https://aka.ms/AA23jat。
```

#### 检查 rsync 是否安装
```bash
rsync --version    # 检查工具是否安装，如果已经安装，将输出工具版本等信息

# 如果没有安装，安装：
sudo apt-get install rsync
```

#### 编辑/etc/default/rsync
```bash
# 打开rsync
sudo vim /etc/default/rsync

# 编辑rsync，修改下面行
RSYNC_ENABLE=true
```

#### 创建 /etc/rsyncd.conf,并填写配置信息
```bash
# 此文件默认不存在，使用vim创建文件
sudo vim /etc/rsyncd.conf

# 添加文件内容如下
max connections = 2
log file = /var/log/rsync.log
timeout = 300
Charset = UTF-8
use chroot = yes
# port 123    # 修改默认端口，rsync默认监听 873端口

[share] # 模块名
comment = Public Share
# path为需要同步的文件夹路径
path = /home/share
read only = no
list = yes
uid = root
gid = root
# 必须和 rsyncd.secrets中的用户名对应
auth users = user
secrets file = /etc/rsyncd.secrets

 [share2] # 模块2
```

#### 创建/etc/rsyncd.secrets，配置用户名和密码
```bash
# 创建文件
sudo vim /etc/rsyncd.secrets

# 修改文件内容如下
user:password
```

#### 修改rsyncd.secrets文件的权限
```bash
sudo chmod 600 /etc/rsyncd.secrets
# 仅创建者，可以读写，不可以运行
```

#### 启动/重启rsync服务
```bash
sudo /etc/init.d/rsync restart
```
现在 rsync 就可以正常启动了
```bash
# 检查端口占用
sudo lsof -i -P -n | grep LISTEN
```

#### 修改本地 VS 的复制指令
添加 /etc/rsyncd.conf 配置文件后，就无法使用VS的默认项目路径：~/.vs/rsync

修改项目的cmake配置：
顶部工具栏-项目-cmake设置，将会打开 CMakePresets.json 文件
重点关注下面部分（远程开发配置）：
官方文档：https://learn.microsoft.com/zh-cn/cpp/build/cmake-presets-json-reference?view=msvc-170#visual-studio-remote-settings-vendor-map
编译时，目标系统选择wsl，编译配置选择 Linux Debug（下面的 displayName 选项）
```bash
{
    "name": "linux-debug",
    "displayName": "Linux Debug",
    "generator": "Ninja",
    "binaryDir": "${sourceDir}/out/build/${presetName}",
    "installDir": "${sourceDir}/out/install/${presetName}",
    "cacheVariables": {
        "CMAKE_BUILD_TYPE": "Debug"
    },
    "condition": {
        "type": "equals",
        "lhs": "${hostSystemName}",
        "rhs": "Linux"
    },
    "vendor": {
      "microsoft.com/VisualStudioRemoteSettings/CMake/1.0": {
        "sourceDir": "$env{HOME}/project/cpp/poor/$ms{projectDirName}",
        "rsyncCommandArgs": [
          "-t",
          "--delete",
          "--delete-excluded",
          "-v",
          "-r",
          "--exclude=.vs",
          "--exclude=.git",
          "--exclude=out",
          "--exclude=build",
          "-8",
          "--port=873"
        ]
      }
    }
}
```
--port = 873
```bash
•    name: 预设的名称。
•    displayName: 预设的显示名称。
•    generator: 使用的生成器，这里是 Ninja。
•    binaryDir: 构建输出目录。
•    installDir: 安装目录。
•    cacheVariables: CMake 缓存变量，这里设置了 CMAKE_BUILD_TYPE 为 Debug。
•    condition: 条件，只有在 hostSystemName 为 Linux 时才使用此预设。
•    vendor: 特定于供应商的设置，这里是 Visual Studio 的远程设置。
•    sourceDir: 远程主机上的源代码目录。
•    rsyncCommandArgs：传递给 rsync工具的参数（参数解释参考上面）

# rsync 指令
rsync -t --delete -v -r --exclude=.vs --exclude=.git --exclude=out --exclude=build -8 

--port = 873 . rsync://rhfgxg@localhost:873/CMakeProject1
•    rsync：用于同步文件和目录的工具。
•    -t：保留文件的修改时间。
•    --delete：删除目标目录中源目录没有的文件。
•    -v：详细模式，显示传输的文件信息。
•    -r：递归复制目录及其内容。
•    --exclude=.vs：排除 .vs 目录。
•    --exclude=.git：排除 .git 目录。
•    --exclude=out：排除 out 目录。
•    --exclude=build：排除 build 目录。
•    -8：指定使用 UTF-8 编码。
•    --port = 873：指定复制时连接服务器的端口，当与 user@localhost:123 同时存在时，会被 123端口 覆盖
•    . ：复制当前目录。
•    rsync://rhfgxg@localhost:873/CMakeProject1：到目标地址，使用 rsync 协议将文件复制到 localhost 上的 873 端口，并放置在 CMakeProject1 目录中。
•    CMakeProject1：实际上是前面 /etc/rsyncd.conf 配置文件中配置的模块名，将复制到模块指定的目录中
```

## 编译项目，验证结果
1.新建一个cmake项目（防止其他环境配置干扰 编译结果）
2.目标系统选择 wsl
<img width="285" height="219" alt="image" src="https://github.com/user-attachments/assets/333d8556-9235-4fae-a1b1-8de42f7f2688" />
3.cmake 配置选择linux Debug
<img width="343" height="179" alt="image" src="https://github.com/user-attachments/assets/7787da5a-f90a-451f-8105-5461bb27a571" />
4.编译项目

## 可能出现的错误
### rsync 复制文件到虚拟机时，总是使用随机端口导致复制出错
```bash
1> 正在将文件复制到远程计算机。 
1> 开始将文件复制到远程计算机。 
1> [rsync] rsync -t --delete --delete-excluded -v -r --exclude=.vs --exclude=.git --exclude=out --exclude=build -8 --port=873 -v -r --exclude=.vs --exclude=.git --exclude=out --exclude=build -8  "." rsync://rhfgxg@localhost:38862/-home-rhfgxg-project-cpp-poor-CMakeProject1 
1> [rsync] opening tcp connection to localhost port 38862 
1> [rsync] rsync: [sender] failed to connect to localhost (::1): Connection timed out (116) 
1> [rsync] rsync: [sender] failed to connect to localhost (127.0.0.1): Connection timed out (116) 
1> [rsync] rsync error: error in socket IO (code 10) at clientserver.c(139) [sender=3.3.0] 
1> 复制时出错。有关疑难解答，请参阅 https://aka.ms/AA23jat。
```
可以发现，我使用 --port=873 指定了端口，但 VS 依然使用随机端口进行复制，而虚拟机对应端口没有开放导致错误

##### 解决方法
网上没有找到准确的解决方法
但是将虚拟机的 防火墙关闭后，可以临时解决问题

### CMake file APIs: download of '/home/rhfgxg/project/cpp/poor/CMakeProject1/out/build/linux-debug/.cmake/api/v1/reply' failed!
从wsl下载此文件夹失败
wsl中的 目标文件夹存在，且权限正常：655

#### 解决方法
1.修改 /etc/ssh/sshd_config 文件：将 下面选项，取消注释，并改为 yes
```bash
AllowTcpForwarding yes
```
在官方论坛找到了这个问题，我尝试了一下，并没有解决


