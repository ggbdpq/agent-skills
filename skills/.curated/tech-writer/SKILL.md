---
name: tech-writer
description: |
  技术文档总入口 skill：既能将英文 Markdown、系统提示词或 JS/TS 注释翻译为简体中文，
  也能把已有中文技术文档改写为发布级终稿。适用于文档英译中、翻译后终稿化、CLI 手册、
  教程、功能指南和技术博客的发布前处理。
  触发词："翻译技术文档""翻译 Markdown""技术文档润色""改写为发布级"
  "去 AI 味并结构化""文档翻译后润色""英译中""发布前检查""Markdown 质量门禁"。
---

# Tech Writer：技术文档翻译与终稿化

一个入口，三种模式：

- 英文到中文的结构保持翻译
- 已有中文文档的发布级终稿化
- 先翻译，再做终稿化

## 适用场景

- 英文 Markdown、系统提示词、政策文档、技术文档英译中
- 已有中文初稿，但结构、语气、格式还不够稳定
- 翻译稿需要继续做发布级润色、结构整理和最终质检

## 不适用场景

- 用户只要摘要或流程提炼，而不是完整文档处理
- 只做事实核查，不要求翻译或改写
- 原文本身不是技术文档、系统提示词、教程或操作说明

## 模式选择

### 翻译模式

- 输入是英文原文
- 目标是忠实翻成中文
- 重点是结构锁定、代码块与占位符不变

### 终稿模式

- 输入已经是中文
- 目标是变成面向程序员的发布级终稿
- 重点是结构、文风、核验路径、排版和质量门禁

### 双模式

- 先做翻译模式
- 再以翻译结果进入终稿模式
- 适用于“英文原文直接产出中文终稿”的请求

## 翻译模式硬约束

- 代码块内容逐字不变；只允许翻译纯注释行
- 占位符、变量、标签、锚点、URL、文件路径完全一致
- YAML frontmatter 只翻 value，不翻 key，不改顺序
- HTML 标签数量和配对关系保持不变
- 不为“更自然”而改写事实边界或章节结构

## 工作流程

1. 先判模式
   先判断当前任务属于翻译模式、终稿模式，还是双模式。

2. 走翻译模式时
   先识别代码块、YAML、占位符、标签、链接和路径，只翻自然语言。

3. 翻译模式基础自检
   完成后读取 [references/self-check-list.md](references/self-check-list.md) 做结构自检。
   如果发现结构错误，再读取 [references/error-fix-table.md](references/error-fix-table.md) 做定点修复。

4. 翻译模式发布级处理
   当任务包含术语统一、专名核对、锁版或逐节记录时，必须读取：
   - [references/heading-and-layout-spec.md](references/heading-and-layout-spec.md)
   - [references/termbook-template.md](references/termbook-template.md)
   - [references/release-lock-checklist.md](references/release-lock-checklist.md)
   - [references/section-term-log-template.md](references/section-term-log-template.md)

   只有明确需要章节骨架时，才读取 `assets/*`。
   发布前运行：

   ```bash
   python scripts/check_translation_lock.py <markdown-file>
   ```

5. 走终稿模式时
   先读取 [references/structure-templates.md](references/structure-templates.md) 选择最接近的模板，再做结构重排、最小示例补充和命令上下文补充。

6. 做终稿语言净化
   读取 [references/anti-patterns.md](references/anti-patterns.md)，删除 AI 腔、营销式表达、空洞过渡句、武断断言和元叙述。

7. 做终稿排版和质量门禁
   读取 [references/formatting-rules.md](references/formatting-rules.md) 统一排版。
   再读取 [references/quality-checklist.md](references/quality-checklist.md) 完整自检。
   如有未通过项，再读取 [references/troubleshooting.md](references/troubleshooting.md) 对应修复。

8. 双模式收尾
   如果是双模式，必须在翻译完成后再跑一次终稿模式，不要把“翻译完成”当成“可以直接发布”。

## 核心判断

对每一段都问这三个问题：

1. 这段对程序员有没有信息增量
2. 这段能不能让读者自己验证
3. 这段是不是在解释“怎么做”，而不是空泛描述“为什么很重要”

## 反模式

- 不要在翻译模式下修改代码块、占位符、链接和 YAML key
- 不要把结构保持翻译做成自由改写
- 不要直接润色原句而不先收结构
- 不要保留“此外”“值得注意的是”“综上所述”这类填充句
- 不要堆命令而不解释用途、上下文和预期结果
- 不要写“所有版本都支持”“一定可以解决”这类无法核验的断言
- 不要把 AI 味改成另一种 AI 味；要改成程序员真会写出来的技术文风
- 不要在双模式里跳过终稿阶段

## 输出要求

- 输出完整 Markdown 全文，不截断
- 不输出改写过程说明或 diff
- 不提及所用模式、工具或过程

## 与其他 Skill 的边界

- 只需要快速提炼长文主题、核心观点和关键判断时，先用 `extracting-article-essence`
- 本 skill 负责翻译和终稿化，不负责长文精华提炼
