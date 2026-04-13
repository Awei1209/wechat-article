---
title: Onboard（首次风格设置）
---

# Onboard：建立你的写作风格配置（style.yaml）

当 `{skill_dir}/style.yaml` 不存在时，进入 Onboard。目标是一次性拿到足够信息，避免后续反复追问。

## 需要向用户确认的问题（建议最多 6 个）

1. **账号/作者名**：你希望文末署名是什么？
2. **目标平台**：公众号 / 博客 / 知乎 / Newsletter / 其他？
3. **目标读者**：主要写给谁？（新人/从业者/管理者/投资人/泛人群）
4. **主战主题**：3-5 个长期主题（如：AI 工具、效率、产品、增长）
5. **语气与人设**：更像朋友聊天 / 行业观察 / 冷静分析 / 犀利评论？
6. **黑名单**：不想出现的词、口头禅、价值观表达、敏感话题（可空）

## 生成 style.yaml（示例）

> 你可以把字段改成自己的格式；关键是后续步骤能读到这些信息来约束写作。

```yaml
name: "你的账号名"
author: "署名"
platform: "wechat"
topics:
  - "AI 工具"
  - "效率方法"
tone: "克制但有温度"
voice: "第一人称为主，允许自我怀疑与自我修正"
blacklist:
  - "赋能"
  - "颠覆"
content_preferences:
  length_range: [1500, 2300]
  include_edit_anchors: true
  cite_sources: true
```

## Onboard 完成口径

创建/更新 `{skill_dir}/style.yaml` 后，提示用户：
- “已保存风格配置。以后你只要说：**写一篇长文** / **写一篇公众号文章** 就可以全自动生成。”

