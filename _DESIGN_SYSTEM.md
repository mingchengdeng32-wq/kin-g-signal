# A股研究报告 · 设计系统参考手册

> 本文件是 kin-g-signal 项目的设计语言规范。
> 生成新报告时，所有 HTML 必须遵循本规范，确保风格一致。
> 参考实现：`meinian.html`（美年健康 002044）

---

## 颜色体系

```css
:root {
  --bg: #07080F;                          /* 页面背景：深夜蓝黑 */
  --surface: rgba(255,255,255,0.042);     /* 卡片背景 */
  --surface-hover: rgba(255,255,255,0.07);
  --border: rgba(201,163,71,0.18);        /* 金色边框 */
  --border-subtle: rgba(255,255,255,0.07);

  --gold: #C9A347;                        /* 主金色：标题/标签 */
  --gold-light: #E8C76A;
  --gold-dim: rgba(201,163,71,0.25);

  --bull: #15B77D;                        /* 多头绿 */
  --bull-dim: rgba(21,183,125,0.15);
  --bear: #E04455;                        /* 空头红 */
  --bear-dim: rgba(224,68,85,0.15);
  --ice: #4A9EDB;                         /* 中性蓝 */
  --ice-dim: rgba(74,158,219,0.15);
  --amber: #F5A623;                       /* 警示橙（中性偏空评级）*/

  --text: #E2D9CC;                        /* 主文本 */
  --text-muted: rgba(226,217,204,0.55);
  --text-dim: rgba(226,217,204,0.35);

  --serif: 'Noto Serif SC', serif;
  --sans:  'Noto Sans SC', sans-serif;
  --mono:  'JetBrains Mono', monospace;
}
```

---

## 幻灯片结构（8页标准）

| 页码 | 标题 | aria-label | 关键组件 |
|------|------|------------|---------|
| 0 | 封面 | 封面 | cover-eyebrow / cover-headline / cover-stats / cover-date |
| 1 | 公司全景 | 公司全景 | grid-4 指标卡 / SVG 柱状图 / 业务矩阵进度条 |
| 2 | 多头 | 多头逻辑 | reason-row.bull-row × 5 |
| 3 | 空头 | 空头逻辑 | reason-row.bear-row × 5 |
| 4 | PK | 多空PK | pk-table / star-bar / pk-winner / 得分卡 |
| 5 | 估值 | 估值模型 | val-row × 5 / val-bar / current-line / 情景表 |
| 6 | 结论 | 投资结论 | rating-badge / kpi-row × 4 / 操作策略 |
| 7 | 回报 | 回报测算 | grid-4 持仓参数 / roi-table × 4行 / EV汇总3格 |

---

## 核心组件速查

### 封面 Cover
```html
<section class="slide active" id="slide-0">
  <div class="cover-eyebrow">深度投资研究报告</div>
  <div class="cover-headline">{公司名}<br>{代码}.SZ/.SH</div>
  <div class="cover-sub">{行业} · {核心标签1} · {核心标签2}</div>
  <div class="cover-stats">
    <div><div class="cover-stat-label">投资评级</div><div class="cover-stat-value amber/bear/bull">{评级}</div></div>
    <div><div class="cover-stat-label">当前股价</div><div class="cover-stat-value">{价格} 元</div></div>
    <div><div class="cover-stat-label">机构目标价</div><div class="cover-stat-value bull">{目标} 元</div></div>
    <div><div class="cover-stat-label">建议介入区间</div><div class="cover-stat-value gold">{低价} – {高价} 元</div></div>
    <div><div class="cover-stat-label">核心矛盾</div><div class="cover-stat-value" style="font-size:0.85rem;">{矛盾描述}</div></div>
  </div>
  <div class="cover-date">{日期} | 基于公开市场数据及机构研报整理 | 仅供参考，不构成投资建议</div>
</section>
```

### 理由行 Reason Row
```html
<div class="reason-row bull-row" style="animation-delay:0.05s;">
  <div class="reason-num gold">01</div>
  <div>
    <div class="reason-title">{标题15字内}</div>
    <div class="reason-sub">{数据依据 · 两行}</div>
  </div>
  <div class="reason-badge bull-badge">{强度标签}</div>
</div>
<!-- 空头：bull-row→bear-row, reason-num gold→red, bull-badge→bear-badge/warn-badge -->
```

### PK 星级表
```html
<table class="pk-table">
  <thead><tr>
    <th style="text-align:left;">维度</th>
    <th>多头 🐂</th><th>空头 🐻</th><th>裁判</th>
  </tr></thead>
  <tbody>
    <tr>
      <td>{维度名}</td>
      <td><div class="star-bar">
        <!-- N颗亮 + (5-N)颗暗 -->
        <div class="star lit-bull"></div>...<div class="star"></div>
      </div></td>
      <td><div class="star-bar">
        <div class="star lit-bear"></div>...<div class="star"></div>
      </div></td>
      <td><span class="pk-winner bull/bear/tie">多头胜/空头胜/平局</span></td>
    </tr>
  </tbody>
</table>
```

### 估值条形图
```html
<!-- max = 比最高目标价稍大的整数，如最高目标9元则max=10 -->
<!-- 当前价百分比 = 当前价/max * 100 -->
<div class="val-row">
  <div class="val-scenario" style="color:var(--bull);">梦想</div>
  <div class="val-bar-wrap">
    <div class="val-bar" style="width:{目标价/max*100}%; background:linear-gradient(90deg,#15B77D,#1DD6A0);"></div>
    <div class="current-line" style="left:{当前价/max*100}%;"></div>
    <!-- current-line::after 内容改为 "当前 {价格}" -->
  </div>
  <div class="val-price" style="color:var(--bull);">{价格}元</div>
  <div class="val-pct" style="color:var(--bull);">+{百分比}%</div>
</div>
```

颜色规则：梦想=bull渐变 | 乐观=gold渐变 | 基准=ice渐变 | 保守=amber渐变 | 悲观=bear红

### 评级徽章
```html
<!-- 中性偏空 -->
<div class="rating-badge neutral-rating">
  <div class="rating-dot amber"></div>
  <div class="rating-text amber">中性偏空 · 等信号再入场</div>
</div>
<!-- 偏多 -->
<div class="rating-badge" style="border:1px solid rgba(21,183,125,.35);background:var(--bull-dim);">
  <div class="rating-dot" style="background:var(--bull);"></div>
  <div class="rating-text" style="color:var(--bull);">谨慎做多 · 分批建仓</div>
</div>
```

### KPI监控行
```html
<div class="kpi-row">
  <div class="kpi-dot red/amber/green"></div>
  <div>
    <div class="kpi-name">{指标名称}</div>
    <div class="kpi-desc">{说明}</div>
    <div class="kpi-status pending/watch">{待观察/持续监控} · {时间窗}</div>
  </div>
</div>
```

---

## 评级颜色规则

| 评级 | 颜色 | rating-badge class |
|------|------|--------------------|
| 强烈推荐/买入 | --bull | 自定义绿色 |
| 谨慎做多 | --ice | 自定义蓝色 |
| 中性偏多 | --gold | 自定义金色 |
| 中性偏空 | --amber | neutral-rating |
| 回避 | --bear | bear-rating |

---

## SVG 柱状图规范（营收趋势）

- viewBox: `0 0 380 145`
- 年份间距：50px
- Y轴范围：0 到 max值的120%
- 渐变：金色=`barGrad`（实际数据）| 蓝色=`foreGrad`（预测）
- 如有AI/新业务：叠加绿色小柱 `aiGrad`
- 每柱顶部标数值，底部标年份
- 图例在底部 y=133

---

## ROI 情景颜色

| 情景 | 行class | 文字色 | 左边框色 |
|------|---------|--------|---------|
| 强烈看涨 | bull-row | --bull | --bull |
| 基准看涨 | ice-row | --ice | --ice |
| 震荡横盘 | amber-row | --amber | --amber |
| 下行止损 | bear-row | --bear | --bear |

---

## 幻灯片控制器

始终使用 `PresentationController` 类（见 meinian.html），支持：
- 键盘左右/上下箭头
- 鼠标滚轮
- 触摸左右滑
- 底部进度条
- 左下角页码计数器

---

## Git 工作流

```bash
REPO="D:\Users\dengmingcheng\kin-g-signal"
git -C "$REPO" add {文件名}.html index.html
git -C "$REPO" commit -m "feat: add {股票名称} {代码} deep research report"
git -C "$REPO" push origin main
# GitHub Pages 地址：https://mingchengdeng32-wq.github.io/kin-g-signal/{文件名}.html
```
