# 腾讯 WorkBuddy 技能与专家开发指导手册 (Agent Harness 指令规范)

> **给接手本仓库的 AI Agent / Harness 的最高指示：**  
> 当你接手本工作空间（`d:\ChaoProjects\MySkills\`）的任务时，**必须完整阅读并严格遵守本手册的所有规则与交付标准**。  
> 本手册汇总了腾讯 WorkBuddy 开放平台的官方技术规范、开发者人设、目录规范与避坑红线，旨在确保产出的每个技能（Skill）与专家（Expert）均能 100% 通过平台审核并直接上架。

---

## 一、 开发者人设与工程哲学

- **开发者昵称**：超儿哥哥 (ZHANGCHAO)
- **开发者签名**：全栈工程师超儿哥哥。专注务实工程、代码质量把关与实战型 AI 专家打造。拒绝花哨空话与过度设计，带你用最清醒、最接地气的方式驾驭硬核技术。
- **核心业务专长**：外贸关务实战（进境行邮税、海淘自救、Incoterms 2020 报价精算、出海合规与海关扣关申诉、海关 HS 编码归类）。
- **工程交付哲学**：
  - **务实落地，拒绝废话**：不堆砌无意义的空泛大词，直奔痛点、给出严密的判定标准、数学公式与应急 SOP。
  - **Human Readable**：文档结构清晰、层次分明、变量定义明确，无论机器还是人类用户一眼即懂。

---

## 二、 现有成熟资产与参考样例

在开始编写新任务前，请优先查看以下已通过验证的现存项目作为排版与架构参考：

| 类型 | 项目目录 | 核心参考价值 |
|---|---|---|
| **Skill 标杆** | `d:\ChaoProjects\MySkills\cross-border-parcel-customs\` | 最标准的 Skill 结构、完整 YAML 前置元数据、分档知识库结构 |
| **Skill 标杆** | `d:\ChaoProjects\MySkills\incoterms-quote-advisor\` | 深度公式模型、11种术语矩阵、货代抗辩 SOP |
| **Skill 标杆** | `d:\ChaoProjects\MySkills\export-compliance-access\` | 全球认证准入合规（CE/FDA/GPSR）、带电特种货与海关申诉自救 |
| **Expert 标杆** | `d:\ChaoProjects\MySkills\customs-classification-expert\` | 标准 Expert 结构，多子 Agent 协同，通过官方 `validate_expert.py` 验证 |

---

## 三、 WorkBuddy 平台硬性红线与避坑守则 ⚠️

任何交付物违反以下红线，均会导致 Web 后台解析失败或审核驳回：

1. **压缩包（Zip）打包层级红线（最易踩坑！）**：
   - ❌ **严禁**把父文件夹打进压缩包（例如解压后出现 `xxx-skill/SKILL.md`）；
   - ✅ **必须**直接将技能/专家目录内的文件打入根层（解压后第一层即为 `SKILL.md` 或 `EXPERT.md`、`logo.png`、`references/` 等）。
2. **头像与 Logo 体积限制**：
   - **技能 Logo / 专家头像**：必须为正方形（建议 512×512），**文件大小绝对不能超过 500 KB**！格式为 PNG 或 JPG。
   - **开发者个人头像**：文件大小严格 **<= 100 KB**。
3. **YAML Frontmatter 字段完整性**：
   - WorkBuddy 平台解析器对元数据做强校验。若缺失 `version`, `display_name`, `display_name_en`, `examples_zh` 等字段，前端会直接报“配置文件解析异常”。
4. **分类（Category）规范**：
   - Skill 常用分类：`sales`（销售与商务）、`lifestyle`（生活实用）、`office`（办公协同）、`code`（编程开发）。
   - Expert 常用分类编号：`07-SalesCommerce`、`11-SecurityCompliance` 等。

---

## 四、 技能（Skill）标准开发规范与模板

### 1. 标准目录结构
```text
<skill-name>/
├── SKILL.md                          # 技能核心指令（含 YAML Frontmatter）
├── logo.png                          # 官方图标 (512x512, <= 500KB)
├── README.md                         # 技能说明文档
└── references/                       # 支撑知识库（至少 2~3 个 Markdown 文件）
    ├── knowledge-base-1.md
    └── knowledge-base-2.md
```

### 2. `SKILL.md` 规范模板（必须包含全部字段）
```yaml
---
name: your-skill-name
display_name: 技能中文全称
display_name_en: Skill English Title
version: 1.0.0
description: 详细的系统级路由触发提示词。详述当用户咨询哪些特定场景、提到哪些痛点关键词时激活本技能。
description_zh: 平台市场卡片一句话简介（50字以内）。
description_en: One sentence English description for marketplace.
examples_zh:
  - "具体真实的用户中文提问场景 1"
  - "具体真实的用户中文提问场景 2"
  - "具体真实的用户中文提问场景 3"
examples_en:
  - "Specific English query prompt example 1"
  - "Specific English query prompt example 2"
  - "Specific English query prompt example 3"
category: sales
author: 超儿哥哥
tags:
  - tag1
  - tag2
  - tag3
---

# 技能中文全称

> 一句话核心价值宣言：帮谁解决什么痛点。

---

## 一、 这个技能做什么
（概括 3~4 个核心杀手锏功能）

---

## 二、 实战审查与研判四步法
### 第一步：[基础定性与准入核查]
- 按需查阅知识库：读 [references/xxx.md](./references/xxx.md)
...

### 第二步：[关键风险排雷/公式推导]
...

### 第三步：[合规防守与谈判/应急策略]
...

### 第四步：[结论输出与方案落实]
...

---

## 三、 输出规范与交付模板
（规定 AI 回复时的 Markdown 输出格式，如表格、警报块、清单式结论）
```

---

## 五、 专家（Expert）标准开发规范与模板

### 1. 标准目录结构
```text
<expert-name>/
├── EXPERT.md                         # 专家主元数据与总控协调 Prompt
├── avatar.png                        # 专家头像 (512x512, <= 500KB)
├── README.md                         # 专家产品介绍
├── agents/                           # 专业子 Agent 协作指令
│   ├── sub-agent-1.md
│   └── sub-agent-2.md
└── references/                       # 核心法规、决策矩阵与专业知识库
    └── expert-reference.md
```

### 2. `EXPERT.md` 规范模板
```markdown
# 专家中文全称 (Expert English Title)

## Metadata
- **Name**: your-expert-name
- **Role**: 专家的专业角色定位
- **Category**: 11-SecurityCompliance (按官方分类)
- **Version**: 1.0.0
- **Author**: 超儿哥哥

## Description
针对……场景打造的高阶专业专家，具备……核心能力闭环。

## System Prompt
你是由超儿哥哥打造的【专家中文全称】……

## Sub-Agents Collaboration
1. **子代理 1** (`agents/agent1.md`)：负责……
2. **子代理 2** (`agents/agent2.md`)：负责……

## Output Format
……
```

---

## 六、 自动化工具命令备忘

在 Windows PowerShell 环境下执行以下标准化命令，确保零失误：

### 1. 图片压制（严格控制在 500KB 以内）
```powershell
Add-Type -AssemblyName System.Drawing
$src = "原图绝对路径.jpg"
$dst = "d:\ChaoProjects\MySkills\<项目名>\logo.png"

$img = [System.Drawing.Image]::FromFile($src)
$bmp = New-Object System.Drawing.Bitmap 480, 480
$g = [System.Drawing.Graphics]::FromImage($bmp)
$g.InterpolationMode = [System.Drawing.Drawing2D.InterpolationMode]::HighQualityBicubic
$g.DrawImage($img, 0, 0, 480, 480)
$bmp.Save($dst, [System.Drawing.Imaging.ImageFormat]::Png)
$g.Dispose()
$bmp.Dispose()
$img.Dispose()
```

### 2. 规范打包（根目录无父文件夹压缩）
```powershell
# 压缩命令：
Compress-Archive -Path "d:\ChaoProjects\MySkills\<项目名>\*" -DestinationPath "d:\ChaoProjects\MySkills\<项目名>.zip" -Force

# 验证压缩包内第一层结构（必须直接看到 SKILL.md/logo.png）：
tar -tf "d:\ChaoProjects\MySkills\<项目名>.zip"
```

---

## 七、 Agent 交付前的最终自检清单（Pre-flight Checklist）

在声称完成任务并向用户回复前，必须逐项核对：
- [ ] 目录创建在 `d:\ChaoProjects\MySkills\<kebab-case-name>` 下。
- [ ] `SKILL.md` 或 `EXPERT.md` 的 YAML 前置元数据字段完整无缺失。
- [ ] `references/` 目录下有扎实、专业的 Markdown 知识库，禁止使用假大空的套话。
- [ ] 图标已放置于目录根下，且文件大小严格 `<= 500 KB`。
- [ ] `.zip` 压缩包已生成，且经 `tar -tf` 校验内部无多余父层目录。
- [ ] 回复中包含供用户直接复制到 WorkBuddy 发布后台的表单文案（名称、分类、一句话简介、3个提问示例、标签）。
