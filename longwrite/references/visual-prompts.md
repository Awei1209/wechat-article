---
title: 视觉提示词规范（Visual Prompts）
---

# 视觉提示词规范（封面 + 内文）

> 用于 Step 6。若你没有接入图片生成器，可只输出提示词供人工/其他工具使用。

## 1) 通用要求

- 每条提示词必须包含至少 2 个“具体实体”：
  - 人物/职业、产品名、场景（办公室/地铁/咖啡馆）、数据点（“3 倍增长”）、行业物件（键盘/仪表盘/报表）
- 禁止只写“科技感/未来感/高级感”这类抽象词。
- 封面确定后抽取视觉锚点（色板/风格关键词/构图），内文配图必须复用锚点保证一致性。

## 2) 封面提示词模板（输出 3 套）

每套输出字段：
- `title_text`：封面上要出现的文字（如目标平台允许）
- `prompt`：画面描述
- `negative_prompt`：不希望出现的元素（如：水印、乱码字、过度拥挤）
- `style_anchor`：色板/风格（后续复用）

示例（占位）：
```yaml
title_text: "把 X 做对，只需要这 3 步"
prompt: "封面插画，场景：深夜办公室，人物：一位产品经理在白板前写流程图，白板上有 3 个清晰步骤，配色：深蓝+荧光绿，风格：干净矢量插画，留白充足，构图：左人右标题"
negative_prompt: "水印，乱码字，过度细节，低清晰度"
style_anchor: "deep navy + neon green, clean vector, high contrast, plenty of whitespace"
```

## 3) 内文配图类型（3-6 张）

按段落选择类型：
- 流程图（flowchart）
- 对比图（comparison）
- 时间线（timeline）
- 信息图（infographic）
- 场景图（scene）

每张图输出：
- `placement`：建议插入到哪个 H2 下面
- `caption`：图注（1 句）
- `prompt`：必须复用 `style_anchor`

