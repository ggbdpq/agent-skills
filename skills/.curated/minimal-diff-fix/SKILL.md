---
name: minimal-diff-fix
description: 强制采用“快速失败、最小改动、先解除阻断”的修复方式，只修当前直接阻断链，不做顺手重构、兜底逻辑或范围扩张。适用于 Codex 处理明确 bug、启动失败、本地环境阻断、回归修复、最小 diff / minimal patch、unblock-first、scope creep 风险高，或用户明确强调“只改必要部分”“不要加 fallback”“不要改脚本/依赖/约定”“先跑起来再说”的场景。
---

# 最小改动修复

## 概览

只解决用户当前目标的直接阻断点。优化目标是“最小可审阅 diff”，而不是完整性、整洁性、优雅性或未来可扩展性。
本 skill 追求最小 diff，不追求顺手让代码库更小或更整洁。

只有在范围开始漂移，或者你需要具体案例来判断某个改动到底是不是“必要改动”时，才去读 `references/cases.md`。如果直接阻断链已经很清楚，就不要加载它。

## 铁律

- Fail fast and loudly. Do not add fallback logic unless the user explicitly requires fallback behavior.
- Let exceptions and real errors surface early. Do not absorb business-layer errors just to make the UI look calmer.
- Write tests only when they first prove the current bug exists by failing. Do not expand scope with test infrastructure when the user's goal is immediate unblocking.

这三条保持原文，不做软化，不做二次包装。

## 执行流程

1. 先用一句话复述用户目标。
2. 从报错或失败现象往回追，找完整的直接阻断链。
3. 在动手前，先给所有候选改动分三类：
   - `required`：不做它，用户当前目标仍然失败
   - `optional`：更整洁、更安全、更完整，但当前不是必须
   - `off-scope`：文档、脚本、重构、helper、测试基建、顺手修的相邻问题
4. 只实现 `required`。
5. 优先在现有文件里原地修复，不优先新建文件、helper、adapter 或抽象层。
6. 阻断一旦解除就停下，把可选后续列出来，不要默认一起做。

## 决策规则

- 如果问题本质上是代理、服务、环境变量、登录态之类的运行前置条件缺失，先明确说出缺了什么、要执行什么命令。除非用户要求，不要写代码去绕过它。
- 如果命令、README、脚本名、依赖名、版本号看起来不一致，但并不阻断用户当前路径，不要顺手统一。
- 如果一个问题只需要“一个平台判断 / 一个 import 改法 / 一个现有文件里的条件分支”就能解决，就不要抽成共享 helper。
- 如果你准备新增文件，先问自己：删掉这个文件会不会让用户当前目标重新失败？如果不会，就不要加。
- 如果你准备捕获异常，先问自己：用户有没有明确要求“优雅恢复”或“降级处理”？如果没有，就让错误自然暴露。
- 如果你准备修改失败路径之外的行为，立刻停下，重新检查范围。

## 反模式

- 不要“既然来了顺手清理一下”。
- 不要为了让小修复看起来更完整，就补测试、补文档、补 wrapper。
- 不要因为你觉得名字更顺，就去重命名现有命令或改脚本入口。
- 不要为用户没有要求支持的路径添加兼容层。
- 不要把一个直接 bug 修复扩展成通用框架或通用体系。
- 不要在下层吞掉异常，再在上层发明一套新 UI 状态去解释它。

## 不适用场景

- 用户明确要求 fallback、优雅降级、重试、容错或错误恢复。
- 任务目标是重构、统一规范、清理代码、提升可维护性或做结构性优化。
- 根因还不清楚，需要先做系统化排障或证据收集。

## 替代路径

- 根因不明确时，先用系统化调试类 skill，先查清阻断链，再决定是否回到本 skill。
- 用户明确要求 test-first 或完整测试流程时，改用 TDD 类 skill，不要在这里硬塞测试基础设施。
- 用户明确要做容错、重试、错误处理体验时，改用专门的错误处理或 UX 类 skill。

## 输出方式

- 先说根因，再说补丁。
- 说明每个修改文件为什么是“必须改”。
- 如果有意不改某些东西，而且这能减少 review 歧义，就明确说出来。
- 如果第二个问题只是运行前置条件，直接给命令，不要写代码绕过。
