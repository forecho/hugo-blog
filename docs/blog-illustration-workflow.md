# 博客配图 + 封面图工作流（可复用 SOP）

给这个 Hugo 博客的文章批量配「信息图」并生成封面图的标准流程。
下次写新文章、或改造现有文章时，把这份文档丢给我（Claude），说「按 docs/blog-illustration-workflow.md 给这篇配图」即可直接复用。

工具链一句话总结：**baoyu-skills 出图逻辑 + 4AICode relay 文生图后端 + imgant 图床**。
图只上 imgant，不进仓库（本地中间产物放在 gitignore 的 `illustrations/`）。

---

## 0. 何时用哪种图

- **数据/对比/流程/参数密集的小节** → 做「信息图」，文字数字必须准确（这是这类文章的价值所在，装饰插画不承载信息，别用）。
- **纯概念、引言、结论** → 可用 `scene` 概念插画或 `framework` 清单图。
- 判断标准：这一节有没有「可以结构化成表格/卡片/箭头」的内容？有就做信息图。

每个二级标题（H2）配一张（`per-section` 密度），与文章结构一一对应。

---

## 1. 前置环境（已配好，确认即可）

| 项 | 值 |
|---|---|
| bun | `/Users/forecho/.bun/bin/bun` |
| 文生图后端 | baoyu-image-gen：`.agents/skills/baoyu-image-gen/scripts/main.ts` |
| relay 端点 | `OPENAI_BASE_URL=https://relay.4aicode.com/v1` |
| relay 密钥 | 环境变量 `RELAY4AI_API_KEY`（映射为 `OPENAI_API_KEY`，**不要打印**） |
| 模型 | `gpt-image-2`，`--ar 16:9 --quality 2k` 会输出 1672×941 横图 |
| 图床 | `imgant upload <file> --album "博客" -f url -q` → 返回 `https://imgant.forecho.com/...png` |
| illustrator 配置 | `~/.baoyu-skills/baoyu-article-illustrator/EXTEND.md`（watermark off / zh / same-dir） |
| cover 配置 | `~/.baoyu-skills/baoyu-cover-image/EXTEND.md`（watermark off / zh / 16:9 / flat-vector auto） |

后端连通性自测（出一张测试图即可）：

```bash
env OPENAI_API_KEY="$RELAY4AI_API_KEY" OPENAI_BASE_URL="https://relay.4aicode.com/v1" \
  /Users/forecho/.bun/bin/bun .agents/skills/baoyu-image-gen/scripts/main.ts \
  --prompt "flat minimalist blue circle, no text" --image /tmp/t.png \
  --provider openai --model gpt-image-2 --ar 16:9 --quality 2k
```

---

## 2. 视觉系统（保持全站统一）

固定走 **editorial 编辑信息图 + 青绿/深蓝金融科技配色**。每个 prompt 文件都带这段：

```
STYLE: Editorial magazine-style infographic, clean modern flat-vector, financial-report
aesthetic. Thin precise line icons, modular rounded cards, subtle soft shadows, crisp
sans-serif labels, strong hierarchy. Diagram-style only — no photorealism, no 3D.

PALETTE (teal + navy fintech):
- Background off-white #F5F8FA; deep navy #0B2239 for text/frames; teal #12B6A0 main
  accent & positive; blue #1C7ED6 secondary; soft tints #D7F2EC / #E3EEF7 for card fills;
  coral #EF5B5B ONLY for risks / warnings / cons.
Color values (#hex) and color names are rendering guidance only — do NOT display them as visible text.

TEXT: All labels in Simplified Chinese, short keywords only, large & legible. Render
numbers, percentages, dates and English tickers (AAPL/TSLA/NVDA) exactly. No garbled characters.

COMPOSITION: Clean, generous white space, grid-aligned.
ASPECT: 16:9
```

封面图换成 **conceptual + cool 蓝绿 + flat-vector + 主标/副标**（见第 6 节）。

---

## 3. 小节 → 图表类型 映射

| 内容形态 | type | 例子 |
|---|---|---|
| 链路/结构/层级 | `framework` | 托管链路：用户→代币→SPV→托管→DTCC |
| A vs B | `comparison`（左右两栏） | 传统券商 vs Bitget |
| 多选项并排 | `comparison`（三列卡） | 现货/合约/IPO Prime |
| 数字/费率/指标 | `infographic`（大数字 hero） | 费率 0.05%、安全基金 |
| 步骤流程 | `flowchart`（左→右编号卡） | 上手三步 |
| 误区/清单 | `infographic`（2×2 卡片） | 几个坑 |
| 概念/收尾 | `scene` / `framework` | 引言三条路、结论行动清单 |

---

## 4. 标准流程（新文章配图）

> baoyu-article-illustrator 的硬性要求：**先确认设置 → 先写 prompt 文件 → 再批量出图**。

1. **确认设置**（AskUserQuestion 一次）：type=混合信息图、density=per-section、style=editorial、palette=青绿+深蓝。
2. **建工作目录**（gitignore 掉，图最终上图床，本地是中间产物）：
   ```bash
   mkdir -p illustrations/<slug>/prompts
   grep -qxF 'illustrations/' .gitignore || printf '\nillustrations/\n' >> .gitignore
   ```
3. **写 outline.md + 每张一个 prompt 文件** `prompts/NN-{type}-{slug}.md`：
   - 每个 LABELS 用**文章里的真实数字/术语**（别编）。
   - **标题行只写中文**，不要带 "Comparison / Data Visualization / Framework" 这类英文类型词——否则会被渲染成副标题。可加一句：
     `Render only this Chinese title; do NOT render any English subtitle.`
4. **先出 1 张样板**对齐风格（挑信息最密的那张，如三列对比）：
   ```bash
   env OPENAI_API_KEY="$RELAY4AI_API_KEY" OPENAI_BASE_URL="https://relay.4aicode.com/v1" \
     /Users/forecho/.bun/bin/bun .agents/skills/baoyu-image-gen/scripts/main.ts \
     --promptfiles illustrations/<slug>/prompts/04-xxx.md \
     --image illustrations/<slug>/04-xxx.png \
     --provider openai --model gpt-image-2 --ar 16:9 --quality 2k
   ```
   用 Read 工具看图，核对中文/数字是否准确、有没有英文泄漏。
5. **批量出剩余的**。写 `batch.json`（⚠️ 路径相对于 batch.json 自身目录，不是 cwd）：
   ```json
   { "jobs": 3, "tasks": [
     { "id":"01-...", "promptFiles":["prompts/01-...md"], "image":"01-...png",
       "provider":"openai", "model":"gpt-image-2", "ar":"16:9" }
   ]}
   ```
   ```bash
   env OPENAI_API_KEY="$RELAY4AI_API_KEY" OPENAI_BASE_URL="https://relay.4aicode.com/v1" \
     /Users/forecho/.bun/bin/bun .agents/skills/baoyu-image-gen/scripts/main.ts \
     --batchfile illustrations/<slug>/batch.json --jobs 3
   ```
6. **逐张 Read 验收**。文字错了就**改 prompt 重出**，绝不用代码在图上贴字/涂改。
7. **上传图床**：
   ```bash
   cd illustrations/<slug>
   for f in 0*.png; do echo "$f -> $(imgant upload "$f" --album "博客" -f url -q)"; done
   ```
8. **插入文章**：每个 H2 开头一张 `![一句中文说明](url)`，前后空行。
9. **验证**：`hugo --buildFuture --quiet` 通过；`curl -s -o /dev/null -w "%{http_code}" <url>` 每张返回 200。

---

## 5. 改造现有文章（替换旧配图）

旧图大多是装饰性概念插画，按上面 1–7 步生成新信息图、上传图床，然后**只替换 URL**（保留原 alt 文案）：

```
# 用 Edit 工具把每个旧 imgant URL 换成新 URL，按 H2 顺序一一对应
旧: ![原说明](https://imgant.forecho.com/.../旧hash.png)
新: ![原说明](https://imgant.forecho.com/.../新hash.png)
```

替换后核对：`grep -c imgant 文章.md` 数量对得上、无残留旧 hash、`hugo --buildFuture` 通过、新 URL 全 200。

---

## 6. 封面图（baoyu-cover-image）

1. 确认维度（AskUserQuestion 一次）：type=conceptual、palette=cool 蓝绿、rendering=flat-vector、text=主标+副标。
2. 写 `prompts/cover.md`：核心概念一句话 + 主标/副标（中文，照抄不要编）+ 上面的 cool 配色块 + `ASPECT: 16:9`。
3. 出图：
   ```bash
   env OPENAI_API_KEY="$RELAY4AI_API_KEY" OPENAI_BASE_URL="https://relay.4aicode.com/v1" \
     /Users/forecho/.bun/bin/bun .agents/skills/baoyu-image-gen/scripts/main.ts \
     --promptfiles illustrations/<slug>/prompts/cover.md \
     --image illustrations/<slug>/cover.png \
     --provider openai --model gpt-image-2 --ar 16:9 --quality 2k
   ```
4. 上传图床后**插在 frontmatter 之后、第一个 H2 之前**当头图（echo 主题没有 frontmatter 封面字段，og:image 固定是头像，所以封面靠正文首图体现）。

---

## 7. 踩过的坑

- **标题英文泄漏**：prompt 标题行带英文类型词会被画进图里当副标题 → 标题行只留中文，并显式禁用英文副标题。
- **batch.json 路径**：相对于 batch.json 所在目录解析，写 `prompts/xx.md` 而不是 `illustrations/<slug>/prompts/xx.md`。
- **中文/数字准确性**：gpt-image-2 出短中文关键词 + 数字 + 英文 ticker 基本可靠；句子越短越稳。错了重出，别涂改。
- **尺寸**：`--quality 2k --ar 16:9` 给 1672×941 横图，适合正文内联与头图。
- **先样板后批量**：先出一张对齐视觉，避免 9 张一起翻车。
- **macOS 无 `timeout` 命令**：靠工具自身超时，别用 `timeout`。
- **图不进仓库**：`illustrations/` 已 gitignore，最终只引用 imgant URL。
