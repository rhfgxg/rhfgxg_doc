修改电源模式
```bash
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
