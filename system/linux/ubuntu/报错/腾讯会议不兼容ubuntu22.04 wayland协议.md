Linux 安装腾讯会议了以后，打不开，显示：腾讯会议不兼容 wayland 协议！
```bash
sudo vi /etc/gdm3/custom.conf

# 把 #WaylandEnable=false 的注释去掉

sudo service gdm3 restart
# 刷新
```
