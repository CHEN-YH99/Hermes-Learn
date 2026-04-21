```markdown
# Windows 11 彻底卸载 WSL 指南

## 前置说明

本文档介绍如何在 Windows 11 系统中彻底卸载 WSL（Windows Subsystem for Linux）及其所有相关组件和残留文件。

---

## 步骤一：删除所有 Linux 发行版

打开 **PowerShell（管理员）**，先查看已安装的发行版：

```powershell
wsl --list --verbose
```

然后逐个注销：

```powershell
wsl --unregister Ubuntu
wsl --unregister Debian
# 根据实际输出，有哪些就卸载哪些
```

---

## 步骤二：卸载 WSL 组件

### 方法一：命令行卸载

```powershell
wsl --uninstall
```

### 方法二：图形界面卸载

1. 按 `Win + I` 打开 **设置**
2. 进入 **应用** → **已安装的应用**
3. 搜索并卸载以下内容：
   - Windows Subsystem for Linux
   - Windows Subsystem for Linux Update
   - Windows Subsystem for Linux Preview
   - 以及你安装的发行版（如 Ubuntu、Debian 等）

---

## 步骤三：关闭相关 Windows 功能

### 命令行方式

打开 **PowerShell（管理员）**：

```powershell
dism.exe /online /disable-feature /featurename:Microsoft-Windows-Subsystem-Linux /norestart
dism.exe /online /disable-feature /featurename:VirtualMachinePlatform /norestart
```

### 图形界面方式

1. 按 `Win + R`，输入 `optionalfeatures`，回车
2. 取消勾选 **适用于 Linux 的 Windows 子系统**
3. 取消勾选 **虚拟机平台**
4. 点击 **确定**
5. **重启电脑**

---

## 步骤四：删除残留文件

手动删除以下目录：

```
C:\Users\<你的用户名>\AppData\Local\Packages\*Ubuntu*
C:\Users\<你的用户名>\AppData\Local\Packages\*Debian*
C:\Users\<你的用户名>\AppData\Local\Packages\CanonicalGroupLimited*
C:\Users\<你的用户名>\AppData\Local\lxss\
```

> **注意：** 将 `<你的用户名>` 替换为你实际的 Windows 用户名。

---

## 步骤五：重启电脑

```powershell
shutdown /r /t 0
```

---

## 验证是否卸载干净

重启后在 PowerShell 中运行：

```powershell
wsl --status
```

如果提示 `wsl` 不是内部或外部命令，说明已彻底卸载。

---

## 常见问题

| 问题                           | 解决方案                          |
| ------------------------------ | --------------------------------- |
| `wsl --uninstall` 提示需要重启 | 正常现象，重启后再次确认即可      |
| 功能关闭后仍占用磁盘空间       | 检查步骤四的残留目录是否已删除    |
| 卸载后想重新安装               | 运行 `wsl --install` 即可重新安装 |

---

*文档版本：v1.0*
*适用系统：Windows 11*