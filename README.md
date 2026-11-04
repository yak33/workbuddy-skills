# WorkBuddy Skills & Agents (超儿哥哥的实战技能与专家集)

> 🚀 专为腾讯 WorkBuddy 打造的工业级实战 AI 技能（Skills）与专家（Agents）仓库。  
> 秉持 **务实工程、Human Readable、拒绝过度设计与空洞大词** 的交付哲学，覆盖**跨境关务合规**、**国际贸易精算**与**工业级代码审查**等核心场景。

---

## 📦 现有技能与专家矩阵

| 类型 | 资产名称 | 目录 | 核心定位与专长 |
|---|---|---|---|
| **Skill** | **不废话代码审查**<br>`no-bs-review` | [`no-bs-review/`](./no-bs-review/) | 拒绝教条主义与过度设计，按红线零容忍、黄线讲清理由的工业级规范把关合并前代码质量。跨语言通用底座，按需挂载 Java/前端专项深度。 |
| **Expert** | **海关归类与关税专家**<br>`customs-classification-expert` | [`customs-classification-expert/`](./customs-classification-expert/) | 十五年关务实战经验，专精 10 位 HS 编码 GIR 1-6 推导、规范申报要素提炼、RCEP 优惠关税筹划及海关查验合规抗辩。 |
| **Skill** | **跨境个人包裹邮税与清关指引**<br>`cross-border-parcel-customs` | [`cross-border-parcel-customs/`](./cross-border-parcel-customs/) | 聚焦海关总署最新进境包裹限值与行邮税则，提供应税额精准核算、退运/商业报关红线判别及海淘被税自救指南。 |
| **Skill** | **出海商品合规准入与海关查验申诉**<br>`export-compliance-access` | [`export-compliance-access/`](./export-compliance-access/) | 覆盖欧美主流市场准入认证（CE/FCC/FDA/GPSR/CPC）、带电带磁特种货合规、海关扣关查验实战申诉 SOP。 |
| **Skill** | **国际贸易术语与跨境物流报价顾问**<br>`incoterms-quote-advisor` | [`incoterms-quote-advisor/`](./incoterms-quote-advisor/) | 深度解析 Incoterms 2020 规则，提供 11 种贸易术语权责矩阵、综合到岸价全链路精算模型与货代虚高附加费抗辩。 |

---

## 📂 仓库目录结构

```text
MySkills/
├── no-bs-review/                     # [Skill] 不废话代码审查
│   ├── SKILL.md                      # 技能定义与四步审查流
│   ├── README.md                     # 使用说明与触发词
│   ├── logo.png                      # 技能图标
│   └── references/                   # 专项深度规则库 (Java/前端/安全性能)
├── customs-classification-expert/    # [Expert] 海关归类与关税专家
│   ├── .codebuddy-plugin/            # 专家插件元数据配置 (plugin.json)
│   ├── agents/                       # 核心 Prompt (GIR方法论/SOP)
│   ├── avatars/                      # 专家头像资源
│   └── README.md                     # 专家说明文档
├── cross-border-parcel-customs/      # [Skill] 跨境个人包裹邮税与清关
├── export-compliance-access/         # [Skill] 出海商品合规准入与海关查验
├── incoterms-quote-advisor/          # [Skill] 国际贸易术语与物流报价
├── WORKBUDDY_DEVELOPMENT_GUIDE.md    # 腾讯 WorkBuddy 官方技术开发指导手册
├── EXPERT_SECURITY_NOTE.md           # 专家数据安全与内网零接口说明
└── README.md                         # 仓库总说明文档
```

---

## 🛠️ 打包与导入 WorkBuddy

根据 [`WORKBUDDY_DEVELOPMENT_GUIDE.md`](./WORKBUDDY_DEVELOPMENT_GUIDE.md) 的平台硬性规范：

### 1. 打包原则（避坑守则 ⚠️）
- ❌ **严禁**将外层文件夹打进 Zip（解压后不能出现父文件夹）；
- ✅ **必须**直接选中子目录内的所有文件与文件夹进行压缩，确保解压后的根层直接是 `SKILL.md`（技能）或 `plugin.json` / `agents/`（专家）。
- 🖼️ **图标/头像体积限制**：必须严格控制在 **500 KB** 以内（建议 512×512 像素）。

### 2. 导入与发布
- 登录腾讯 WorkBuddy 开发者后台，选择“新建技能”或“新建专家”；
- 上传对应的 ZIP 压缩包；
- 平台校验通过后，即可一键发布并分发使用。

---

## 🧭 后续演进规划

- [ ] **代码审查体系**：持续扩充 Python、Go 等语言的专属避坑指南与最佳实践；
- [ ] **关务合规引擎**：跟踪 2026 最新海关税则公告、自贸协定（FTA）税率更新与通关典型案例；
- [ ] **引入更多领域专家**：规划扩展跨境财税筹划、合同合规初审、架构重构顾问等新领域 Skill/Expert。

---

## 👨‍💻 开发者信息

- **开发者**：超儿哥哥 (`ZHANGCHAO`)
- **工程理念**：*代码首先写给人看，其次才是写给机器执行。简单不是低级，清晰本身就是高级工程能力。*
- **License**：[MIT License](LICENSE)

