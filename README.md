# 📊 A股深度研究报告系统

基于 Claude Code 的 A 股个股多空分析工具。输入股票代码，自动收集券商研报、生成8页交互式 HTML 分析报告，一键发布到 GitHub Pages。

**在线示例：**
- [金信诺 300252](https://mingchengdeng32-wq.github.io/kin-g-signal/analysis.html) — AI算力 · 卫星互联网
- [美年健康 002044](https://mingchengdeng32-wq.github.io/kin-g-signal/meinian.html) — 体检龙头 · AI健康

---

## 效果预览

每份报告包含 8 页交互式幻灯片，键盘 / 滚轮 / 触摸均可翻页：

| 页 | 内容 |
|----|------|
| 1 | 封面：评级 · 目标价 · 建议买点 · 核心矛盾 |
| 2 | 公司全景：营收趋势图 · 业务矩阵进度条 |
| 3 | 多头视角：必涨5条理由（含数据依据） |
| 4 | 空头视角：必跌5条理由（含风险标签） |
| 5 | 多空PK：6维度星级评分 · 胜负裁判 |
| 6 | 估值模型：5情景目标价区间图 · 当前价标注 |
| 7 | 投资结论：评级 · 操作策略 · 监控KPI |
| 8 | 回报测算：4情景ROI · 期望收益 · 风险回报比 |

---

## 第一步：注册 Claude 账号

所有方式都需要一个 Claude 账号，先完成注册。

1. 打开 [https://claude.ai](https://claude.ai)，点击 **Sign Up**
2. 用邮箱或 Google 账号注册
3. **套餐选择：**

| 套餐 | 价格 | 是否支持本工具 | 说明 |
|------|------|--------------|------|
| Free | 免费 | ❌ 不支持 | 无法使用 Claude Code |
| Pro | $20/月 | ✅ 支持 | 个人使用足够 |
| Max | $100/月 | ✅ 支持 | 用量更大，重度用户 |

> **必须是 Pro 或以上套餐**，免费版不包含 Claude Code 功能。  
> 国内可以用支付宝/微信通过第三方平台订阅，或直接用信用卡。

---

## 第二步：安装 Claude（两种方式二选一）

### 方式 A：Claude Desktop（推荐新手，图形界面）

Claude Desktop 是桌面客户端，内置 Claude Code 功能，界面友好，适合不熟悉命令行的用户。

#### Windows 安装

1. 打开 [https://claude.ai/download](https://claude.ai/download)
2. 点击 **Download for Windows**，下载 `.exe` 安装包（约 100MB）
3. 双击运行安装，一路点 Next
4. 安装完成后桌面会出现 Claude 图标，双击打开
5. 用你注册的账号登录

**开启 Claude Code 模式：**

打开 Claude Desktop 后，在左侧边栏找到 **Claude Code** 入口，或按快捷键打开终端面板。出现命令行输入框即表示 Claude Code 已就绪。

#### Mac 安装

1. 打开 [https://claude.ai/download](https://claude.ai/download)
2. 点击 **Download for Mac**，下载 `.dmg` 文件
3. 双击 `.dmg`，将 Claude 拖入 Applications 文件夹
4. 打开 Launchpad 找到 Claude，首次打开时右键选"打开"（绕过 Gatekeeper 提示）
5. 登录账号

**开启 Claude Code 模式：**

打开 App 后，左侧边栏点击 **Claude Code**，出现终端命令行即可使用。

---

### 方式 B：Claude Code CLI（推荐有命令行基础的用户）

Claude Code CLI 是纯命令行工具，在系统终端里运行，更轻量，适合已经习惯用终端的用户。

#### Windows 安装

**前置：先安装 Node.js**

1. 打开 [https://nodejs.org](https://nodejs.org)
2. 下载 **LTS 版本**（左边那个，稳定版）
3. 双击安装，全部默认选项，一路 Next
4. 安装完成后打开 **PowerShell**（开始菜单搜索 powershell），输入以下命令验证：
   ```
   node --version
   ```
   看到类似 `v20.x.x` 的版本号表示安装成功。

**安装 Claude Code：**

在 PowerShell 里运行：
```powershell
npm install -g @anthropic-ai/claude-code
```

等待安装完成（约1-2分钟），然后验证：
```powershell
claude --version
```

**登录账号：**
```powershell
claude
```
首次运行会弹出浏览器要求登录 Claude 账号，完成后自动返回终端，出现 `>` 提示符表示登录成功。

#### Mac 安装

**前置：先安装 Node.js**

打开终端（Command + Space 搜索 Terminal），运行：
```bash
# 推荐用 Homebrew 安装（没有 Homebrew 先运行下面那行）
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 安装 Node.js
brew install node
```

或者直接去 [https://nodejs.org](https://nodejs.org) 下载 Mac 安装包。

**安装 Claude Code：**
```bash
npm install -g @anthropic-ai/claude-code
```

**登录账号：**
```bash
claude
```
同样会弹出浏览器完成登录，出现 `>` 提示符即就绪。

---

### 方式 C：VS Code 插件（适合程序员）

如果你日常用 VS Code 写代码，可以直接装插件：

1. 打开 VS Code，按 `Ctrl+Shift+X`（Mac：`Cmd+Shift+X`）打开插件市场
2. 搜索 **Claude Code**
3. 点击安装
4. 安装后左侧出现 Claude 图标，点击登录账号

使用时在 VS Code 内打开终端（`Ctrl+\``），直接输入命令即可。

---

## 第三步：安装 Git 和 GitHub

### 安装 Git

**Windows：**
1. 打开 [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. 下载安装包，双击运行
3. 安装时 **编辑器选 VS Code 或 Notepad++**，其他选项默认即可
4. 安装完成后打开 PowerShell，输入 `git --version` 看到版本号即成功

**Mac：**
```bash
# 终端里运行（Homebrew 安装）
brew install git

# 或者输入 git --version，Mac 会自动提示安装 Xcode Command Line Tools
git --version
```

### 注册 GitHub 账号

1. 打开 [https://github.com](https://github.com)，点右上角 **Sign up**
2. 填邮箱、密码、用户名，完成邮箱验证
3. 免费账号即可，无需付费

### 配置 Git 身份

在终端（PowerShell 或 Mac Terminal）里运行：
```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱@example.com"
```

---

## 第四步：Fork 并克隆本仓库

### 为什么要先 Fork 再 Clone？

这是两个不同的操作，缺一不可：

```
Fork  = 在 GitHub 云端，把别人的仓库复制一份到你自己账号下
Clone = 把你自己账号下的仓库，下载到本地电脑
```

**为什么不能直接 Clone 原仓库？**

可以 Clone，但之后 `git push` 会报错"没有权限"——你无法往别人的仓库里写文件。  
Fork 之后，`yourname/kin-g-signal` 完全属于你，可以随意推送报告文件，GitHub Pages 也是你自己的地址。

### 操作步骤

**① Fork（在 GitHub 网页上，点一次按钮）**

1. 登录 GitHub，打开原仓库：  
   `https://github.com/mingchengdeng32-wq/kin-g-signal`
2. 点右上角 **Fork** 按钮
3. 点 **Create fork**，等几秒
4. 页面跳转到 `github.com/你的用户名/kin-g-signal`，Fork 完成

**② Clone（在终端里，把云端仓库下载到本地）**

进入你 Fork 后的仓库，点绿色 **Code** 按钮，复制 HTTPS 地址，然后：

**Windows（PowerShell）：**
```powershell
# 进入你想存放项目的目录
cd D:\Users\你的用户名\Documents

# 克隆（注意：是你自己 Fork 后的地址，不是原作者的）
git clone https://github.com/你的用户名/kin-g-signal.git
cd kin-g-signal
```

**Mac（Terminal）：**
```bash
cd ~/Documents
git clone https://github.com/你的用户名/kin-g-signal.git
cd kin-g-signal
```

---

## 第五步：开启 GitHub Pages

1. 进入你 fork 的仓库主页
2. 点击顶部 **Settings** 选项卡
3. 左侧菜单找到 **Pages**
4. 在 **Source** 下拉框选择 `Deploy from a branch`
5. Branch 选 `main`，目录选 `/ (root)`
6. 点 **Save**
7. 等待 1-2 分钟后刷新页面，会出现绿色提示框显示你的访问地址

你的报告首页地址：`https://你的用户名.github.io/kin-g-signal/`

> 💡 GitHub Pages 对公开仓库免费，无需付费。

---

## 第六步：安装 Skill

打开终端，进入刚才克隆的目录，运行对应命令：

**Windows（PowerShell）：**
```powershell
# 创建 Skill 目录
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills\stock-report"

# 复制 Skill 文件
Copy-Item ".claude\skills\stock-report\SKILL.md" `
  "$env:USERPROFILE\.claude\skills\stock-report\SKILL.md"

# 验证安装成功
Get-Content "$env:USERPROFILE\.claude\skills\stock-report\SKILL.md" | Select-Object -First 5
```

**Mac / Linux（Terminal）：**
```bash
# 创建 Skill 目录
mkdir -p ~/.claude/skills/stock-report

# 复制 Skill 文件
cp .claude/skills/stock-report/SKILL.md ~/.claude/skills/stock-report/SKILL.md

# 验证安装成功
head -5 ~/.claude/skills/stock-report/SKILL.md
```

看到以下内容说明安装成功：
```
---
name: stock-report
description: 生成A股个股深度研究报告...
```

---

## 第七步：生成第一份报告

### 使用 Claude Desktop

1. 打开 Claude Desktop，进入 Claude Code 面板
2. 在终端里进入仓库目录：

   **Windows：**
   ```
   cd D:\Users\你的用户名\Documents\kin-g-signal
   ```
   **Mac：**
   ```
   cd ~/Documents/kin-g-signal
   ```

3. 输入命令：
   ```
   /stock-report 600519 贵州茅台
   ```

### 使用 Claude Code CLI

1. 打开 PowerShell（Windows）或 Terminal（Mac）
2. 进入仓库目录并启动 Claude Code：

   **Windows：**
   ```powershell
   cd D:\Users\你的用户名\Documents\kin-g-signal
   claude
   ```
   **Mac：**
   ```bash
   cd ~/Documents/kin-g-signal
   claude
   ```

3. 出现 `>` 提示符后输入：
   ```
   /stock-report 600519 贵州茅台
   ```

### 首次运行（自动配置）

第一次运行时，Claude 会自动检测仓库路径和 GitHub Pages 地址，完成一次性配置（约10秒），然后开始生成报告。

整个流程约 3-5 分钟，完成后返回：
```
✅ 报告已生成并发布

📊 贵州茅台 600519 研究报告
🔗 报告：https://你的用户名.github.io/kin-g-signal/maotai.html
🏠 首页：https://你的用户名.github.io/kin-g-signal/
```

把链接发给家人朋友，手机电脑都能直接打开。

---

## 日常使用

```
/stock-report <股票代码> <股票名称>
/stock-report <股票代码> <股票名称> 持仓<金额> 持仓<天数>天
```

**示例：**
```
/stock-report 600519 贵州茅台
/stock-report 300750 宁德时代 持仓10万
/stock-report 000858 五粮液 持仓30万 持仓60天
/stock-report 002594 比亚迪 持仓20万 持仓120天
```

也可以直接用中文说：
```
帮我分析一下 600519 贵州茅台
研究下宁德时代，持仓20万，持90天
```

---

## 重置配置

换了电脑或迁移仓库时，告诉 Claude：
```
stock-report 重置路径
```

或手动删除配置文件：

**Windows：**
```powershell
Remove-Item "$env:USERPROFILE\.claude\skills\stock-report\config.md"
```
**Mac：**
```bash
rm ~/.claude/skills/stock-report/config.md
```

---

## 常见问题

**Q：Free 免费账号能用吗？**  
A：不能。必须是 Pro（$20/月）或以上套餐，才能使用 Claude Code 功能。

**Q：不会用命令行怎么办？**  
A：用 Claude Desktop，图形界面操作，Claude Code 面板和普通聊天窗口差不多，只是多了一个终端输入框。

**Q：国内网络能用吗？**  
A：Claude 服务在国内需要网络代理。安装和使用过程中保持代理开启即可，报告生成的 GitHub Pages 链接不需要代理，可以直接分享给家人浏览。

**Q：GitHub 要付费吗？**  
A：不用。公开仓库 + GitHub Pages 对免费账号完全免费。

**Q：数据来源是哪里？**  
A：Claude 实时搜索东方财富、证券之星、新浪财经、各券商公开研报等，不依赖付费数据接口。

**Q：支持港股/美股吗？**  
A：当前 Skill 针对 A 股优化。修改 `SKILL.md` 里的搜索关键词可适配其他市场。

**Q：报告内容可靠吗？**  
A：报告基于公开数据整理，仅供参考学习，**不构成任何投资建议**。

---

## 项目结构

```
kin-g-signal/
├── index.html                        # 报告导航首页
├── _DESIGN_SYSTEM.md                 # 设计规范文档
├── README.md                         # 本安装说明
├── .claude/
│   └── skills/
│       └── stock-report/
│           └── SKILL.md              # Claude Code Skill（核心）
└── *.html                            # 各股票报告文件
```

---

## 设计系统

所有报告使用统一深色设计语言，详见 [`_DESIGN_SYSTEM.md`](./_DESIGN_SYSTEM.md)：

- 深夜蓝黑背景 `#07080F`，金色主题 `#C9A347`
- 多头绿 `#15B77D` / 空头红 `#E04455`
- 字体：Noto Serif SC（标题）+ JetBrains Mono（数据）

---

## License

MIT — 自由使用和修改，保留原始声明即可。

**免责声明：** 报告内容仅供参考学习，不构成任何投资建议。股市有风险，入市需谨慎，请以自身风险承受能力为最终决策依据。
