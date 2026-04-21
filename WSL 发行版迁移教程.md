# WSL 发行版迁移教程

将 WSL 从一个位置迁移到另一个位置（通常是从 C 盘迁移到 D 盘）。

---

## 步骤一：查看当前发行版

```powershell
wsl --list --verbose
```

记下你要迁移的发行版名称，比如 `Ubuntu`。

---

## 步骤二：导出发行版

```powershell
wsl --export Ubuntu D:\WSL\ubuntu-backup.tar
```

> 将 `Ubuntu` 替换为你的发行版名称，路径可以自定义。

---

## 步骤三：注销原有发行版

```powershell
wsl --unregister Ubuntu
```

> **警告：** 此操作会删除 C 盘上该发行版的所有数据，请确保步骤二导出成功。

---

## 步骤四：导入到新位置

```powershell
wsl --import Ubuntu D:\WSL\Ubuntu D:\WSL\ubuntu-backup.tar
```

参数说明：

| 参数                       | 含义           |
| -------------------------- | -------------- |
| `Ubuntu`                   | 发行版名称     |
| `D:\WSL\Ubuntu`            | 新的安装位置   |
| `D:\WSL\ubuntu-backup.tar` | 刚才导出的文件 |

---

## 步骤五：验证迁移结果

```powershell
wsl --list --verbose
```

进入 WSL 检查数据是否完整：

```powershell
wsl -d Ubuntu
```

```bash
# 检查用户目录
ls -la ~

# 检查磁盘使用
df -h
```

---

## 步骤六：清理导出文件

确认无误后，删除导出的备份文件：

```powershell
Remove-Item D:\WSL\ubuntu-backup.tar
```

---

## 常见问题

| 问题                              | 解决方案                                         |
| --------------------------------- | ------------------------------------------------ |
| 导入后默认用户变成 root           | 运行 `ubuntu config --default-user <你的用户名>` |
| 发行版名称带版本号如 Ubuntu-22.04 | 命令中名称也要带版本号                           |
| 导出文件过大                      | 正常现象，包含整个 Linux 系统                    |
| 提示正在运行无法注销              | 先执行 `wsl --shutdown` 再注销                   |

---

## 修改默认用户（可选）

导入后如果默认用户变成 root：

```powershell
# Ubuntu
ubuntu config --default-user yourusername

# Ubuntu 22.04
ubuntu2204 config --default-user yourusername

# Debian
debian config --default-user yourusername
```

---

## 完整流程示例

```powershell
# 1. 查看发行版
wsl --list --verbose

# 2. 关闭 WSL
wsl --shutdown

# 3. 导出
wsl --export Ubuntu D:\WSL\ubuntu-backup.tar

# 4. 注销
wsl --unregister Ubuntu

# 5. 导入
wsl --import Ubuntu D:\WSL\Ubuntu D:\WSL\ubuntu-backup.tar

# 6. 设置默认用户
ubuntu config --default-user yourusername

# 7. 验证
wsl -d Ubuntu

# 8. 清理备份
Remove-Item D:\WSL\ubuntu-backup.tar
```

---

需要我将这个教程也制作成单独的 md 文件吗？