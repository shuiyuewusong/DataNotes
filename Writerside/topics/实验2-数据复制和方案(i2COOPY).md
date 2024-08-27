# 实验2-数据复制和方案i2COOPY

## 目标

− 掌握复制规则、关键参数配置  
− 掌握对关键数据文件的保护  
− 掌握数据一致性的检验  
− 掌握普通文件的原机恢复

## 环境

- 工作机和灾备机安装i2node软件【每个实验开始前记得给自己的vm1/vm2/做好初始快照】
- 登录控制台，注册工作机和灾备机，授权coopy许可

## 拓扑

## 预检步骤

1. 查下sfs是否加载`lsmod |grep sfs `
2. 若未加载，工作机系统手动加载sfs
3. 控制台注册工作机节点时，复制路径一定要指定

  ```Bash
  insmod /usr/local/sdata/modules/sfs3.ko  
  ```

## 步骤及评分要求

- 在源系统vm1_Oracle12c /srcdir1目录下生成1GB数据文件datafile1。

```Bash
dd if=/dev/urandom of=/srcdir1/datafile1 bs=1k count=10000  
```

- 创建复制规则，保护源系统/srcdir1到目标系统vm2_DRServer /dstdir1（路径映射类型为一对一）。

> > 文件实时复制规则配置过程，截图并说明 【1分】

- 复制规则状态从镜像进入复制，确认两端的数据文件datafile1的md5值是否一致。  
  源系统：`md5sum /srcdir1/datafile1`  
  目标系统：`md5sum /dstdir1/datafile1`

> > 复制规则状态镜像完成后，检查目标数据文件的md5值与源数据文件一致。验证数据镜像能力，截图并说明【2分】  
> > 复制规则查看数据流量图，截图并说明【1分】

- 在源系统/srcdir1目录下执行脚本，持续输出内容到文本文件textfile1，脚本内容如下：

```Bash
#!/bin/sh
for i in {1..1000}
do 
 echo `date` Hello,welcome to Information2. >> textfile1
 sleep 0.1
done

```

- 使用tail命令观察目标系统/dstdir1目录下textfile1内容。

```Bash
tail -f /dstdir1/textfile1  
```

> > 复制规则状态为复制时，检查目标文件内容更新变化，验证文件实时复制规则的数据实时复制，截图并说明【2分】

- 源系统修改datafile1文件的用户组属性，检查目标文件的权限  
  源系统：`chown -R ftp:ftp datafile1`

> > 验证文件实时复制规则的文件权限修改同步能力，截图并说明【1分】

- 源系统增加textfile1文件的可执行权限,检查目标文件的权限。  
  源系统：`chmod 777 textfile1`

> > 验证文件实时复制规则的文件权限修改同步能力，截图并说明【1分】

- 创建即时恢复规则，选择恢复数据文件datafile1到原机异位置。

> > 验证文件实时复制规则的数据恢复能力，截图并说明【1分】

- 源系统删除datafile1文件。  
  源系统：`rm -rf /srcdir1/datafile1`

> > 验证文件实施复制规则的文件删除操作同步能力，截图并说明【1分】

- 在i2UP界面单击i2COOPY任务，单击查看 实时流量图，进行截图

**环境清理：**

1.在管理界面停止i2COOPY任务，删除该任务，并同意删除 备份数据