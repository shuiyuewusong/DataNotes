# 实验6-全服务器备份和还原方案i2FFO

## 目标

- 完成文件级整机备份和恢复
- 完成块级整机备份和恢复

## 环境

- 工作机选择vm1 CentOS7.6操作系统（包含Oracle数据库），建议使用考试中心提供的虚拟机模板创建。
- 灾备机选择vm2 CentOS7.6操作系统，建议使用考试中心提供的虚拟机模板创建。
- 工作机系统安装i2node（需加载sfs复制驱动以及块复制驱动）
- 灾备机需要安装i2node和i2vp_plugin，上传CentOS.vmdk到指定位置(i2FFO实验附件)，是文件级备份的前置要求。
- 下载livecd引导镜像，此步骤是块级备份的前置要求
- 登录控制台，注册工作机和灾备机，授权ffo许可。

## 文件级整机备份要求

- 创建全服务备份任务，"源类型“选择“文件”，目标类型”选择“vmdk” ，非备端拉起，保存到vm2
- 启用“镜像设置> 运到错误写入日志文件继续同步”选项- 备份策略：添加按周计划，每周六凌晨0：00全备，每天中午12点，做增量备份，保留2周【2分】
- 提交备份规则后，手动执行全备，查看备份点。【2分】
- 模拟工作机vm1故障（删除数据库/u01 目录，模拟Oracle数据库故障），建议提前对原虚拟机快照以便回滚。【2分】
- 创建全服务器恢复任务，从已有的备份点进行原机vm1恢复【2分】- 原机恢复完成且系统重启后，验证Oracle可用性【2分】


