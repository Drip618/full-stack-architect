# 灵枢核心铁律（IRON RULES）

> 以下规则为最高优先级，所有操作必须严格遵守，不得违反。
> 违反任何一条视为严重错误，必须立即纠正。

---

## 一、系统分离铁律（SYSTEM SEPARATION）

| 系统 | GitHub仓库 | 本地位置 | 隐私 |
|------|-----------|---------|------|
| 灵枢·AI全栈构建师 | `Drip618/full-stack-architect` | `.claude/skills/code/` | ✅ 可公开 |
| 灵枢·AI影视系统 | `Drip618/lingshu-ai-director` | `.claude/skills/creative/` | 🔒 私有 |
| 灵枢·Director Pro | （含在影视系统内） | `.claude/skills/creative/lingshu/lingshu-director-pro/` | 🔒 私有 |
| 女娲·人物蒸馏系统 | （含在影视系统内） | `.claude/skills/tools/huashu-nuwa/` | 🔒 私有（影视部分） |
| 灵枢·人物视角库 | （含在影视系统内） | `.claude/skills/people/` | 🔒 私有 |

**铁律1**：两个GitHub仓库完全独立，内容零交叉
**铁律2**：全栈仓库禁止包含任何影视/剧组/人物库/女娲相关文件
**铁律3**：灵枢仓库包含全部影视内容：creative/ + people/ + tools/huashu-nuwa/
**铁律4**：本地联动通过 `.claude/skills/` 目录实现，不需要跨仓库

---

## 二、目录结构铁律（DIRECTORY STRUCTURE）

```
/Users/Drip/TRAE CN/
├── .claude/                    ← 隐藏文件夹：所有Agent/Skill/知识库/资料库
│   ├── skills/
│   │   ├── code/              ← 全栈构建师（推送到 full-stack-architect）
│   │   │   └── full-stack-architect/
│   │   ├── creative/           ← 影视系统（推送到 lingshu-ai-director）
│   │   │   ├── lingshu/
│   │   │   │   ├── lingshu-creative-assistant/   ← 创作助手
│   │   │   │   ├── lingshu-director-pro/         ← 导演Pro
│   │   │   │   ├── lingshu-hotspot-aggregator/   ← 热点聚合
│   │   │   │   └── lingshu-platform-bridge/      ← 平台桥
│   │   │   ├── directors/        ← 18个导演Skill
│   │   │   ├── cinematographers/ ← 10个摄影Skill
│   │   │   ├── editors/          ← 10个剪辑Skill
│   │   │   ├── composers/        ← 10个作曲Skill
│   │   │   ├── production-designers/ ← 10个美术Skill
│   │   │   └── screenwriters/    ← 10个编剧Skill
│   │   ├── people/            ← 人物视角库
│   │   │   ├── business/       ← 商业人物
│   │   │   ├── tech/           ← 科技人物
│   │   │   ├── thinkers/        ← 思想人物
│   │   │   └── media/           ← 媒体人物
│   │   ├── tools/
│   │   │   └── huashu-nuwa/     ← 女娲蒸馏系统
│   │   ├── docs/               ← 配置文件（daily_update_config.json等）
│   │   └── INDEX.md            ← 全局索引
│   └── ...
├── 更新/                       ← 仅存放备份压缩包（zip），不放源码！
│   ├── 灵枢·AI影视系统/
│   │   └── 灵枢·AI影视系统_vX.X_YYYYMMDD.zip
│   ├── 灵枢·AI全栈构建师/
│   │   └── 灵枢·AI全栈构建师_vX.X_YYYYMMDD.zip
│   ├── 灵枢·AI人物视角库/
│   │   └── 灵枢·AI人物视角库_vX.X_YYYYMMDD.zip
│   ├── 灵枢·AI工具系统/
│   │   └── 灵枢·AI工具系统_vX.X_YYYYMMDD.zip
│   ├── 灵枢·AI配置文件/
│   │   └── 灵枢·AI配置文件_vX.X_YYYYMMDD.zip
│   ├── scripts/               ← 同步脚本（feishu_sync.py等）
│   ├── README.md
│   └── install.sh
├── 项目/                       ← 用户项目（永远不进入Git/备份/云端）
├── 工具/                       ← 工具软件
├── 应用/                       ← 应用程序
├── 配置/                       ← 配置文件
└── .git                        ← Git仓库根
```

**铁律5**：Agent、新Skill、知识库、资料库 → 统一放 `.claude/skills/` 隐藏文件夹
**铁律6**：`更新/` 目录 → **只放zip备份压缩包**，绝对不放源码目录
**铁律7**：`项目/` 目录 → 永远不纳入Git追踪、不备份、不上传

---

## 三、备份与上传铁律（BACKUP & UPLOAD）

### 备份规则
**铁律8**：每次备份只包含系统+知识库+资料库，**绝不带 `项目/`**
**铁律9**：备份格式为zip，命名规范 `系统名_v版本_日期.zip`
**铁律10**：备份前必须确认本地文件完整性（特别是filter-branch后要恢复误删文件）

### 云端上传规则
**铁律11**：每次推送GitHub/飞书**必须附带完整README说明文档**
**铁律12**：README必须包含：
- 仓库用途说明（一句话）
- 包含的系统/模块列表
- 目录结构说明
- 隐私声明（私有/公开）
- 版本信息
- 最后更新时间
**铁律13**：两个仓库分别维护各自的README，内容完全独立

### 飞书同步规则
**铁律14**：飞书知识库为私密库，每次同步标记 `🔒 私密知识库，禁止公开`

### GitHub仓库README铁律
**铁律14.1**：每个GitHub仓库根目录**必须有 `README.md`**
**铁律14.2**：README必须按GitHub专业标准撰写，包含：
- 仓库标题 + 一句话描述（首行）
- 功能特性列表（带emoji图标）
- 模块/子系统说明
- 目录结构树
- 快速开始/安装指南
- 隐私声明（私有/公开）
- 版本信息表（版本/许可证/语言/维护者/最后更新）
- 许可证链接
**铁律14.3**：README放在**仓库根目录**，不是子目录里
**铁律14.4**：每次推送后必须在GitHub上验证README正确显示
**铁律14.5**：版本更新时同步更新README中的版本号和更新日期

---

## 四、Skill管理铁律（SKILL MANAGEMENT）

**铁律15**：68个剧组Skill属于灵枢·AI影视系统，必须在 `creative/` 下
**铁律16**：新增Skill时必须更新 INDEX.md 和对应子系统的SKILL.md
**铁律17**：TRAE加载的Skill列表必须与 `.claude/skills/` 实际文件一致
**铁律18**：修改Skill后必须验证TRAE能正确读取（检查Skill描述是否最新）
**铁律19**：剧组调度协议(crew-dispatch-protocol)属于影视系统，不放入全栈仓库

---

## 五、隐私铁律（PRIVACY）

| 内容 | 可否公开 | 归属 |
|------|---------|------|
| 全栈构建师代码/知识库 | ✅ 可以 | full-stack-architect |
| 影视系统全部内容 | ❌ 私有 | lingshu-ai-director |
| Director Pro | ❌ 私有 | lingshu-ai-director |
| 女娲（影视部分） | ❌ 私有 | lingshu-ai-director |
| 人物视角库 | ❌ 私有 | lingshu-ai-director |
| 用户项目 | ❌ 绝不外泄 | 本地仅限 |

**铁律20**：涉及用户项目的内容（剧本/分镜/角色设定等）严禁上传任何云端

---

## 六、操作禁令（OPERATIONAL PROHIBITIONS）

❌ **禁止**：使用 `git filter-branch` 前不先创建备份
❌ **禁止**：将 `creative/` 或 `people/` 的内容推送到 `full-stack-architect`
❌ **禁止**：将 `code/` 的内容推送到 `lingshu-ai-director`
❌ **禁止**：在 `更新/` 下创建源码目录（只允许zip）
❌ **禁止**：将 `项目/` 纳入任何形式的备份或上传
❌ **禁止**：推送无README的仓库
❌ **禁止**：README放在子目录而非根目录
❌ **禁止**：README内容不完整（缺功能/目录/隐私/版本信息）
❌ **禁止**：删除 `.claude/skills/` 下的文件后不验证恢复
❌ **禁止**：修改系统配置不通知用户

---

## 七、分镜表输出格式规范（STORYBOARD OUTPUT）

> 所有 16 列分镜输出必须按以下标准执行。

### Markdown 输出标准（对话/文件查看）
- 16 列必须拆分为 **2 个 8 列表格**
- 每批显示 **5 镜**，用 `## SHOT X-Y` 标题分隔
- **表 1 = 画面层（8 列）**：镜号 | 开始 | 结束 | 时长 | 画面描述 | 故事内容 | 景别 | 角度
- **表 2 = 声音层+AI 生成层（8 列）**：镜号 | 运动 | 焦距景深 | 光线 | BGM | 音效 | 图像生成提示词 (Nano) | 视频运动提示词
- 文件命名：`第 XX 集·XXX_16 列分镜表.md`，与 Word 文件同目录

**禁止**：输出单表 16 列 Markdown（会被挤成竖排）、每镜单独显示、超过 8 列/表、省略任何列。

### Word 文档输出标准
- 横屏 A4（29.7×21.0cm），窄边距 0.5cm
- 单表格 16 列，Table Grid 细线网格
- 全微软雅黑，表头 7pt 加粗居中，内容 6.5pt
- 数字列居中，文本列左对齐，单元格垂直居中

详见：`V2.0AI 全流程制作/lingshu-storyboard-output-spec-v4.2.md`

---

*最后更新：2026-05-02 | 版本：v1.1 | 违反铁律 = 严重错误*
