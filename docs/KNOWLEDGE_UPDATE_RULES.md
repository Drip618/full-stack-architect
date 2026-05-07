# 灵枢知识库更新规范

> **版本**: v1.0 | **创建日期**: 2026-04-24
> **适用范围**: 所有灵枢技能的知识更新操作

---

## 一、知识库文件清单

| 知识库文件 | 路径 | 格式 | 追加方式 |
|-----------|------|------|---------|
| 影视技术知识库 | `.claude/skills/lingshu-director-pro/scripts/knowledge_base.json` | JSON数组 | 脚本追加 or 手动追加 |
| 全球风格库 | `.claude/skills/lingshu-creative-assistant/references/global-style-library.md` | Markdown | 手动追加 |
| 创作知识库 | `.claude/skills/lingshu-creative-assistant/references/knowledge-base.md` | Markdown | 手动追加 |

## 二、影视技术知识库更新规则

### 追加条件
当联网检索到以下类型的新知识时，应追加到 `knowledge_base.json`：
- 新的摄影/运镜技法
- 新的灯光方案与参数
- 新的美术置景案例
- 新的调色风格与LUT参数
- 新的剪辑技法与节奏分析
- 新的视效/声音/音乐制作技术
- 新的AI视频/图像生成工具与最佳实践

### 节点格式（必须严格遵守）
```json
{
  "id": "DOP_186",
  "department": "DOP",
  "content_text": "## 英文名称\n中文名称\n\n### 分类\n主分类 / 子分类\n\n### 描述\n技术说明...\n\n### 视觉效果\n观众感知...\n\n### 适用场景\n什么剧情用...\n\n### 组合用法\n可与其他技法组合...\n\n### AI适配\nAI工具适配说明...\n\n### 示例提示词\n英文Prompt片段...",
  "applicability_context": "适用于[场景类型]，当需要[情绪/效果]时使用",
  "ai_translation": "English keywords for AI generation",
  "physical_params": {
    "movement_type": "Camera Movement",
    "source": "联网检索来源URL"
  },
  "source": "WebSearch: 检索关键词",
  "created_at": "2026-04-24",
  "keywords": ["关键词1", "关键词2"]
}
```

### 部门代码
| 代码 | 部门 | ID前缀 |
|------|------|--------|
| DOP | 摄影 | DOP_XXX |
| PD | 美术 | PD_XXX |
| LD | 灯光 | LD_XXX |
| CD | 服装 | CD_XXX |
| MU | 妆发 | MU_XXX |
| SO | 声音 | SO_XXX |
| ED | 剪辑 | ED_XXX |
| VFX | 视效 | VFX_XXX |
| MX | 音乐 | MX_XXX |
| ALL | 通用 | ALL_XXX |

### ID分配规则
- 读取当前 knowledge_base.json 中该部门的最大ID数字
- 新节点ID = 部门代码 + "_" + (最大数字 + 1)，补零到3位

## 三、全球风格库更新规则

### 追加条件
当联网检索到新的导演/摄影师/剪辑师/调色师等创作者风格时，追加到 `global-style-library.md`。

### 条目格式
```
编号 | 名称 (English) | 中文描述 | [English tags]
```

### 分类卷
| 卷号 | 分类 | 文件 |
|------|------|------|
| 01 | 导演 | global-style-library.md#导演 |
| 02 | 编剧 | global-style-library.md#编剧 |
| 03 | 摄影 | global-style-library.md#摄影 |
| 04 | 剪辑 | global-style-library.md#剪辑 |
| 05 | 美术 | global-style-library.md#美术 |
| 06 | 服装 | global-style-library.md#服装 |
| 07 | 化妆 | global-style-library.md#化妆 |
| 08 | 灯光 | global-style-library.md#灯光 |
| 09 | 调色 | global-style-library.md#调色 |
| 10 | 音频 | global-style-library.md#音频 |
| 11 | 视效 | global-style-library.md#视效 |
| 12 | 制片 | global-style-library.md#制片 |

## 四、创作知识库更新规则

### 追加条件
当联网检索到以下内容时，追加到 `knowledge-base.md`：
- AI影视制作新工具/新模型
- AI视频生成最佳实践
- Prompt工程新技巧
- 工作流优化方案
- 行业趋势分析

### 条目格式
```markdown
## [分类] 知识标题
- **来源**: URL
- **采集日期**: YYYY-MM-DD
- **标签**: #tag1 #tag2
- **内容**: 详细描述...
```

## 五、文件大小管理

当单个知识库文件超过以下大小时，按部门/分类拆分为多个文件：

| 文件 | 拆分阈值 | 拆分策略 |
|------|---------|---------|
| knowledge_base.json | 500KB | 按部门拆分：knowledge_dop.json, knowledge_ld.json 等 |
| global-style-library.md | 200KB | 按卷拆分为独立文件：style_01_director.md 等 |
| knowledge-base.md | 200KB | 按分类拆分：kb_tools.md, kb_workflow.md 等 |

## 六、SKill库分类规范

### 必须遵守的分类结构

所有新创建的Agent、Skill、知识库、资料库必须按照以下分类存放：

```
skills/
├── INDEX.md                              # 总索引（必须更新）
├── people/                               # 人物视角库
│   ├── business/                         # 商业/投资人物
│   ├── tech/                             # 科技/AI人物
│   ├── thinkers/                         # 思想/教育人物
│   └── media/                            # 媒体/内容人物
├── creative/                             # 创意/影视创作库
│   ├── directors/                        # 导演视角库
│   └── lingshu/                          # 灵枢影视系统
├── code/                                 # 代码/架构库
├── tools/                                # 工具/框架库
├── docs/                                 # 文档区
└── archive/                              # 归档区
```

### 分类规则

| 分类 | 存放内容 |
|------|---------|
| **people/** | 女娲蒸馏的人物视角Skill |
| **people/business** | 商业/投资/创业领域人物（Paul Graham, Elon Musk, Steve Jobs等） |
| **people/tech** | 科技/AI领域人物（Karpathy, Ilya等） |
| **people/thinkers** | 思想/教育领域人物（Feynman, Taleb等） |
| **people/media** | 媒体/内容创作人物（MrBeast, 特朗普等） |
| **creative/** | 影视创作相关系统 |
| **creative/directors** | 导演视角库（王家卫、诺兰等导演的蒸馏Skill） |
| **creative/lingshu** | 灵枢影视系统（创意助手、导演Pro等） |
| **code/** | 代码开发/架构设计相关（全栈架构导师、PRD系统） |
| **tools/** | 工具/框架系统（女娲蒸馏系统等） |
| **docs/** | 文档和规则文件 |
| **archive/** | 归档区（旧的/不活跃的内容） |

### 新增流程

当新增Agent/Skill时：
1. 判断其核心职责和领域
2. 放入对应分类文件夹
3. 更新 `INDEX.md` 索引文档
4. 执行备份和GitHub上传

---

## 七、备份与上传规范

### 备份规则

每次完成以下操作后必须执行备份：
- 新增Agent/Skill/知识库
- 重大版本更新
- 批量修改文件

### 备份位置

| 系统 | 备份位置 | GitHub仓库 |
|------|---------|-----------|
| **灵枢·AI全栈构建师** | `/Users/Drip/TRAE CN/更新/灵枢·AI全栈构建师/` | https://github.com/Drip618/full-stack-architect |
| **灵枢·AI影视系统** | `/Users/Drip/TRAE CN/更新/灵枢·AI影视系统/` | https://github.com/Drip618/lingshu-ai-director |
| **灵枢·AI人物视角库** | `/Users/Drip/TRAE CN/更新/灵枢·AI人物视角库/` | 合并到对应系统仓库 |
| **灵枢·AI工具系统** | `/Users/Drip/TRAE CN/更新/灵枢·AI工具系统/` | 按需 |

### 上传流程

1. 在本地备份目录创建带日期的版本文件夹
2. 打包zip文件：`系统名_v版本_YYYYMMDD.zip`
3. 使用GitHub API上传到对应仓库
4. 创建Release或Tag标记版本

### GitHub仓库对应

| 内容类型 | GitHub仓库 |
|---------|-----------|
| 灵枢·AI全栈构建师系统 | `full-stack-architect` |
| 灵枢·AI影视系统 | `lingshu-ai-director` |
| 灵枢·AI人物视角库 | 合并到对应系统仓库 |
| 灵枢·AI工具系统 | 保留本地或创建独立仓库 |

---

## 八、不可修改的文件

以下文件是代码/配置文件，**绝对不能手动修改**：
- 所有 `.py` 脚本文件
- 所有 `SKILL.md` 技能定义文件
- `SKILL_PROMPT_ENGINEERING.md` Prompt规范文件
- `SKILL_VIDEO_WORKFLOW.md` 工作流规范文件
- `scout_config.json` 侦察配置文件
- `package.json` 项目配置文件
