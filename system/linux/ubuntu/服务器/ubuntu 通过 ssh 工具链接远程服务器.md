## 使用 ssh软件 Putty远程链接服务器
```bash
# 启用 Universe存储库
sudo add-apt-repository universe

# 安装 Putty
sudo apt install putty
putty --version  # 查看版本，验证安装成功

# 可以在图形界面启用 putty

# 卸载 putty
sudo apt remove putty

# 如果在图形界面启动，链接后没有弹出终端界面
# 可以使用指令：
sudo putty
```
链接界面
<img width="667" height="588" alt="image" src="https://github.com/user-attachments/assets/68f348de-1dbb-4b40-918c-9c5ce2f00225" />
在 Host Nme(or IP address) 输入服务器的 公网IP
点击 open 链接
首次链接会有安全提示弹窗，点击 Accept 继续
<img width="506" height="348" alt="image" src="https://github.com/user-attachments/assets/f299b34d-22d3-4b21-8d74-35f03f3c4580" />
输入链接服务器的 用户名和密码
<img width="753" height="466" alt="image" src="https://github.com/user-attachments/assets/db0e9a9b-5eaf-4482-8711-f8f77c64935f" />



