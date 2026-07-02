# AI Pocketable Wiki Framework

> 给中型项目的超轻量 AI 友好知识库。一个目录，即插即用。

## 为什么需要这个？

中型项目的知识散落在代码注释、飞书文档、口头约定里。AI Agent 每次新 session 都要从零了解项目，效率极低。

这个框架解决三个问题：

- **即插即用** — 复制一个 `wiki/` 目录到项目根，AI 立刻知道怎么用
- **跨 session 存活** — AI 自动将 wiki 信息写入长期记忆，下次启动无需重新发现
- **随用随长** — 每次任务发现新知识，AI 自动按流程沉淀回 wiki

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
```

### 2. 让 AI 知道它

打开 `wiki/AI-OPERATION-SPEC.md`，让 AI Agent 读一遍。它会自动：

1. 将 wiki 路径写入自己的长期记忆（跨 session 生效）
2. 在项目配置文件中注册 wiki 优先原则（双保险）

> 不需要手动配置任何东西。AI 读完规范后会自己处理。

### 3. 开始使用

下次 AI 处理项目任务时，它会：

1. 先查 wiki 是否有相关知识
2. 有 → 直接使用
3. 没有 → 执行任务，发现新知识后自动写入 wiki

## 目录结构

```
wiki/
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

| 平台 | 支持情况 |
|------|---------|
| Hermes Agent | ✅ 完整支持（memory 工具自动注册） |
| Claude Code | ✅ 支持（.claude/CLAUDE.md 注册） |
| Codex | ✅ 支持（.codex.md 注册） |
| Cursor | ✅ 支持（.cursorrules 注册） |
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
