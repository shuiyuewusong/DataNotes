# Lets Encrypt

自动获取证书

[官网](https://letsencrypt.org/zh-cn/)

[自动机器人地址](https://certbot.eff.org/)

获取证书目前有两种方式

- 通过80 端口映射文件 自动颁发证书
- 通过 DNS解析文本 映射证书

## 通过DNS解析 映射证书的配置方法

由于 certbot 并不支持 阿里云和腾讯云 所以使用 github 大佬开发的第三方插件

[](https://github.com/ywdblog/certbot-letencrypt-wildcardcertificates-alydns-au)

通过下载大佬的 脚本 并安装 python3

```Bash
# 修改 au.sh 
pythoncmd="/usr/bin/python3"
ALY_KEY="key"
ALY_TOKEN="token"
#保证脚本可以顺利执行

```

## 安装 certbot {id="certbot_1"}

```Bash
sudo snap install --classic certbot
```

## 准备 certbot 命令 {id="certbot_2"}

```Bash
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

## 测试命令执行情况

配置日志的和文件的保存目录 配置脚本的启动目录

- `-d *.ch7.top` 域名
- `--preferred-challenges` 使用的方式
- `--dry-run` 测试运营
- `--config-dir` 证书存储目录
- `--work-dir` 工作缓存目录
- `--logs-dir` 日志目录
- `--manual-auth-hook` 启动前勾子
- `--manual-cleanup-hook` 执行完毕清理勾子
- `"/app/certbot/certbot-aliyun/au.sh python aly add"` 使用au.sh脚本 使用py脚本 使用阿里云 模式 新增 DNS txt解析
- `"/app/certbot/certbot-aliyun/au.sh python aly clean"` 使用au.sh脚本 使用py脚本 使用阿里云 模式 清理 DNS txt解析

### 测试执行

```Bash
certbot certonly  -d *.ch7.top --manual --preferred-challenges dns --dry-run \
    --config-dir "/app/certbot/config" \
    --work-dir "/app/certbot/work" and \
    --logs-dir "/app/certbot/log" \
    --manual-auth-hook "/app/certbot/certbot-aliyun/au.sh python aly add" \
    --manual-cleanup-hook "/app/certbot/certbot-aliyun/au.sh python aly clean"
```

### 最终执行

```Bash
certbot certonly  -d *.ch7.top --manual --preferred-challenges dns \
    --config-dir "/app/certbot/config" \
    --work-dir "/app/certbot/work" \
    --logs-dir "/app/certbot/log" \
    --manual-auth-hook "/app/certbot/certbot-aliyun/au.sh python aly add"  \
    --manual-cleanup-hook "/app/certbot/certbot-aliyun/au.sh python aly clean"
```

### 尝试模拟续订

```Bash
certbot renew --dry-run \
    --config-dir "/app/certbot/config" \
    --work-dir "/app/certbot/work" \
    --logs-dir "/app/certbot/log"
```

日志

```Log
Saving debug log to /app/certbot/log/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /app/certbot/config/renewal/ch7.top.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Simulating renewal of an existing certificate for *.ch7.top

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations, all simulated renewals succeeded: 
  /app/certbot/config/live/ch7.top/fullchain.pem (success)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```

~~默认情况下 certbot 会自动续订证书并更新 证书信息 所以不需要自行配置自动续订设置~~

### 配置自动续订 
添加到crontab里面 定时执行  
```Bash
crontab -e 
# 编辑定时任务 0分 0时 每日 每月 每周 执行任务
0 0 15 * *  sudo certbot renew --cert-name ch7.top --config-dir "/app/certbot/config" --work-dir "/app/certbot/work" --logs-dir "/app/certbot/log" --manual-auth-hook "/app/certbot/certbot-aliyun/au.sh python aly add" --manual-cleanup-hook "/app/certbot/certbot-aliyun/au.sh python aly clean"

```
