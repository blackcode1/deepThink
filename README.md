# deepThink 小龙虾

> AI 驱动的"思维管家" —— 捕捉碎片灵感 → 引导深度思考 → 构建个人知识图谱 → 一键生成并传播内容

## 项目简介

deepThink 小龙虾是一个基于 [OpenClaw](https://github.com/openclaw) 框架构建的 AI Agent 应用。它以轻量化方式收集用户随手记下的思考笔记（支持多端语音/文字输入），自动生成结构化思维日志，定期总结沉淀，并通过 AI 模拟深度访谈推动思考深化，最终形成知识网络化和周期复盘，能够自动生成文章/播客，一键发布到小红书等平台，实现思维的体系沉淀与传播。

**解决四大痛点：** 知识碎片化 · 思考难以深入 · 灵感易遗忘 · 输出效率低

## 系统架构

```
~/.openclaw/workspace/
├── IDENTITY.md          # Agent 人格定义（名字、性格、语气）
├── SOUL.md              # Agent 行为准则与价值观
├── USER.md              # 用户画像（偏好、习惯、上下文）
├── BOOTSTRAP.md         # 首次启动引导流程
├── HEARTBEAT.md         # 定时心跳任务配置
│
├── thought_logs/        # 思维日志（JSONL 格式，按天归档）
│   ├── 2026-03-18.jsonl
│   └── 2026-03-19.jsonl
│
├── memory/              # 会话记忆（Markdown 格式，按日期+主题命名）
│   ├── 2026-03-19-thought-log.md
│   ├── 2026-03-19-problem-definition.md
│   ├── 2026-03-18-seeddance-insight.md
│   └── ...
│
├── skills/              # 核心业务技能
│   ├── scheduled-interview/   # 定时深度访谈
│   ├── daily-digest/          # 日度摘要
│   ├── weekly-review/         # 周度复盘
│   └── xiaohongshu-ops/       # 小红书运营（选题→写作→发布→复盘）
│
├── .agents/skills/      # 通用 Agent 技能（多 IDE 共享）
│   ├── podcast/         # 播客生成
│   ├── tts/             # 语音合成
│   ├── asr/             # 语音转文字
│   ├── image-gen/       # AI 图片生成
│   ├── content-parser/  # URL 内容解析
│   └── explainer/       # 解说视频生成
│
└── .<ide>/skills/       # 多 IDE 适配层
    ├── .cursor/         # Cursor
    ├── .claude/         # Claude Code
    ├── .windsurf/       # Windsurf
    ├── .trae/           # Trae
    └── ...              # 支持 20+ IDE/Agent 环境
```

## 核心工作流

### 1. 碎片捕捉

用户通过飞书私聊、语音或文字随时输入灵感碎片。系统自动进行：
- 文本规范化与语义提取
- 分类打标（insight / question / observation）
- 成熟度评估（raw_fragment → emerging_idea → developing_thesis → stable_insight）
- 写入 `thought_logs/` 目录的 JSONL 日志

### 2. 深度访谈（scheduled-interview）

基于已积累的思维日志，在合适时机发起 AI 模拟访谈，遵循六阶段模型：

| 阶段 | 名称 | 目标 |
|------|------|------|
| Stage 1 | 回顾 (recall) | 梳理已有记录 |
| Stage 2 | 澄清 (clarify) | 厘清核心概念 |
| Stage 3 | 追因 (reason) | 追问逻辑与因果 |
| Stage 4 | 挑战 (challenge) | 提出反面论证 |
| Stage 5 | 抽象 (abstract) | 提炼为框架或原则 |
| Stage 6 | 应用 (apply) | 迁移到具体场景 |

每轮只问一个高价值问题，不凭空追问，不重复已说清楚的内容。

### 3. 定期沉淀

- **日度摘要（daily-digest）**：每天自动生成当日思考总结，识别值得深入的候选主题
- **周度复盘（weekly-review）**：每周生成思维演化报告，识别哪些主题变清晰、哪些洞察已稳定、哪些可以转化为内容

### 4. 内容生成与传播

思维成熟后，一键转化为多种内容形式：
- **文章** — 精炼的小红书文案、公众号长文
- **播客** — 基于主题自动生成对话式播客
- **解说视频** — 配合 AI 生成画面的视频内容
- **语音** — TTS 朗读与配音

通过 `xiaohongshu-ops` 技能实现小红书端到端运营：选题 → 内容生产 → 发布 → 评论互动 → 复盘。

## 数据流

```
用户输入（飞书/语音/文字）
    ↓
思维日志（thought_logs/*.jsonl）—— 结构化存储每条碎片
    ↓
会话记忆（memory/*.md）—— 保留完整对话上下文
    ↓
AI 深度访谈 —— 推动思考从碎片到体系
    ↓
日度摘要 / 周度复盘 —— 定期沉淀与趋势识别
    ↓
内容输出（文章/播客/视频）—— 一键发布到社交平台
    ↓
飞书文档同步 —— 所有阶段成果自动同步到飞书
```

## 技术特性

- **基于 OpenClaw 框架**：统一的 Agent 技能体系，支持技能的模块化安装与组合
- **多 IDE 适配**：同一套核心技能可在 Cursor、Claude Code、Windsurf、Trae 等 20+ 环境中运行
- **飞书深度集成**：通过飞书私聊收集碎片、同步思维日志与总结文档
- **结构化思维日志**：JSONL 格式，每条记录包含原始文本、规范化文本、分类、标签、成熟度、子思考、后续候选问题等字段
- **记忆持久化**：Agent 每次唤醒时从文件系统加载记忆，实现跨会话的连续性

## 项目资料

- **项目说明书**：https://cloud.tsinghua.edu.cn/f/47c909ebf7ad4aa5ba0d/
- **项目海报**：https://cloud.tsinghua.edu.cn/f/19c6c2971a3b4d41810b/
- **演示视频**：https://www.bilibili.com/video/BV1N5w1ztEo1/?spm_id_from=333.1387.homepage.video_card.click&vd_source=5c3567881b2fba8ec8a0b8c2ccd84925
