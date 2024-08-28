# SSH

## 在 ubuntu 开启 ssh

在Ubuntu上启用SSH服务通常涉及以下步骤：

### 安装SSH服务（如果尚未安装）

```Bash
sudo apt update
sudo apt install openssh-server
```

### 启动SSH服务： {id="ssh_1"}

```Bash
sudo systemctl start ssh

```

### 使SSH服务在系统启动时自动运行 {id="ssh_2"}

```Bash
sudo systemctl enable ssh
```

### 检查SSH服务状态 {id="ssh_3"}

```Bash
sudo systemctl status ssh

```

（可选）配置SSH服务，编辑 /etc/ssh/sshd_config 文件。

### 重启SSH服务以应用任何配置更改 {id="ssh_4"}

```Bash
sudo systemctl restart ssh
```

### 确保您的防火墙允许SSH连接（默认端口22）

```Bash
sudo ufw allow ssh
```

现在，SSH服务应该在Ubuntu系统上运行，并允许通过SSH协议远程连接。您可以使用ssh
user@your_server_ip从任何支持SSH的客户端连接到您的Ubuntu服务器。