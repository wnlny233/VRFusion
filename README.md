很好，这一步做完，你的 VPS 安全性会直接提升一个数量级 👍
下面是 macOS → VPS 使用 SSH Key 登录的标准安全做法（一步一步来）。
你现在用的是：
ssh root@176.122.153.111 -p 29633

一、在 macOS 本地生成 SSH Key
在你 Mac 终端本机 执行（不是 VPS 上）：
ssh-keygen -t ed25519
一路回车即可（不用输入密码也可以，想更安全可以设置一个 passphrase）。
生成完成后你会得到：
~/.ssh/id_ed25519        （私钥，绝不能泄露）
~/.ssh/id_ed25519.pub    （公钥，等下要上传）

二、把公钥上传到 VPS
在你 Mac 上执行：
ssh-copy-id -p 29633 root@176.122.153.111
ssh-copy-id -p 29633 root@172.245.79.202
ssh-copy-id -p 22 root@107.175.87.171

输入一次 VPS 密码即可，以后就不用密码了。

三、测试 Key 是否生效
重新连接：
ssh -p 29633 root@176.122.153.111

ssh -p 29633 root@172.245.79.202
如果 不再让你输密码，直接进系统，说明成功。

四、关闭密码登录（非常重要）
在 VPS 上：
vim /etc/ssh/sshd_config
确认 / 修改为：
PermitRootLogin prohibit-password
PasswordAuthentication no
PubkeyAuthentication yes
保存后重启 SSH：
systemctl restart ssh

五、防止你输错 Key 导致锁死（保险）
在你 Mac 上做一个专用配置：
vim ~/.ssh/config
添加：
Host bwg
    HostName 176.122.153.111
    Port 29633
    User root
    IdentityFile ~/.ssh/id_ed25519

Host rn1
    HostName 172.245.79.202
    Port 29633
    User root
    IdentityFile ~/.ssh/id_ed25519

Host rn2
    HostName 107.175.87.171
    Port 22
    User root
    IdentityFile ~/.ssh/id_ed25519

以后直接：
ssh bwg

六、最终安全状态
项目
状态
密码暴力破解
❌ 完全失效
端口扫描
❌ 无意义
自动僵尸扫描
❌ 全部废掉
SSH 登录
✔ 只有你能进
你现在这台 VPS，已经从“公网裸奔”，升级成“银行金库级别”的 SSH 访问安全了 🔐
如果你愿意，下一步我可以顺手帮你把 UFW 规则也一起“锁死成只对你开放”。
