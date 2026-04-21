## 一、在 Windows 上安装并配置 WSL2

**1.安装 WSL**

以 **管理员身份** 打开 PowerShell，并执行以下命令：

shell

```text
wsl --install
```

执行完毕后，**重启你的电脑**

**2.设置 WSL2 为默认版本**

查看在线商店可下载的 Linux 发行版列表，

再次进入 PowerShell ，并执行以下命令：

shell

```text
wsl.exe --list --online
```

效果如图：![图像](https://pbs.twimg.com/media/HGRiFcYa0AAu4n0?format=png&name=900x900)

1. 我选择的 **Ubuntu-24.04**，执行以下命令：

   shell

   ```text
   wsl.exe --install Ubuntu-24.04
   ```

   它好像自动给我下了**Ubuntu 22.04 LTS**（最新 LTS 版本）

   安装完毕，可以查询当前**Ubuntu的型号，**查询代码和效果如下：![图像](https://pbs.twimg.com/media/HGRjEuxbQAAqSq_?format=png&name=900x900)

   shell

   ```text
   wsl.exe --list --verbose
   ```

   我这里有两个，我需要切换一下 **Ubuntu 22.04**，命令如下：

   shell

   ```text
   wsl --set-default Ubuntu-20.04
   ```

   **3.安装 Linux 发行版（推荐 Ubuntu）**

   1. 打开 **Microsoft Store** (微软应用商店)
   2. 搜索 **“Ubuntu”**
   3. 选择 **“Ubuntu 20.04 LTS”** (或最新 LTS 版本)，点击 **“获取”** 进行安装![图像](https://pbs.twimg.com/media/HGSA3ozbcAAqOi3?format=jpg&name=900x900)**4.初始化 Ubuntu**
      1. 安装完成后，在开始菜单中启动 **“Ubuntu”**
      2. 首次启动会要求你创建一个 **Linux 用户名和密码**（这与你的 Windows 账户无关，请必须牢记）
      3. 初始化完成后，你就拥有了一个功能完备的 Linux 终端![图像](https://pbs.twimg.com/media/HGSAsIYaYAAUJns?format=png&name=900x900)

**补充必看：安装完毕之后，会弹出一个适用于Linux的Windows子系统设置**

> **我们需要修改一下网络设置，默认为Nat模式，**我们千万不要选那个模式，你选了那个模式之后，它的各种网络请求都不会跟你电脑配置相同，导致后续安装出问题，**大家一定要选择Mirrored这个模式（非常重要，我没改之前卡了好久）**

> **注：**选择Mirrored这个模式之后，它就会跟随你电脑的网络，比如说我电脑这里挂代理，因为我们下载一些从GitHub上拉项目，安装都需要网络代理支持的，如果说你选择NAT的话，就没办法下载成功

> **关闭防火墙，启用localhost转发打开，自动代理开启，设置完重启电脑**

![图像](https://pbs.twimg.com/media/HGSFcqeaAAAKPdb?format=jpg&name=900x900)

## 二、在 WSL2 中安装 Hermes Agent

我们的操作环境切换到了 WSL2 的 Ubuntu 终端

**1.执行官方一键安装脚本**

在 Ubuntu 终端中粘贴并运行(Linux / macOS同理)：

bash

```
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermesagent/main/scripts/install.sh | bash
```

![图像](https://pbs.twimg.com/media/HGSM8cBa4AAv7ZN?format=png&name=900x900)

这个脚本会自动完成以下工作：

- 安装 **uv** (超快的 Python 包安装器和虚拟环境管理器)
- 安装 Python 3.11+
- 克隆 Hermes Agent 仓库
- 创建并激活虚拟环境
- 安装所有依赖
- 将 **hermes** 命令添加到你的 PATH

**2.重载 Shell 并验证安装**

安装完成后，为了让 **hermes** 命令生效，需要重载你的 Shell 配置：

```bash
# 对于 Bash 用户
source ~/.bashrc

# 对于 Zsh 用户（如果你已切换）
source ~/.zshrc
```

验证是否安装成功：

```bash
hermes --version
```

应该能看到类似 **hermes x.x.x** 的输出，如图：

![图像](https://pbs.twimg.com/media/HGSawSlbcAAH-82?format=png&name=900x900)

**3.配置模型提供商**

运行设置向导，连接你的大模型 API，输入下面命令，启动配置向导：

bash

```bash
hermes setup
```

来到这个页面，选择 **Quick setup**

> **Quick setup**（推荐）— 只配置核心的：AI provider、模型、和消息平台

> **Full setup** — 配置所有选项，更细致但步骤更多

![图像](https://pbs.twimg.com/media/HGScVi0aQAAudFG?format=png&name=900x900)

**在交互式界面中：**

> 选择你的模型提供商（如 OpenAI, Anthropic, OpenRouter, 或国内便宜好用的大模型）

![图像](https://pbs.twimg.com/media/HGSnLaLaEAAVSxU?format=png&name=900x900)

输入对应的 **API Key，有人会问老师我没有怎么办？下面我会讲**

![图像](https://pbs.twimg.com/media/HGSnQXHbAAAVl4W?format=png&name=900x900)

选择一个默认模型（如 gpt-4o, claude-3-5） 我选择一个便宜的minimax-m2.5

![图像](https://pbs.twimg.com/media/HGSoyZ6aQAAHm11?format=png&name=900x900)

**获取 API Key 的简单方法**（以 **OpenRouter** 为例）：

- 注册账号去

  [https://openrouter.ai](https://openrouter.ai/)

- 充值一点钱（几美元就够用很久）

- 在 dashboard 里复制你的 API Key（一串很长的字符）

![图像](https://pbs.twimg.com/media/HGSmUmNbwAAvFbL?format=jpg&name=900x900)

**4.配置聊天软件（以 Telegram 为例）**

来到这个页面我们选择 **Set up messaging now**

- **Set up messaging now**（推荐）— 现在就绑定平台，装完即可使用
- **Skip** — 跳过，之后用 hermes setup gateway 命令再配置

![图像](https://pbs.twimg.com/media/HGSpyEeakAAVhTH?format=png&name=900x900)

我们选择 **Telegram，下面番外篇要连**

![图像](https://pbs.twimg.com/media/HGSqFTEbgAAnAPF?format=png&name=small)

配置完成后，你可以通过 **hermes** 命令直接与 Agent 对话，测试其基本功能

![图像](https://pbs.twimg.com/media/HGSrVEWacAALBbF?format=png&name=900x900)

**避坑提示**：

> **网络问题**：如果 **curl** 命令卡住或失败，很可能是网络问题。请确保你的 WSL2 能正常访问 GitHub。必要时可配置 Git 代理

> **权限问题**：不要在 WSL2 中使用 **sudo** 来运行 **hermes** 命令，这可能导致权限混乱。始终以普通用户身份运行

## **三、小白常见问题 & 建议**

**1.安装是卡住了怎么办**

> 耐心等待，第一次会下载很多东西

**2.我没有API Key怎么办**

> 上面有获取 API Key 的简单方法

**3.我想24小时运行怎么办**

> 荐买个便宜 VPS（阿里云、腾讯云 5-10 美元/月），在 VPS 上安装，操作同理

**4.我想把我的OpenClaw 迁移过来怎么怎么办**

> 直接用 hermes claw migrate 命令即可

## 常用命令（记住这几个行）

- hermes  （进入交互式聊天）
- hermes setup （一站式配置向导）
- hermes model （切换模型）
- hermes tools （配置可用工具）
- hermes config set KEY 值 （设置单个配置）
- hermes claw migrate （如果你以前用 OpenClaw，可以一键迁移数据）

## 四、番外篇连接聊天软件

以 Telegram 为例（超级简单）：

1. 打开 Telegram，搜索

   [**@BotFather**](https://x.com/@BotFather)

2. 发送 /newbot

3. 给机器人起名字和用户名（比如 星辰机器人小助理）

4. BotFather 会给你一个 **Bot Token**（一长串英文数字），复制下来

然后在终端输入：

bash

```bash
hermes gateway
```

或者用配置命令设置 Telegram Token，我用的第二种自动帮我配置好了

具体配置可以输入：

bash

```bash
hermes config set TELEGRAM_BOT_TOKEN 你的token在这里
```

![图像](https://pbs.twimg.com/media/HGSusRQbUAAHVWo?format=png&name=900x900)

启动网关：

bash

```bash
hermes gateway telegram
```

然后在 Telegram 里搜索你的机器人，可以开始聊天了

如果你的电脑/VPS 一直开着，它会 24小时在线

![图像](https://pbs.twimg.com/media/HGTMEnPaYAAIPZD?format=jpg&name=900x900)

## 结语

**大家有什么问题可以评论区留言，教程编写不易，如果此教程对你有帮助麻烦给我一个小小的关注**

WSL子系统安装在C盘会很占内存，可以做迁移，可以看这位大佬的教程

https://x.com/liyue_ai/status/2044047643098398799

WSL 安装官方文档：

https://learn.microsoft.com/en-us/windows/wsl/install

官方 GitHub：

https://github.com/nousresearch/hermes-agent

Hermes 学习官方文档：

[https://hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs/)