## 连接linux服务器
### 使用xshell生成密钥
打开xshell,点击工具-新建用户密钥生成向导，如图：
<img width="602" height="473" alt="image" src="https://github.com/user-attachments/assets/277460f5-8644-4692-b37f-69c0541c397a" />
这里的密钥类型和密钥长度保持默认，单击下一步：
<img width="601" height="484" alt="image" src="https://github.com/user-attachments/assets/32f816aa-b1f4-4752-b41e-437b8d0e914c" />
继续单击下一步：
<img width="600" height="489" alt="image" src="https://github.com/user-attachments/assets/e597db7d-fa45-4660-9ad4-c9d4475f4ed6" />
输入密钥名称，然后给密钥设置加密的密码，建议设置成复杂密码，这样即使密钥被别人获取，别人也无法使用。因为使用密钥需要输入密码。
点击完成：
这里显示出了刚才生成的密钥。点击关闭。
生成的密钥包括公钥和私钥。需要将其中的公钥上传至服务器。
所以需要导出公钥。
在xshell窗口点击-工具-用户密钥管理者，如图：
双击之前生成的密钥 id_rsa_2048 ：
点击公钥选项卡：
<img width="594" height="548" alt="image" src="https://github.com/user-attachments/assets/bd64a267-4ba0-452c-be1b-3525b37389d4" />
单击保存为文件：
将公钥文件 id_rsa_2048.pub 保存至电脑。

### 设置服务器
将id_rsa_2048.pub公钥文件 上传到 /root/.ssh/ 目录下。
进入/root/.ssh/ 目录，执行以下命令：
```bash
cat 51anidea.pub >> authorized_keys
chown root:root authorized_keys
chmod 600 authorized_keys
```
禁止用用户名密码的方式登陆服务器：
```bash
编辑ssh配置文件
vi /etc/ssh/sshd_config
更改以下内容
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication no #保存退出
重启sshd服务以生效：
service sshd restart
```
### xshell密钥连接
打开xshell,点击文件-新建：
主机一栏填写服务器的ip地址，然后点击左侧的用户身份验证：
然后点击方法一栏的下拉框，选择Public Key，用户名填 root，密码填加密密钥的密码：
点击连接，即可用密钥登陆上服务器。
而用用户名密码的方式将不能登陆服务器。
























