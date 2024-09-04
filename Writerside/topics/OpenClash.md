# OpenClash

## meta内核官方版本

[https://github.com/MetaCubeX/mihomo](https://github.com/MetaCubeX/mihomo)
[https://clash.wiki/](https://clash.wiki/)

## 参考文档

[](https://wiki.metacubex.one/config/rule-providers/content/)
[](https://clash.wiki/configuration/configuration-reference.html)
## Clash 基础配置

```yaml
# 官方wiki https://clash.wiki/
port: 7890 # HTTP(S) 代理服务端口
socks-port: 7891 # SOCKS5 代理服务端口
redir-port: 7892 # Linux 和 macOS 的透明代理服务端口 (TCP 和 TProxy UDP 重定向)
tproxy-port: 7895 # Linux 的透明代理服务端口 (TProxy TCP 和 TProxy UDP)
mixed-port: 7893 # HTTP(S) 和 SOCKS4(A)/SOCKS5 代理服务共用一个端口
authentication: # 本地 SOCKS5/HTTP(S) 代理服务的认证 账户密码
  - Clash:password

allow-lan: true # 设置为 true 以允许来自其他 LAN IP 地址的连接
# 仅当 `allow-lan` 为 `true` 时有效
# '*': 绑定所有 IP 地址
# 192.168.122.11: 绑定单个 IPv4 地址
# "[aaaa::a8aa:ff:fe09:57d8]": 绑定单个 IPv6 地址
bind-address: "*"

mode: rule # Clash 路由工作模式 # rule: 基于规则的数据包路由 # global: 所有数据包将被转发到单个节点 # direct: 直接将数据包转发到互联网

log-level: info # 默认情况下, Clash 将日志打印到 STDOUT # 日志级别: info / warning / error / debug / silent

ipv6: true # 当设置为 false 时, 解析器不会将主机名解析为 IPv6 地址
external-controller: 0.0.0.0:9090 # RESTful Web API 监听地址
external-ui: "/usr/share/openclash/ui" # 配置目录的相对路径或静态 Web 资源目录的绝对路径. Clash core 将在`http://{{external-controller}}/ui`  中提供服务.
secret: "chun0930" # RESTful API 密钥 (可选) # 通过指定 HTTP 头 `Authorization: Bearer ${secret}` 进行身份验证 # 如果RESTful API在 0.0.0.0 上监听, 务必设置一个 secret 密钥.
# interface-name: en0 # 出站接口名称
# routing-mark: 6666 # fwmark (仅在 Linux 上有效)

# 用于DNS服务器和连接建立的静态主机 (如/etc/hosts) .
#
# 支持通配符主机名 (例如 *.clash.dev, *.foo.*.example.com)
# 非通配符域名优先级高于通配符域名
# 例如 foo.example.com > *.example.com > .example.com
# P.S. +.foo.com 等于 .foo.com 和 foo.com
# hosts:
# '*.clash.dev': 127.0.0.1
# '.dev': 127.0.0.1
# 'alpha.clash.dev': '::1'
```

## profile

```yaml
 profile:
   # 将 `select` 手动选择 结果存储在 $HOME/.config/clash/.cache 中
   # 如果不需要此行为, 请设置为 false
   #当两个不同的配置具有同名的组时, 将共享所选值
   # store-selected: true

   # 持久化 fakeip
   store-fake-ip: false
```

## DNS 配置

此配置配置各种情况使用的DNS服务

```yaml
dns: # DNS 服务设置 # 此部分是可选的. 当不存在时, DNS 服务将被禁用.
  enable: true
  ipv6: true # ipv6: false # 当为 false 时, AAAA 查询的响应将为空
  listen: 0.0.0.0:7874
  default-nameserver: # 这些 名称服务器(nameservers) 用于解析下列 DNS 名称服务器主机名.   # 仅指定 IP 地址
    - 119.29.29.29
    - 119.28.28.28
    - 1.0.0.1
    - 208.67.222.222
    - 1.2.4.8
  enhanced-mode: fake-ip
  fake-ip-range: 198.18.0.1/16 # Fake IP 地址池 CIDR
  # use-hosts: true # 查找 hosts 并返回 IP 记录
  # search-domains: [local] # A/AAAA 记录的搜索域
  fake-ip-filter: # 此列表中的主机名将不会使用 Fake IP 解析   # 即, 对这些域名的请求将始终使用其真实 IP 地址进行响应
    - "*.baidu.com"
    - "*.qq.com"
  nameserver: # 支持 UDP、TCP、DoT、DoH. 您可以指定要连接的端口. # 所有 DNS 查询都直接发送到名称服务器, 无需代理 # Clash 使用第一个收到的响应作为 DNS 查询的结果.
    - 114.114.114.114
    - 119.29.29.29
    - https://doh.pub/dns-query
    - https://dns.alidns.com/dns-query
    - https://1.1.1.1/dns-query
    - tls://dns.adguard.com:853
    # 当 `fallback` 存在时, DNS 服务器将向此部分中的服务器
  # 与 `nameservers` 中的服务器发送并发请求
  # 当 GEOIP 国家不是 `CN` 时, 将使用 fallback 服务器的响应
  fallback:
    - 9.9.9.9
    - 8.8.8.8
    - tls://dns.quad9.net
    - tls://dns.google
    - tls://1.1.1.1
    - https://dns.google/dns-query

```

## proxies 代理配置

此配置主要用于配置连接方式

```yaml
proxies:
  # ss
  # 支持的加密方法:
  #   aes-128-gcm aes-192-gcm aes-256-gcm
  #   aes-128-cfb aes-192-cfb aes-256-cfb
  #   aes-128-ctr aes-192-ctr aes-256-ctr
  #   rc4-md5 chacha20-ietf xchacha20
  #   chacha20-ietf-poly1305 xchacha20-ietf-poly1305
  - name: "ss1"
    type: ss
    server: server
    port: 443
    cipher: chacha20-ietf-poly1305
    password: "password"
    # udp: true
```

## proxy-groups 代理组

此组可以配合 第三方规则联合使用
创建与第三方规则相同的名称即可配置代理并设置代理所走路径

```yaml
proxy-groups:
  - name: China
    type: select
    proxies:
      - DIRECT
  - name: HelloWorld
    type: select
    proxies:
      - ss.ss.top
```

## rules 路由 配置特定ip 走哪个代理组

```yaml
rules:
  - DST-PORT,7895,REJECT
  - DST-PORT,7892,REJECT
  # 用于 IP 规则 (GEOIP, IP-CIDR, IP-CIDR6) 的可选参数 "no-resolve"
  - IP-CIDR,198.18.0.1/16,REJECT,no-resolve
  - DOMAIN-SUFFIX,google.com,HelloWorld
  - DOMAIN-SUFFIX,github.com,HelloWorld
  - DOMAIN-SUFFIX,docker.com,HelloWorld
  - DOMAIN-SUFFIX,x.com,HelloWorld
  - DOMAIN-SUFFIX,twitter.com,HelloWorld
  - MATCH,China
```