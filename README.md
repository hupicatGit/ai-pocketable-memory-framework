# AI Pocketable Wiki Framework

> 给中型项目的超轻量 AI 友好知识库。一个目录，即插即用。
>
> **完全兼容 [Obsidian](https://obsidian.md/)** — `[[双向链接]]`、YAML frontmatter、目录结构均原生支持。既是 AI 知识库，也是人类可用的 Obsidian vault。

## 为什么需要这个？

中型项目的知识散落在代码注释、飞书文档、口头约定里。AI Agent 每次新 session 都要从零了解项目，效率极低。

这个框架解决三个问题：

- **即插即用** — 复制一个 `wiki/` 目录到项目根，AI 立刻知道怎么用
- **跨 session 存活** — AI 自动将 wiki 信息写入长期记忆，下次启动无需重新发现
- **随用随长** — 每次任务发现新知识，AI 自动按流程沉淀回 wiki
- **人机共用** — 完全兼容 Obsidian，团队成员可以用 Obsidian 可视化浏览 AI 积累的知识

## 核心设计：分层摘要金字塔

```
sources（原始资料摘要）
    ↓ 提炼
concepts（概念）+ entities（实体）
    ↓ 聚合
overview（综合概述）
    ↓ 导航
index（总目录）
    ↓ 追记
log（变更日志）
```

AI 从 overview 获取全局视图，按需 drill-down 到具体条目。不需要一次性加载所有文档。

## 快速开始

### 1. 复制到你的项目

```bash
cp -r ai-pocketable-wiki-framework/wiki/ your-project/wiki/
cp ai-pocketable-wiki-framework/AGENTS.md your-project/
```

两行命令，完事。

### 2. 开始使用

`AGENTS.md` 会引导 AI 自动读取 `wiki/AI-OPERATION-SPEC.md`，然后它会：

1. 将 wiki 路径写入自己的长期记忆（跨 session 生效）
2. 以后每次处理项目任务，自动先查 wiki

## 目录结构

```
your-project/
├── AGENTS.md                  ← 触发文件（引导 AI 发现 wiki）
└── wiki/
    ├── AI-OPERATION-SPEC.md   ← AI 操作规范（AI 第一个读的文件）
    ├── index-总目录.md         ← 总目录
    ├── overview-综合概述.md    ← 项目核心知识提炼
    ├── log-变更日志.md         ← 只追加的变更记录
    ├── sources/               ← 原始资料摘要
    ├── concepts/              ← 概念条目（方法、术语、流程）
    ├── entities/              ← 实体条目（项目、团队、组件）
    └── comparisons/           ← 横向对比（方案 A vs B）
```

## 工作原理

### 三层发现保障

| 层级 | 机制 | 说明 |
|------|------|------|
| 1 | 长期记忆 | AI 读完规范后将 wiki 路径写入自身记忆，每次 session 自动注入 |
| 2 | 自动发现 | AI 扫描 wiki/ 目录时自动读取 AI-OPERATION-SPEC.md |
| 3 | 显式注册 | 项目配置文件中引用 wiki 规范（可选双保险） |

### 知识回流（7 步流程）

```
来源 → sources/ 摘要 → concepts/ 更新 → entities/ 更新 → overview/ 提炼 → index/ 更新 → log/ 追加
```

AI 每次发现新知识，按此流程自动沉淀，无需人工干预。

### 跨 Session 记忆

```
Session 1: 读 wiki → 发现 SVN 地址 → 写入 wiki → 更新长期记忆
                                                          ↓
Session 2: 自动注入记忆 → 直接知道 wiki 在哪 → 查到 SVN 地址 → 无需重新发现
```

## 兼容性

### Obsidian 原生兼容

本框架的目录结构和 Markdown 规范完全遵循 Obsidian 约定：

- **`[[双向链接]]`** — 概念、实体、来源之间互相引用，Obsidian 图谱视图可直接可视化知识网络
- **YAML frontmatter** — 每个文件的 `title`、`tags`、`last_updated` 等元数据，Obsidian 原生解析
- **目录分类** — `sources/`、`concepts/`、`entities/`、`comparisons/` 直接对应 Obsidian 的文件夹组织
- **零配置打开** — 用 Obsidian 打开项目根目录即可使用，无需额外设置

> 用 Obsidian 打开你的项目，立刻拥有一个可视化的 AI 知识库图谱。

### Agent 兼容性

| 平台 | 支持情况 |
|------|---------|
| Hermes Agent | ✅ 扫描目录读到 AGENTS.md |
| Claude Code | ✅ 读 CLAUDE.md / AGENTS.md |
| Codex | ✅ 读 .codex.md / AGENTS.md |
| Cursor | ✅ 读 .cursorrules / AGENTS.md |
| Obsidian | ✅ 完全兼容（[[链接]] + frontmatter） |
| GitHub / GitLab | ✅ Markdown 正常渲染 |
| 任何能读文件的 Agent | ✅ 可用 |

## 设计原则

- **零依赖** — 纯 Markdown，不需要数据库、服务器、任何工具链
- **AI 原生** — 不是"人用的文档加了 AI 兼容"，而是从第一天为 AI Agent 设计
- **渐进式** — 一个空目录就能开始，知识随使用自然积累
- **可追溯** — 每个知识条目都能追溯到原始来源

## 许可

无版权限制，自由用于任何项目。
