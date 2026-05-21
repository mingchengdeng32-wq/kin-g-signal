# 📊 A股深度研究报告系统

基于 Claude Code 的 A 股个股多空分析工具。输入股票代码，自动收集券商研报、生成8页交互式 HTML 分析报告，一键发布到 GitHub Pages。

**在线示例：**
- [金信诺 300252](https://mingchengdeng32-wq.github.io/kin-g-signal/analysis.html) — AI算力·卫星互联网
- [美年健康 002044](https://mingchengdeng32-wq.github.io/kin-g-signal/meinian.html) — 体检龙头·AI健康

---

## 效果预览

每份报告包含 8 页幻灯片，键盘/滚轮/触摸均可翻页：

| 页 | 内容 |
|----|------|
| 1 | 封面：评级 · 目标价 · 建议买点 |
| 2 | 公司全景：营收趋势图 · 业务矩阵 |
| 3 | 多头视角：必涨5条理由 |
| 4 | 空头视角：必跌5条理由 |
| 5 | 多空PK：6维度星级评分表 |
| 6 | 估值模型：5情景目标价区间图 |
| 7 | 投资结论：评级 · 操作策略 · 监控KPI |
| 8 | 回报测算：4情景ROI · 期望收益 · 风险回报比 |

---

## 安装（5分钟完成）

### 前置条件

- [Claude Code](https://claude.ai/code) 已安装并登录
- Git 已安装，GitHub 账号已登录
- 有自己的 GitHub 仓库（或 fork 本仓库）

### 第一步：Fork 并 clone 本仓库

```bash
# 在 GitHub 上 fork 本仓库，然后 clone 到本地
git clone https://github.com/你的用户名/kin-g-signal.git
cd kin-g-signal
```

### 第二步：开启 GitHub Pages

1. 进入你 fork 的仓库 → Settings → Pages
2. Source 选 `main` 分支，目录选 `/ (root)`
3. 保存，等 1-2 分钟即可访问

你的 Pages 地址：`https://你的用户名.github.io/kin-g-signal/`

### 第三步：安装 Skill 到 Claude Code

```bash
# Mac / Linux
mkdir -p ~/.claude/skills/stock-report
cp .claude/skills/stock-report/SKILL.md ~/.claude/skills/stock-report/SKILL.md

# Windows (PowerShell)
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills\stock-report"
Copy-Item ".claude\skills\stock-report\SKILL.md" "$env:USERPROFILE\.claude\skills\stock-report\SKILL.md"
```

### 第四步：首次运行（自动配置）

打开 Claude Code，进入你 clone 的目录，运行：

```
/stock-report 600519 贵州茅台
```

Claude 会自动检测仓库路径和 GitHub Pages 地址，完成一次性配置后立即生成报告。

> 如果自动检测失败，按提示手动输入路径即可，只需配置一次。

---

## 使用方法

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

也可以直接说：「帮我分析一下 600519 贵州茅台」

**生成完成后 Claude 会返回：**
```
✅ 报告已生成并发布

📊 贵州茅台 600519 研究报告
🔗 报告：https://你的用户名.github.io/kin-g-signal/maotai.html
🏠 首页：https://你的用户名.github.io/kin-g-signal/
```

---

## 重置配置

如果换了电脑或迁移仓库，告诉 Claude：

```
stock-report 重置路径
```

或者直接删除配置文件：

```bash
# Mac / Linux
rm ~/.claude/skills/stock-report/config.md

# Windows (PowerShell)
Remove-Item "$env:USERPROFILE\.claude\skills\stock-report\config.md"
```

---

## 项目结构

```
kin-g-signal/
├── index.html              # 报告导航首页
├── _DESIGN_SYSTEM.md       # 设计规范（Skill 生成报告时参考）
├── README.md               # 本文件
├── .claude/
│   └── skills/
│       └── stock-report/
│           └── SKILL.md    # Claude Code Skill 定义
└── *.html                  # 各股票报告文件
```

---

## 常见问题

**Q：需要 API Key 吗？**
A：不需要，完全通过 Claude Code 客户端运行，使用你自己的 Claude 账号。

**Q：数据来源是哪里？**
A：Claude 实时搜索东方财富、证券之星、新浪财经、各券商公开研报等公开来源，不依赖付费数据。

**Q：支持港股/美股吗？**
A：当前 Skill 针对 A 股优化。修改 SKILL.md 里的搜索关键词可以适配其他市场。

**Q：报告内容可靠吗？**
A：报告基于公开市场数据和机构研报整理，仅供参考学习，**不构成投资建议**。

---

## 设计系统

所有报告使用统一的深色设计语言，详见 [`_DESIGN_SYSTEM.md`](./_DESIGN_SYSTEM.md)：

- 深夜蓝黑背景 `#07080F`
- 金色主题 `#C9A347`  
- 多头绿 `#15B77D` / 空头红 `#E04455`
- 字体：Noto Serif SC + JetBrains Mono

---

## License

MIT — 自由使用和修改，保留原始声明即可。

报告内容仅供参考，不构成投资建议，股市有风险，入市需谨慎。
