# Ubuntu
### 选择构建模式为 Release
<img width="311" height="281" alt="image" src="https://github.com/user-attachments/assets/1a5804e5-e196-4295-8fd9-7636d4366014" />
会在项目文件中生成一个 build-项目名-Desktop_Qt_6_3_2_GCC_64bit-Release 文件夹
有的时候编译后没有生成这个文件，需要在pro文件里面加上一句：QMAKE_LFLAGS += -no-pie，然后重新构建

### 新建一个文件夹
这个文件夹用来放打包后的文件

### 新建两个脚本 .sh
s="nolink">copylib.sh ：用来找lib文件
```bash
#!/bin/bash

LibDir=$PWD"/lib"
Target=$1

lib_array=($(ldd $Target | grep -o "/.*" | grep -o "/.*/[^[:space:]]*"))

$(mkdir $LibDir)

for Variable in ${lib_array[@]}
do
    cp "$Variable" $LibDir
done
```

项目名.sh ：打包后用来运行项目的脚本，名称应该与项目的可执行文件相同：
```bash
#!/bin/sh
appname=`basename $0 | sed s,\.sh$,,`

dirname=`dirname $0`
tmp="${dirname#?}"

if [ "${dirname%$tmp}" != "/" ]; then
dirname=$PWD/$dirname
fi
LD_LIBRARY_PATH=$dirname
export LD_LIBRARY_PATH
$dirname/$appname "$@"
```
<img width="731" height="234" alt="image" src="https://github.com/user-attachments/assets/52720ef8-d41e-4006-bee1-dce1d622f201" />
这个齿轮就是可执行文件

### 复制 qt安装目录里的 platforms文件夹 到 新建文件夹
我的 platforms文件夹路径为：/usr/ lib/ x86_64-linux-gnu/ qt6/ plugins/ platforms
安装方式不同，路径可能不一样，有的在home，有的在opt。。。
（也可以参考路径：~/Qt6.2.3/ 6.2/ gcc_64/ plugins/ platforms ）

### 复制 lib文件到新建文件夹
将 copylib.sh放到 项目文件夹 build-项目名-Desktop_Qt_6_3_2_GCC_64bit-Release 文件夹中
然后执行：
```bash
chmod 777 copylib.sh
./copylib.sh  untitl      // untitl 改成文件夹中的可执行文件名
```
执行完后会生成一个lib文件夹
将这个 lib文件夹和 可执行文件“untitl”复制到新建文件夹中
将 copylib.sh放到 新建文件夹的 platforms文件夹中
然后执行：
```bash
chmod 777 copylib.sh
./copylib.sh  libqxcb.so
```
在 platforms目录也会多出一个 lib文件夹
将这个 lib文件夹复制到 新建文件夹中

### 在别的电脑运行
执行自己创建的脚本 项目名.sh



# windows
### 使用 Release 模式编译
<img width="801" height="382" alt="image" src="https://github.com/user-attachments/assets/f440df0d-3b2b-49de-a289-705444e4accc" />

### 新建一个文件夹A,将Release编译生成的exe文件复制到新建文件夹中
<img width="1305" height="373" alt="image" src="https://github.com/user-attachments/assets/fed604f0-a0e4-49a6-bd88-380d4a8c3f79" />
<img width="1317" height="213" alt="image" src="https://github.com/user-attachments/assets/4d1ac306-a290-4592-b094-aeb166663947" />

### 打开 下载qt时携带的终端工具
在应用列表寻找
如 Q6.4.3(MinGW 7.30 64-bit)
启动后即可使用 windeployqt 指令打包项目文件

### 在打开的工具终端中使用 cd 跳转到新建目录（保存有 exe文件）
```bash
cd D:\project\qt\Release

# 在这个路径下使用windeployqt工具打包
windeployqt 项目名.exe
```
完成之后可以看到文件夹中多了很多程序执行的依赖文件，这里生成的exe已经可以运行了

### 项目包含外部文件或文件夹
在打包exe文件后，需要手动复制相关文件到打包文件夹的exe同级目录
```bash
例如项目添加了静态资源，然后在项目中使用相对路径进行引用（建议不要使用绝对路径）
在编写时，需要把对应文件夹，添加在bulid-debug文件夹
使用相对路径引用时，以build-debug的exe文件为基础
```

### 项目包含第三方库
需要手动寻找第三库的dll文件，然后复制到 exe文件的同级目录

### 使用vcpkg添加的库dll 路径
```bash
vcpkg安装目录/buildtrees/第三方库名/x64-windows-dbg/库名.dll
# 以第三方库minizip为例，vcpkg安装在D:/opt/tools/vcpkg
D:/opt/tools/vcpkg/buildtrees/minizip/x64-windows-dbg/minizip.dll
```

### 确保exe可以运行后，安装Enigma virtual box工具
Enigma virtual box官方链接：https://enigmaprotector.com/en/downloads.html

### 选择下载Enigma virtual box
<img width="1688" height="1172" alt="image" src="https://github.com/user-attachments/assets/c485dc38-f9e3-4124-8644-0020c3f6a904" />

### 修改为中文
<img width="1146" height="796" alt="image" src="https://github.com/user-attachments/assets/ab82a7a0-0266-48c8-bf97-c48db42cdd15" />

### 添加待打包程序
浏览--->打开新建的文件夹，选中.exe文件，打开
会自动添加另一个路径：打包后另存为

### 添加文件
添加--->添加文件夹递归--->选择刚刚新建的文件夹
<img width="1141" height="796" alt="image" src="https://github.com/user-attachments/assets/8d01829c-bed8-4107-a669-2eda5d8d91b0" />
<img width="1148" height="539" alt="image" src="https://github.com/user-attachments/assets/821d6a0b-af9a-437e-b5ec-8f3183a18cce" />

### 选择文件选项
文件选项---->压缩文件（✔）--->确定
<img width="1129" height="775" alt="image" src="https://github.com/user-attachments/assets/272c1ba5-6716-4f95-b8eb-ffe1f75758ff" />

### 点击打包
<img width="1149" height="793" alt="image" src="https://github.com/user-attachments/assets/84486eff-18f5-4538-9a25-d3c75f1895be" />

### 打开新建的文件夹,找到 项目名_boxed.exe文件，这个文件就是最终打包好的可执行文件，可以将其复制到桌面双击运行 












