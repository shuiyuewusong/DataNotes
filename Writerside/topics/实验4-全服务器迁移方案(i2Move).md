# 实验4-全服务器迁移方案i2Move

## 目标

- 掌握整机迁移要求的二次授权和操作步骤
- 完成文件级整机迁移及割接。
- 完成块级整机迁移及割接。

## 环境

- 工作机选择vm1 CentOS7.6操作系统（包含Oracle数据库），建议使用考试中心提供的虚拟机模板创建。
- 灾备机A（用于文件级迁移实验）选择vm2 CentOS7.6操作系统，建议使用考试中心提供的虚拟机模板创建。
- 灾备机B（用于块级迁移实验），无需预装操作系统，建议自己在虚拟平台部署空白虚拟机，
- 工作机vm1安装i2node软件（需加载sfs复制驱动以及块复制驱动）
- 灾备机vm2 安装i2node软件
- 灾备机B使用livecd引导镜像启动系统，配置网络。 livecd引导镜像下载
  ![image.png](https://raw.githubusercontent.com/shuiyuewusong/Image-storage/main/test/image-2024-05-25-13-13-22-728.png)
- 登录控制台，注册工作机、灾备机A、灾备机B，授权move许可。

## 文件级迁移要求

- 对灾备机A vm2打虚拟机快照
- 访问https://license.info2soft.com/i2/activation.php?atype=bind 对工作机二次授权 （参考附件手册）-
  工作机通过数据库脚本插入数据(参考Oracle数据库增量数据命令 ) 【2分】
- 新建整机迁移规则，工作机->灾备机A vm2，迁移类型选择“文件”，迁移设置 选择 “虚拟机”，其他设置默认 【2分】
- 迁移规则提交后，等规则状态进入“迁移就绪”，点击迁移。【2分】
- 迁移规则状态变为重启就绪，点击重启，等状态变为“完成” 【2分】
- 割接完成后在目标主机vm2验证Oracle数据库 (做Oracle查询操作确认数据库可用） 【2分】
- 拓扑
  ![image.png](https://raw.githubusercontent.com/shuiyuewusong/Image-storage/main/test/image-2024-05-25-13-13-37-470.png)

**备注：**

迁移完成后保留目标主机vm2的环境，后面实验需要复用；建议自己创建快照。