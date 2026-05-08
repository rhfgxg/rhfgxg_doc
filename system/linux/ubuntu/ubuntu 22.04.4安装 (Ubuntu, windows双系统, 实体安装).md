适用于ubuntu22.04 lts版本的安装，其他版本可以参考

## Windows相关设置与安装准备
### 下载Ubuntu20.04.4镜像文件
直接选择 ubuntu-20.04.4-desktop-amd.iso , 进行下载即可 ( 没有前缀, 后缀 )
下载之后只有一个 .iso 文件, 注意记下 保存路径
清华大学开源软件镜像站: https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/22.04.02
<img width="1185" height="690" alt="image" src="https://github.com/user-attachments/assets/f02cb66d-0b05-4eff-88ef-a541332f3cec" />

Ubuntu官网: https://ubuntu.com/download/desktop 现在ubuntu官网的速度并不慢，建议到官网下载
选择 ubuntu 22.04.1 LTS （长期支持版本），点击 Download
<img width="720" height="223" alt="image" src="https://github.com/user-attachments/assets/2dd8810f-ed5b-43bb-8445-68322a24ed49" />

### 下载系统盘制作工具, 制作启动盘
用来制作 Ubuntu 的启动u盘
百度网盘链接：https://pan.baidu.com/s/18B5vcbvjDAmanA5QvsWRoA
提取码：1234
打开Rufus制作工具
设备选择 U盘
引导类型选择 手动选择下载的 .iso 文件
其余保持默认
<img width="416" height="511" alt="image" src="https://github.com/user-attachments/assets/71058f7a-03c9-4dec-873a-0f4f320af2b0" />

### 准备 Ubuntu 需要的硬盘空间 单系统跳过此步
在桌面上，点击计算机图标（右键）–> 管理 --> 找到磁盘管理
选择一个分区 右键选择压缩卷, 压缩出一部分空间 ( 注意, 压缩时 1G = 1024Mb )
<img width="594" height="271" alt="image" src="https://github.com/user-attachments/assets/5e68525a-d46a-49a9-98d6-2feff99cba7d" />
磁盘管理
<img width="525" height="361" alt="image" src="https://github.com/user-attachments/assets/b4785601-ce5d-49ad-90ed-861f29f91f0d" />
压缩卷
关机, 正式开始 Ubuntu 的安装

## Ubuntu安装
开机, 同时按快捷键进入 Bios 启动界面
对于如何进入 Bios 看这篇文章: https://zhuanlan.zhihu.com/p/537015084
选择带有 usb字样 或 自己U盘名 的启动项 然后就可以看到以下界面

### 选择系统语言
这里使用英文介绍, 中文的位置与英文相同, 这里会给出中文的大致翻译
在左边, 划到最下方, 选择中文 即可将系统默认语言变成中文,
后面可以在设置中修改系统语言, 比较推荐使用英文, 使用中文可能会有一些乱码和错误, 但是初学者可以使用中文帮助认识
选择 install ubuntu ( 安装Ubuntu )
点击Continue ( Quit 取消安装, Back 返回上一步, Continue 下一步 )
<img width="720" height="540" alt="image" src="https://github.com/user-attachments/assets/17c9adab-77c9-49bd-9c5c-e6bdfe734773" />

### 选择键盘布局
此时我们使用美国键盘布局【English（US）】，点击【Continue】
<img width="720" height="540" alt="image" src="https://github.com/user-attachments/assets/2f3831bc-5a45-44a3-a67a-ac509c6f8887" />

### 选择安装模式
点击 Normal installation ( 完整安装 )
选择 Install third-party software for graphics and Wi-Fi hardware and additional media formats ( 下载一些图片 视频 邮件 等基础工具软件 ) 建议选择
点击【Continue】下一步
<img width="720" height="540" alt="image" src="https://github.com/user-attachments/assets/1046616e-d440-4e35-a5c6-099cb4ca7162" />

### 选择系统分区
第一个是不清理硬盘直接安装 ( 不建议选择 )
第二个是清空整个硬盘, 再进行安装 ( 装单系统的才能选, 会将原有的Windows系统删除 )
第三个是自定义安装 ( 双系统选择这个 )
<img width="720" height="484" alt="image" src="https://github.com/user-attachments/assets/1a5e5f09-cf5b-428a-86d4-f75c5721b6f4" />
选择自定义安装需要配置分区大小
选择 free space 未分配空间
点击加号或鼠标右键， 新建分区
<img width="720" height="387" alt="image" src="https://github.com/user-attachments/assets/5c9c680a-151e-44b8-86ac-3c16a3e55d7b" />

Size：分区大小
Type for the new partition ：分区模式 Primary ( 主分区 ) Logical ( 逻辑分区 )
Location for the new partition： Beginning of this space ( 分区开始 ) End of this space ( 分区结束 )
Use as：分区文件格式
还有一行是挂载点：/ , /boot

<img width="720" height="337" alt="image" src="https://github.com/user-attachments/assets/7007e161-2068-4004-bb3f-edac2aa99327" />

| 分区名 | 推荐大小 | 文件格式 | 挂载点 | 备注 |
|------|------|------|------|------|
| boot 分区 | 1G 左右 | Ext4 | /boot | 有时候会提示未分配 EFI 分区 |
|虚拟内存 swap | 两倍物理内存 | swap | 无 | 交换空间 |
|根目录 | /剩余空间 | Ext4 | / | |

这时需要重新分配分区，把原本 /boot 的文件格式改为EFI，没有挂载点
默认选择 逻辑分区 和 分区起始位置
根目录其实相当于 windows 的此电脑的地位，所有文件和程序都挂在 根目录之下
其他教程有让分出 /home（用户文件夹），/usr（用户程序），/tmp(临时文件) 分区，其实这些都在根目录之下，完全不需要再次划分

### 选择地区
我们选择中国 地址会出现上海
<img width="720" height="540" alt="image" src="https://github.com/user-attachments/assets/a4d2642f-4a2b-48d1-96c7-1f7bd58eb733" />

### 用户信息配置
<img width="720" height="540" alt="image" src="https://github.com/user-attachments/assets/b5440edf-7f65-4c6e-86ca-c62d1964dc33" />

### 安装完成
安装完成后会出现 Restart Now 重启
此时就可以把 U盘 拔下来了
重启时, 还是需要手动进入 Bios 界面, 此时会出现 Ubuntu 选项
<img width="720" height="540" alt="image" src="https://github.com/user-attachments/assets/ca5f161d-8d82-4d08-be8c-e404e10f5bf5" />

## 初始化系统


















