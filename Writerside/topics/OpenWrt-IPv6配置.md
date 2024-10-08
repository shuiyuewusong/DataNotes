# OpenWrt IPv6配置

## 在进行拨号后需要配置IPv6 进而获取IPv6 IP需要进行特定配置

## WAN口配置 及 LAN口配置

- 点击 网络
    - 点击 接口
        - 点击 Wan接口 编辑
            - 点击 高级设置
                - 获取Ipv6地址 自动
                - IPv6 分配长度 已禁用
                - 其他参数默认
            - 点击 DHCP服务器
                - 点击 常规设置
                    - 忽略 此接口 勾选
                - 点击 IPV6设置
                    - 指定的主接口 不勾选
                    - RA服务 已禁用
                    - DHCPv6服务 已禁用
                    - NDP代理 已禁用
                    - IPv6前缀有效期 默认: 12h
                    - 遵守IPv4有效期 不勾选
            - 点击 保存
        - 点击 Lan接口 编辑
            - 点击 高级设置
                - 点击IPv6 分配长度 选择为 64 (根据运营商分配长度选择)
                - 其余参数默认
            - 点击 DHCP服务器
                - 点击 IPv6设置
                    - RA服务 服务器模式
                    - DHCPv6服务 服务器模式
                    - NDP代理 设置为禁用
                - 点击 IPv6RA设置
                    - 默认路由器 自动
                    - 启动SLAAC 勾选
                    - RA标记 受管配置 其他配置
                    - 其他参数默认
            - 点击 保存
        - 点击 保存并应用

## DNS解析需要配置 允许IPv6

- 点击网络
    - DHCP/DNS
        - 过滤器
            - 过滤 IPv6 AAAA 记录 不勾选
        - 点击 保存并应用

## 参考资料

[https://blog.csdn.net/a924282761/article/details/129067001](https://blog.csdn.net/a924282761/article/details/129067001)

