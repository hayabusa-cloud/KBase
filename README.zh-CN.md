# KBase

面向 AI 编程代理的结构化自组织知识库。

[English](README.md) | [中文](README.zh-CN.md) | [Español](README.es.md) | [日本語](README.ja.md) | [Français](README.fr.md)

## 背景

KBase 在会话之间持久化代理知识——规则、参考资料、决策、技能——使代理从已知状态启动，而非从零开始。

## 结构

| 目录 | 用途 |
|------|------|
| `ROOT/` | 元原则、引导流程、命令 |
| `refs/` | 经验证的外部知识 |
| `notes/` | 项目洞察与决策 |
| `skills/` | 自动化工作流 |
| `rules/` | 约束与规范 |
| `env/` | 环境配置 |
| `perms/` | 密钥与凭证（已 gitignore） |
| `hooks/` | 事件驱动回调 |
| `plans/` | 自动生成的任务计划 |

每个目录包含两个文件：`AGENTS.md`（代理指南）和 `INDEX.md`（内容索引）。

## 原则

1. **主动持久化** — 代理立即保存发现，无需等待指令。
2. **KBase 优先** — 先搜索内部知识再查外部来源。顺序：rules、notes、refs、skills，最后外部。
3. **权威来源** — 所有知识须经一手来源验证。未验证的知识不予存储。
4. **文档先行** — 先更新文档，再据其行动。冲突时更新低层级来源。
5. **技能调度** — 执行临时工作前先检查已有技能。

## 通信协议

KBase定义了人机交互的结构化记法。完整规范：[`ROOT/communication.md`](ROOT/communication.md)

| 记法 | 语义 |
|------|------|
| `[[ ... ]]` | 必须交付物 — 不可省略 |
| `[ ... ]` | 可选交付物 |
| `(( ... ))` | 必须约束 — 违反则blocked |
| `( ... )` | 推荐约束 — 尽力而为 |
| `( ... ) > ~( ... )` | 优先＋抑制 — 优先左侧，减少右侧倾向 |
| `?( ... )` | 验证查询 |
| `< ... >` | 输入上下文 |
| `<< ... >>` | 引用指针 |
| `->` | 控制流 — `?( 条件 ) -> [ 动作 ]` |
| `[ X ] : ( Y )` | 审查修正（软） — 审查X是否满足Y，不满足则修正 |
| `[[ X ]] : (( Y ))` | 审查修正（硬） — 深度审查X，必须满足Y |

状态转换：`ready` → `done`（正常）或 `ready` → `blocked`（冲突/输入缺失）。

## 权威层级

| 层级 | 来源 |
|------|------|
| 1 | `ROOT/` |
| 2 | `rules/` |
| 3 | `refs/` |
| 4 | `notes/` |
| 5 | 外部 |

高层级覆盖低层级。冲突时更新低层级来源。

## 生命周期

| 转换 | 条件 |
|------|------|
| notes → refs | 经一手来源验证 |
| notes → rules | 模式反复出现，成为约束 |
| plans → notes/rules | 计划完成，提取结论 |

## 快速开始

1. 克隆：`git clone https://github.com/hayabusa-cloud/KBase.git`
2. 将 AI 编程代理的配置指向 `KBase/` 目录。
3. 开始工作。代理在会话启动时读取 `AGENTS.md` 并自维护知识库。

## 自动保存

代理自动将发现保存到相应目录：

| 发现类型 | 目标目录 |
|---------|---------|
| 约束、规范 | `rules/` |
| 洞察、决策、模式 | `notes/` |
| 经验证的外部知识 | `refs/` |
| 自动化工作流 | `skills/` |
| 任务计划 | `plans/` |

手动添加内容时，在目标目录中创建 Markdown 文件并在其 `INDEX.md` 中添加条目。

## 引导流程

会话启动时，代理按以下顺序读取：

`AGENTS.md` → `ROOT/AGENTS.md` → `ROOT/instructions.md` → `rules/INDEX.md` → `skills/INDEX.md` → `notes/INDEX.md`

上下文压缩后，代理至少重新读取前三个文件。

## 命令

```
research <topic>     收集并验证信息，保存至 refs/ 或 notes/
dive into <topic>    多轮深度研究，使用前沿来源
review <subject>     对照 KBase 与网络研究进行评估
tidy <scope>         重组结构与内容；tidy all 整理整个 KBase
recall <topic>       检索 KBase 已有知识，再查外部来源
save <topic>         将发现保存到相应目录并更新 INDEX.md
promote <file>       将笔记提升至 refs/ 或 rules/；更新两侧 INDEX.md
```

## 许可证

[MIT](LICENSE) — ©2026 [Hayabusa Cloud Co., Ltd.](https://code.hybscloud.com)
