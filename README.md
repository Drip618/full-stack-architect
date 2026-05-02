# 🏗️ 灵枢·AI全栈构建师 (Lingshu Full-Stack Architect)

<p align="center">
  <strong>AI驱动的全栈开发指导Agent</strong> · 集成ADK/Agent Runtime · 从需求到代码的完整链路
</p>

<p align="center">
  <a href="#-功能特性">功能</a> •
  <a href="#-模块架构">架构</a> •
  <a href="#-目录结构">目录</a> •
  <a href="#-快速开始">开始</a> •
  <a href="#-隐私声明">隐私</a>
</p>

---

## ✨ 功能特性

- **🤖 AI模型引擎** — 多模型管理（代码生成/文本生成/LLM调度）
- **⚡ 代码生成器** — 智能代码引擎 + 质量检查
- **🧠 NLP模块** — 对话管理 / 实体提取 / 意图识别
- **📋 PRD系统** — 产品需求文档自动生成、检查、执行（含6大行业模板）
- **🔧 技术栈推荐** — 智能技术选型 + 趋势分析
- **📚 知识参考库** — 19个领域最佳实践（前端/后端/数据库/安全/DevOps等）
- **🔗 ADK运行时** — Agent Runtime + 多平台适配器
- **🌐 Web界面** — 可视化交互控制台

## 🏗️ 模块架构

```
┌─────────────────────────────────────────────┐
│              灵枢·AI全栈构建师               │
├──────────┬──────────┬──────────┬────────────┤
│ AI模型引擎 │ 代码生成器 │ NLP模块   │ PRD系统    │
│          │          │          │            │
│ LLM调度   │ 引擎+质检 │ 对话/实体 │ 生成/检查  │
│ 模型管理  │          │ 意图识别  │ 执行       │
├──────────┴──────────┴──────────┴────────────┤
│         技术栈推荐 + 知识库(19领域)           │
├─────────────────────────────────────────────┤
│     ADK运行时 (Agent Runtime)                │
│     ┌─────────┐ ┌─────────┐ ┌──────────┐   │
│     │Avatar   │ │Platform │ │Framework │   │
│     │Controller│ │Adapter  │ │Enhance   │   │
│     └─────────┘ └─────────┘ └──────────┘   │
├─────────────────────────────────────────────┤
│              Web Interface                   │
└─────────────────────────────────────────────┘
```

## 📁 目录结构

```
.claude/skills/
├── code/
│   └── full-stack-architect/      # ← 核心入口
│       ├── SKILL.md               # Skill主文件（Agent入口）
│       ├── ai-models/             # AI模型引擎
│       │   ├── code_generation.py
│       │   ├── model_manager.py
│       │   └── text_generation.py
│       ├── code-generator/        # 代码生成器
│       │   ├── code_engine.py
│       │   └── code_quality.py
│       ├── nlp/                   # NLP模块
│       │   ├── dialogue_manager.py
│       │   ├── entity_extraction.py
│       │   └── intent_recognition.py
│       ├── prd-modules/           # PRD系统
│       │   ├── prd-generator.md
│       │   ├── prd-checker.md
│       │   ├── story-executor.md
│       │   └── templates/         # 6个行业模板
│       │       ├── ecommerce.md
│       │       ├── education.md
│       │       ├── fintech.md
│       │       ├── gaming.md
│       │       ├── healthcare.md
│       │       └── saas.md
│       ├── tech-stack/            # 技术栈推荐
│       │   ├── recommendation_algorithm.py
│       │   ├── tech_database.json
│       │   └── trends_analyzer.py
│       ├── references/            # 知识库(19领域)
│       │   ├── frontend_*.md      # 前端系列(6)
│       │   ├── backend_*.md       # 后端系列(5)
│       │   ├── database_*.md      # 数据库
│       │   ├── security_*.md      # 安全
│       │   └── ...
│       ├── integrations/          # ADK运行时
│       │   ├── adk_agent_runtime.py
│       │   ├── avatar_controller.py
│       │   ├── platform_adapters.py
│       │   └── framework_enhancements.py
│       ├── scripts/               # 工具脚本
│       ├── web-interface/         # Web界面
│       ├── knowledge_index.json   # 知识图谱索引
│       └── requirements.txt
├── prd-context/                   # PRD上下文与历史
│   ├── conventions/
│   └── prd-files/
├── docs/                          # 配置与规则
│   ├── daily_update_config.json
│   └── KNOWLEDGE_UPDATE_RULES.md
├── INDEX.md                       # 全局索引
└── README.md                      # 本文件
```

## 🚀 快速开始

### 前置要求

- Python 3.10+
- Node.js 18+ (Web界面)

### 安装

```bash
# 克隆仓库
git clone https://github.com/Drip618/full-stack-architect.git

# 安装Python依赖
cd full-stack-architect/.claude/skills/code/full-stack-architect
pip install -r requirements.txt
```

### 使用方式

本仓库作为 **TRAE/Claude Code Skill** 使用，自动被IDE加载：

1. 将仓库克隆到 `.claude/skills/code/` 目录
2. 启动 TRAE 或 Claude Code
3. 直接对话即可调用全栈构建能力

## 🔒 隐私声明

| 内容 | 状态 |
|------|------|
| 本仓库全部内容 | ✅ **可公开** |
| 影视/剧组相关 | ❌ 不包含（→ [lingshu-ai-director](https://github.com/Drip618/lingshu-ai-director)） |
| 用户项目数据 | ❌ 不包含 |

## 📊 版本信息

| 项目 | 值 |
|------|-----|
| **版本** | v2.0 |
| **许可证** | MIT |
| **语言** | Python 37.9% / JavaScript 37.5% / CSS 22.6% |
| **维护者** | [@Drip618](https://github.com/Drip618) |
| **最后更新** | 2026-05-02 |

## 📄 许可证

本项目基于 [MIT License](.claude/skills/code/full-stack-architect/LICENSE) 开源。

---

<p align="center">
  <sub>遵循 <a href=".trae/rules/project_rules.md">灵枢核心铁律 v1.0</a> 管理 · 违反铁律 = 严重错误</sub>
</p>
