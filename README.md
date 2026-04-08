# SCITAS S3 数据传输与 Rclone 操作手册

本文档详细说明了如何在 EPFL SCITAS 集群上配置 Rclone 环境，并执行 S3 存储桶数据的检查、下载及清理操作。

---

## 1. 系统登录与环境准备

**1.1 登录 SCITAS 集群**
首先，使用 SSH 登录到 izar3 节点（更换自己的名字）：
```bash
ssh pxu@hpc.izar3.epfl.ch
```

**1.2 切换工作目录**
进入指定的工作路径（就用这个，不要改）：
```bash
cd /work/upablasser/CS-spsb3
```

**1.3 配置 Rclone 环境变量**
将 rclone 模块加载命令写入环境变量配置文件，并使其立即生效：
```bash
echo 'module load rclone/1.65.2' >> ~/.bashrc
source ~/.bashrc
```

---

## 2. 配置 Rclone

**2.1 编辑配置文件**
使用 `vi` 编辑器打开（或创建）Rclone 的配置文件：
```bash
vi .config/rclone/rclone.conf
```

**2.2 导入配置信息**
1. 在打开的空文件中，按 `i` 键进入插入模式。
2. 将**邮件附件中提供的配置内容**复制并粘贴到该文件中。
3. 按 `Esc` 键退出插入模式，输入 `:wq` 并按回车键（保存并退出）。

---

## 3. 数据操作与管理

**3.1 检查云端数据**
在下载前，使用 `ncdu` 交互式命令查看 S3 存储桶中的数据目录结构和占用大小：
```bash
rclone ncdu S3-UNIL-RW:
```

**3.2 下载数据**
使用 `copy` 命令将特定的数据集从 S3 存储桶下载到当前工作目录。`-P` 参数用于实时显示下载进度：
```bash
rclone copy -P S3-UNIL-RW:DW_Glacios_EPFL_Ablasser_Lingyun_grid3_D2_20260407 ./DW_Glacios_EPFL_Ablasser_Lingyun_grid3_D2_20260407
```

**3.3 删除云端数据（清理）**
数据成功下载并确认无误后，使用 `purge` 命令删除云端的数据以释放空间（**注意：此操作不可逆**）：
```bash
rclone purge S3-UNIL-RW:DW_Glacios_EPFL_Ablasser_Lingyun_grid3_D2_20260407 -P
```
