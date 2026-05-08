## 操作
跟着网上教程，想要将windows设为默认启动项，然后在linux终端修改了 /boot/grub.cfg 文件
```bash
# 注意下面做法可能有用，但是我电脑遭殃了，请小心使用
sudo vim /etc/defaute/grub    # 修改此文件
sudo update-grub    # 重新生成新的 /boot/grub.cfg

# 或者直接修改 /boot/grub.cfg文件
sudo vim /boot/grub.cfg
```
## 现象
系统一旦开始引导启动 linux系统，就会跳转到 Bios

## 解决方法
### 进入 grub界面
```bash
引导linux时，按着shift键 或者 esc键

联想 tinkbook + ubuntu是按 esc
```

### 之后会进入 grub命令行
在命令行输入 set 查看当前电脑的 root 和 prefix位置
```bash
grub> set
root=hd1,gpt4
prefix=hd1,gpt4/boot/grub
```
输入 ls (hd1,gpt4) 确定 boot位置
参考教程的作者提供另一种方法，输入 ls (hd1,gpt4) + tab键
```bash
grub> ls (hd1,gpt4)
输入linux系统的 根目录 / 下文件
```

### 查看 内核位置
sdb4就是挂载位置：
例如前面查到的 
(hd0,gpt1)就是/dev/sda1，
(hd1,gpt1)就是/dev/sdb1，
(hd2,gpt1)就是/dev/sdc1
/dev/sd** :
hd0就是a, gpt1就是1
hd1就是b，
hd2就是c，gpt3就是3 
```bash
grub> linux /boot/vm + tab键
会列出补全的linux内核版本
vmlinuz /boot/vmlinuz-6.0.0-41-generic vmlinuz-old

指定使用的内核，注意最后需要添加root位置

grub> linux /boot/vmlinuz-6.0.0-41-generic root=/dev/sda1
```

### 设置 initrd文件
```bash
grub> initrd /boot/initrd.img-6.0.0-41.generic
输入与前面相同的版本号
```

### 启动系统
```bash
grub> boot
```

## 注意进入系统之后，将grub配置文件改回
我这里是被修改了 /etc/defaute/grub文件
```bash
sudo vim /etc/defaute/grub
# 修改内容：将此项改为：GRUB_DEFAULT=0
# 修改的是默认启动项的位置，如果你的Ubuntu系统上第一个就改为0

sudo update-grub    # 重新生成 /boot/grub/grub.cfg文件
```
如果 /etc/defaute/grub文件没有问题
则可能是修改了 /boot/grub/grub.cfg文件
```bash
sudo vim /boot/grub/grub.cfg
# 修改内容：将此项改为：set default="0"
# 修改的是默认启动项的位置，如果你的Ubuntu系统上第一个就改为0
```

## 重启进行测试


