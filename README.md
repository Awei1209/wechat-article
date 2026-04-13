---
title: LongWrite Skill 说明文档
---

# LongWrite Skill（模板）说明

这是一个参考 `oaker-io/wewrite` 的 **SKILL.md 主管道设计**写成的通用模板，用于把“写一篇长文”的工作拆成可自动化的 8 个 Step，并且具备：
- 默认全自动（不中断）
- 交互模式（用户想选题/框架/配图时再停）
- 每步可降级（不因某个能力缺失而全流程崩溃）
- 进度追踪（Step 1-8）
- 完成协议（DONE / DONE_WITH_CONCERNS / BLOCKED / NEEDS_CONTEXT）

本模板默认不依赖具体项目代码；你可以按自己的业务把每一步“挂接”到脚本/API/工作流。

---

## 文件结构

```
skills/longwrite/
├── SKILL.md
└── references/
    ├── onboard.md
    ├── frameworks.md
    ├── writing-guide.md
    ├── seo-rules.md
    └── visual-prompts.md
```

### 各文件作用

- `SKILL.md`：主管道（Step 1-8）+ 运行模式 + 降级策略 + 输出协议（核心文件）
- `references/onboard.md`：首次问答与 `style.yaml` 生成规范
- `references/frameworks.md`：写作骨架库（用于 Step 3）
- `references/writing-guide.md`：写作底线规则（禁用词、真实锚定、编辑锚点等）
- `references/seo-rules.md`：标题/摘要/标签生成规范
- `references/visual-prompts.md`：封面与内文配图提示词结构（可接入生图器）

---

## 如何改成你自己的 Skill（最常见 5 处改动）

### 1) 改 Skill 名称与触发范围

编辑 `SKILL.md` 顶部 YAML：
- `name`: skill 的唯一名称（如 `wewrite`、`longwrite`）
- `description`: **触发关键词**尽量明确你的上下文（如“公众号/长文/草稿箱/排版”）

建议写法：
- “应被触发”的关键词：与你的产品/平台强相关
- “不应被触发”的关键词：避免被通用“写文章/写邮件”误触发

### 2) 明确你的“目标平台”和“输出格式”

这会影响 Step 5（SEO）与 Step 7（排版发布）：
- 公众号：标题长度、摘要字数、标签体系、微信兼容排版
- 博客/Newsletter：Markdown 原样输出即可，排版需求更轻

推荐做法：在 `style.yaml`（Onboard 生成）里保留 `platform` 字段供后续步骤读取。

### 3) 把 Step 2/3 接到你的真实数据源

你可以用三种方式挂接：

**A. 继续用 WebSearch/WebFetch**
- 优点：简单直接
- 风险：在某些环境/域名可能不可用（所以要保留 `skip_websearch` 降级）

**B. 换成你的内部搜索/知识库**
- 在 Step 3 “素材采集”处，改为调用你的 API（并输出统一的“素材清单”格式）

**C. 换成离线语料/本地文件**
- 引导用户提供链接/文件/要点
- 或从指定目录读取（如果你的运行环境允许）

> 关键点：无论数据源是什么，都要产出“可引用的素材锚点”，并在写作时明确插入到对应段落，避免编造。

### 4) 接入（或关闭）配图能力

在 Step 1 设定降级标记：
- 没有图像生成器 → `skip_image_gen=true`，Step 6 只输出提示词（用户可人工生成）
- 有图像生成器 → 在 Step 6 加入你的“生成图片”调用，并把图片产物与插入位置绑定

### 5) 接入（或关闭）发布能力

在 Step 1 设定降级标记：
- 没有草稿箱/发布 API → `skip_publish=true`，Step 7 输出“可复制粘贴”的最终格式 + 手动发布说明
- 有发布 API → 在 Step 7 调用你的发布接口，并回写发布结果（链接/草稿 ID）

---

## 运行时的关键约束（建议不要删）

### 1) 降级标记要“向后传递”

一旦 Step 1 判定：
- `skip_websearch=true`：后续步骤不要再尝试 WebSearch，避免重复报错
- `skip_image_gen=true`：Step 6 自动切换为“提示词模式”
- `skip_publish=true`：Step 7 自动切换为“手动发布模式”

### 2) 不要整篇重写：用“定向修复”

在 Step 5 做质量验证时，推荐策略：
- 找到具体不达标的句子/段落
- 每轮最多改 1-3 处
- 立刻复检该项

这样能显著提升稳定性，也更符合“编辑”的工作方式。

### 3) 必须保留编辑锚点

模板要求每篇文插入 2-3 个编辑锚点，例如：
```html
<!-- ✏️ 编辑建议：在这里加一句你自己的经历/看法 -->
```
这是让“AI 初稿”变成“你的作品”的关键机制。

---

## 快速自测清单（你改完模板后建议跑一遍）

1. 用一句话触发：能否跑完 Step 1-8？
2. 断网/禁用 WebSearch：能否自动降级并完成（DONE_WITH_CONCERNS）？
3. 不接生图：Step 6 是否只输出提示词而不报错？
4. 不接发布：Step 7 是否输出可粘贴格式与说明？
5. 写作质量：是否有编辑锚点、是否引用了素材、是否避免“AI 话术”？

