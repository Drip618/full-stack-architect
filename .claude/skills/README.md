# Skill库使用规范

> **重要**: 所有对话框必须遵守本文件的分类规范和备份规则

---

## 📁 分类结构

```
skills/
├── INDEX.md                              # 总索引
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

## 📋 分类规则

### 1. people/ - 人物视角库（女娲蒸馏）
| 分类 | 存放内容 |
|------|---------|
| business/ | 商业/投资人物（Paul Graham, Elon Musk, Steve Jobs等） |
| tech/ | 科技/AI人物（Karpathy, Ilya等） |
| thinkers/ | 思想/教育人物（Feynman, Taleb等） |
| media/ | 媒体/内容人物（MrBeast, 特朗普等） |

### 2. creative/ - 创意/影视创作库
| 分类 | 存放内容 |
|------|---------|
| directors/ | 导演视角库（王家卫、诺兰等蒸馏Skill） |
| lingshu/ | 灵枢影视系统 |

### 3. code/ - 代码/架构库
| 内容 | 说明 |
|------|------|
| full-stack-architect | 全栈架构导师（你的系统） |

### 4. tools/ - 工具/框架库
| 内容 | 说明 |
|------|------|
| huashu-nuwa | 女娲人物蒸馏系统 |

## 🔄 新增流程

当新增Agent/Skill时，必须：
1. ✅ 判断其核心职责和领域
2. ✅ 放入对应分类文件夹
3. ✅ 更新 `INDEX.md` 索引文档
4. ✅ 执行备份和GitHub上传

## 💾 备份规则

每次完成以下操作后必须执行备份：
- 新增Agent/Skill/知识库
- 重大版本更新
- 批量修改文件

### 备份位置

| 系统 | 本地备份 | GitHub仓库 |
|------|---------|-----------|
| 全栈架构导师 | `/Users/Drip/TRAE CN/更新/全栈架构师/` | full-stack-architect |
| 灵枢影视系统 | `/Users/Drip/TRAE CN/更新/灵枢系统/` | lingshu-ai-director |
| 人物视角库 | `/Users/Drip/TRAE CN/更新/人物视角库/` | 合并到主系统仓库 |
| 工具/框架 | `/Users/Drip/TRAE CN/更新/工具库/` | 按需 |

### 上传流程

1. 在本地备份目录创建带日期的版本文件夹
2. 打包zip文件：`系统名_v版本_YYYYMMDD.zip`
3. 使用GitHub API上传到对应仓库
4. 创建Release或Tag标记版本

---

**📌 完整规范请查看 `docs/KNOWLEDGE_UPDATE_RULES.md`**
