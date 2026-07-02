# 项目知识库 — AI 操作规范

> AI 通过 `AGENTS.md` 发现本文件。读取后立即执行下方行为守则。

## 项目根自定位

> **AI 读取本文件后，从此文件的绝对路径推导项目根**：
> ```
> wiki/ 所在目录 → 向上一级 = <project_root>
> ```
> 例如：本文件在 `D:\Work\MyProject\wiki\AI-OPERATION-SPEC.md`，则 `<project_root>` = `D:\Work\MyProject`。
>
> **所有后续操作中的绝对路径均使用 `<project_root>` 代指项目根目录。**

## AI Agent 行为守则（读取后立即执行）

### 第 0 步：长期记忆注册（最关键，跨 session 生效）

> **为什么要这一步？** Agent 的每次会话是独立的。如果不把 wiki 信息写入长期记忆，下次会话 agent 会完全忘记 wiki 的存在，需要重新发现。长期记忆在每次会话开始时自动注入，是唯一能跨 session 存活的机制。

**立即执行**：将以下内容通过 Agent 的 `memory` 工具写入长期记忆：

```
## 项目知识库 (wiki)
路径: <wiki目录的绝对路径>
入口: <wiki目录绝对路径>/AI-OPERATION-SPEC.md
规则: 处理本项目任务前先读入口文件，查 wiki 再动手，新知识按 7 步流程写回 wiki
```

> **写入目标**：Agent 自身的长期记忆（不是项目文件）。不同 Agent 的实现：
>
> | Agent | 写入方式 |
> |---|---|
> | Hermes | `memory` 工具（target=memory），内容为上述格式 |
> | Claude Code | 项目根 `.claude/CLAUDE.md` + 建议用户在 CLAUDE.md 中加一条 `记住：本项目有 wiki 知识库` |
> | Codex | 项目根 `.codex.md` |
> | Cursor | 项目根 `.cursorrules` |
> | 其他 | 任何支持持久化记忆的机制 |

### 第 1 步：项目配置文件注册（可选但推荐）

将"Wiki 优先原则"写入项目配置文件，作为双保险：

> 不同 Agent 的写入方式：
>
> | Agent | 写入目标 |
> |---|---|
> | Claude Code | `<project_root>/.claude/CLAUDE.md` |
> | Codex | `<project_root>/.codex.md` |
> | Cursor | `<project_root>/.cursorrules` |
> | Hermes | 用 memory 工具保存（已在第 0 步完成） |
> | 其他 | 项目根下对应配置文件 |
>
> 写入内容：
> ```
> ## Wiki 优先原则
> 处理本项目任务前，先检索 wiki/ 目录（先读 AI-OPERATION-SPEC.md 再查 index-总目录.md）。
>
> ## 知识回流
> 任务中发现的新信息，按 wiki/AI-OPERATION-SPEC.md 的 7 步流程写入 wiki。
> ```

### 第 2 步：Wiki 优先

每次接到项目任务，**先查 wiki 再动手**。
   - a. 读取 `index-总目录.md` 了解现有条目
   - b. 按中英文关键词搜索 wiki 目录文件名和内容
   - c. 即使无结果，也要在回答中说明「已检索 wiki，未找到相关条目」

### 第 3 步：知识回流

发现的任何新信息按下方 7 步流程写入 wiki，随任务一起提交。

### 第 4 步：更新长期记忆

当 wiki 新增了重要条目（尤其是 entities/ 和 concepts/ 下的条目）时，更新长期记忆中的 wiki 摘要，确保下次 session 能快速定位。

> **示例**：新增了 `entities/project-svn-addresses.md` 后，更新 memory 中的 wiki 条目：
> ```
> ## 项目知识库 (wiki)
> 路径: G:\MyProject\wiki
> 入口: G:\MyProject\wiki\AI-OPERATION-SPEC.md
> 规则: ...
> 已有重要条目:
>   - entities/project-svn-addresses: 项目工程SVN地址一览
> ```

## 目录结构

```
wiki/
├── AI-OPERATION-SPEC.md  ← 本文件（AI 操作规范，Agent 先读这个）
├── index-总目录.md       ← 总目录
├── log-变更日志.md       ← 变更日志（只追加）
├── overview-综合概述.md  ← 综合概述（核心知识提炼）
├── sources/              ← 原始资料摘要
├── concepts/             ← 概念条目（方法、术语、流程）
├── entities/             ← 实体条目（项目、团队、组件）
└── comparisons/          ← 横向对比
```

## 页面规范

### YAML Frontmatter（每个 md 文件顶部必须有）

```yaml
---
title: 标题
tags: [标签1, 标签2]
source_count: 3         # 来源数（overview 页用）
last_updated: YYYY-MM-DD
---
```

### 文件命名

`{英文slug}-{中文名称}.md`

| 目录 | 命名 | 示例 |
|------|------|------|
| sources/ | `{目录}-{英文slug}-{中文名称}.md` | `sources/codebase-skill-search-技能系统代码调研.md` |
| concepts/ | `{英文slug}-{中文名称}.md` | `concepts/config-export-workflow-配置导出流程.md` |
| entities/ | `{英文slug}-{中文名称}.md` | `entities/project-overview-项目概览.md` |
| comparisons/ | `{英文slug}-{中文名称}.md` | `comparisons/planA-vs-planB-方案A对比方案B.md` |

### 交叉引用

使用 `[[链接]]` 语法（Obsidian 兼容），链接目标为文件名（不含路径前缀）。

### 路径约定

所有项目内路径相对于 `<project_root>`。外部路径（URL、网络共享）保持绝对。

### 标签规范

中英双语，英文逗号分隔，覆盖多个维度方便检索。

### 矛盾处理

发现与已有内容矛盾时，添加 `⚠️ 矛盾` 章节，注明矛盾点和来源。

## 新增资料流程（7 步）

```
来源 → sources/摘要 → concepts/更新 → entities/更新 → overview/提炼 → index/更新 → log/追加
```

1. **完整阅读**来源
2. **sources/**：创建摘要页面（frontmatter + 关键信息 + `[[相关链接]]`）
3. **concepts/**：更新或新建所有相关概念页面
4. **entities/**：更新或新建所有相关实体页面
5. **overview/**：核心论点有变化时更新 `overview-综合概述.md`
6. **index/**：新页面加入 `index-总目录.md` 对应分类
7. **log/**：在 `log-变更日志.md` 追加 `## YYYY-MM-DD 摄入 | 来源名称`

> ⚠️ **overview 与 index 必须同步**：index 新增的条目必须同步加入 overview 的分类列表。

## 维护原则

- **先查后写**：修改前先查 wiki，避免重复或矛盾
- **写后即提**：wiki 更改随任务一起提交，不积压
- **只追加不覆盖**：log 只追加，不修改历史
- **来源可追溯**：每个概念/实体通过 sources 追溯到原始资料
