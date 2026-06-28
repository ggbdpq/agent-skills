# ggbdpq/agent-skills

公开的 Agent Skills 仓库，面向 Codex、Claude Code、Cursor 等支持 Agent Skills 的工具。

## 安装

整仓安装：

```bash
npx skills add ggbdpq/agent-skills
```

按单个 skill 安装：

```bash
npx skills add https://github.com/ggbdpq/agent-skills --skill minimal-diff-fix
npx skills add https://github.com/ggbdpq/agent-skills --skill tech-writer
npx skills add https://github.com/ggbdpq/agent-skills --skill mentor-me
```

安装完成后，重启你的 agent 工具以加载新 skill。

## 目录结构

```text
skills/
├─ .curated/       # 通用、稳定、推荐公开安装的 skills
└─ .experimental/  # 试验中的 skills
```

当前已提供：

- `minimal-diff-fix`：在尽量不扩散改动面的前提下定位并修复局部问题，适用于小范围 bugfix 和回归修复。
- `tech-writer`：翻译英文技术文档或把中文技术文档打磨成发布级终稿，适用于技术文档英译中、终稿化和发布前质检。
- `mentor-me`：像导师带学徒一样循序渐进把一个概念从零讲到能上手，结合用户当下项目举例，适用于想学懂某个概念或需要通俗讲解的场景。

## 维护原则

- `SKILL.md` 是跨工具核心文件
- `agents/openai.yaml` 仅作为 Codex / OpenAI 产品侧增强元数据
- 新 skill 默认先进入 `.experimental/`
- 稳定后再移动到 `.curated/`

## 新增自定义 Skill

后续新增 skill，建议遵循这套最小结构：

```text
skills/.experimental/<skill-name>/
├─ SKILL.md
├─ agents/
│  └─ openai.yaml        # 可选，给 OpenAI / Codex 侧展示名和短描述
├─ references/          # 可选，放检查清单、模板、补充说明
├─ assets/              # 可选，放模板文件或静态素材
└─ scripts/             # 可选，放校验脚本或辅助工具
```

建议流程：

1. 先在 `skills/.experimental/` 下创建新 skill。
2. 在 `SKILL.md` 里写清楚用途、适用场景、不适用场景、执行流程和输出形式。
3. 如需更好的安装或展示体验，再补 `agents/openai.yaml` 的 `display_name`、`short_description` 和 `default_prompt`。
4. 如果 skill 依赖模板、检查清单或脚本，再按需补 `references/`、`assets/`、`scripts/`。
5. 先在实验阶段验证稳定性，再移动到 `.curated/`。

提交建议：

- 新增实验技能：`feat(experimental): add <skill-name> skill`
- 调整技能文案或说明：`docs(skill): refine <skill-name> docs`
- 将技能提升为稳定版：`feat(curated): promote <skill-name> skill`
- 更新索引文档：`docs(readme): update skill catalog`
