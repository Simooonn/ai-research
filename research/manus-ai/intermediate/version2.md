# Manus AI 深度调研：从中国创业到被 Meta 20亿美元收购的完整故事

> 调研日期：2026年2月2日 | 版本：v2.0 | 字数：约7,500字

---

## 执行摘要

1. **闪电退出**：Manus AI 于2025年3月6日正式发布，同年12月29日被 Meta 以超20亿美元收购，从产品上线到被收购仅9个月，是 AI 历史上最快的"从零到巨额退出"案例之一 [S12][S14]。

2. **收入奇迹**：Manus 在上线8个月后即突破1亿美元 ARR，总收入 run rate 达1.25亿美元，是全球增速最快的 AI 创业公司，采用 $39-$199/月的订阅定价模式 [S23][S24]。

3. **Acquihire 结构**：此次收购采用 Silicon Valley 常见的 acquihire 模式——Meta 并未直接收购公司股权，而是通过核心团队集体转移与技术许可实现能力获取，以规避反垄断与外国投资审查 [S18]。

4. **地缘政治风暴**：中国商务部于2026年1月8日宣布将对此次收购进行出口管制、技术进出口及海外投资合规审查，审查可能持续六个月以上 [S6][S8][S16][S17]。

5. **Meta AI 战略**：此次收购是 Meta 成立以来第三大收购（仅次于 WhatsApp 和 Instagram），反映了 Zuckerberg 将 AI 列为公司首要优先事项、承诺三年投入6000亿美元基础设施的战略决心 [S28]。

6. **创始人传奇**：创始人肖弘（Xiao Hong），江西90后，腾讯青藤计划校友，从 Monica AI 浏览器插件起步，一路打造出中国 AI 领域少数实现盈利的应用产品，收购后出任 Meta VP [S12][S13][S14]。

---

## 研究问题与范围

本报告聚焦以下核心问题：

- **Manus AI 是什么？** 其产品定位、技术架构与商业模式如何？
- **为什么 Meta 愿意花超过20亿美元收购一家成立仅9个月的 AI 产品？** Meta 的战略动机是什么？
- **Acquihire 模式的法律结构是什么？** 为什么选择这种方式？
- **中国政府审查意味着什么？** 对交易本身和更广泛的中美科技竞争有何影响？

**研究范围**：覆盖 Butterfly Effect 公司2021年创立至2026年1月中国深入审查的全过程。数据来源包括 Reuters、Bloomberg、CNBC、TechCrunch、36氪、澎湃新闻、中国商务部官方声明等中英文权威媒体及官方公告。

---

## 团队与公司背景

### 创始人肖弘

肖弘（Xiao Hong），1990年代出生于江西，华中科技大学毕业，腾讯青藤计划（Qingteng Program）校友 [S13][S14]。在创业前，肖弘已在中国互联网行业积累了丰富的产品和技术经验。他的创业理念核心是"less structure, more intelligence"——减少结构化约束，让 AI 更加智能地自主行动 [S1]。

> **注**：部分早期媒体报道中出现"季一超"等不同名字写法，经多方交叉验证，创始人中文名为肖弘 [S27]。

### Butterfly Effect 公司历程

| 时间节点 | 关键事件 |
|---|---|
| 2021年 | Butterfly Effect（蝴蝶效应）公司创立，最初根基在北京 [S2][S13][S14] |
| 2022年 | 以 Monica AI 浏览器插件进入市场，提供聊天、搜索、阅读、写作、翻译等功能 [S13][S14] |
| 2023年8月 | 在新加坡注册 Butterfly Effect Pte. Ltd. [S1] |
| 2025年初 | 开始筹备 Manus 产品 |
| 2025年3月6日 | Manus AI 正式发布 [S1] |
| 2025年4月 | 获 Benchmark 领投的7500万美元融资，估值5亿美元 [S3] |
| 2025年4月底 | 开始考虑将总部迁出中国，以应对中美关系紧张 |
| 2025年7月 | 总部正式迁至新加坡 [S4] |
| 2025年10月16日 | 发布 Manus 1.5 [S1] |
| 2025年12月17日 | 宣布突破1亿美元 ARR [S24] |
| 2025年12月29日 | Meta 宣布收购 [S3][S9] |
| 2026年1月8日 | 中国商务部宣布审查 [S8][S16] |
| 2026年1月23日 | Bloomberg 报道中国深化审查 [S6] |

### 团队规模与构成

Butterfly Effect 是一个相对精干的团队。据36氪报道，收购消息传出后"团队人均身价过亿" [S21]。公司在新加坡、东京和旧金山设有团队。此前红杉中国和腾讯曾向公司投资超过1000万美元 [S26]，Benchmark 在2025年4月的融资中领投7500万美元 [S3]。

---

## 产品与技术

### 产品定位：通用 AI Agent

Manus（源自拉丁语，意为"手"）定位为世界首个通用 AI Agent——不同于传统聊天机器人只回答问题，Manus 能够自主执行多步骤任务 [S1][S30]。

官方文档如此描述：

> "Unlike traditional chatbots that simply answer questions, Manus AI takes action." [S30]

### 核心能力

Manus 在完整的 sandbox 环境中运行——即虚拟计算机，具备互联网访问、持久文件系统、安装软件和创建自定义工具的能力 [S30]。其能力矩阵包括：

| 能力类别 | 具体功能 |
|---|---|
| 研究与分析 | 自主执行深度研究、数据分析、市场调研 |
| 内容生成 | 创建幻灯片、撰写报告、内容本地化 |
| 开发与设计 | 建网站、开发应用、UI 设计 |
| 数据处理 | 数据清洗、自动提醒、结构化输出 |
| 个人助理 | 简历制作、专业头像、项目提案 |
| 浏览器操作 | Browser Operator，直接操控网页完成任务 |

[S11][S29]

### 技术架构

Manus 采用多 Agent 架构（Multi-Agent Architecture），这是其核心技术竞争力。根据官方披露的数据 [S24]：

- **多 Agent 性能**：Claude Opus 4 多 Agent 相比单 Agent 性能提升90.2%
- **Benchmark 表现**：在 GAIA（General AI Assistants）评测中达到 SOTA（State-of-the-Art），超越 OpenAI Deep Research [S2]
- **处理规模**：自发布以来处理超过147万亿（147T）tokens，创建超过8000万个虚拟机实例 [S24]

### 产品版本演进

- **Manus 1.0**（2025年3月6日）：首发版本，引发全球关注，邀请码在黑市被炒到约14,000美元（约合人民币数万元）[S22]
- **Manus 1.5**（2025年10月16日）：在速度和可靠性方面大幅提升，尤其在研究和 Web 开发任务上表现更优 [S1]

---

## 融资历程与商业数据

### 融资时间线

| 轮次 | 时间 | 金额 | 领投方 | 估值 |
|---|---|---|---|---|
| 早期投资 | 2021-2023年 | $1000万+ | 红杉中国、腾讯 | 未公开 [S26] |
| 融资轮 | 2025年4月 | $7500万 | Benchmark | $5亿 [S3] |
| 收购前估值 | 2025年12月 | — | — | $20-30亿 [S3][S10] |

### 收入与增长数据

Manus 的商业数据令人震撼：

- **ARR 突破1亿美元**：上线仅8个月（2025年3月→11月），全球最快 [S24]
- **总收入 run rate**：$1.25亿，包含订阅收入和 usage-based 收入 [S4][S23]
- **月环比增长**：自 Manus 1.5 发布以来保持20%以上的月环比增长 [S24]

### 定价策略

Manus 采用分层订阅模式 [S23]：

| 方案 | 月费 | 目标用户 |
|---|---|---|
| 基础版 | $39/月 | 个人用户、轻度使用 |
| 专业版 | $199/月 | 高频用户、专业场景 |
| Team Plan | 定制 | 企业团队 |

这一定价在 AI 应用中属于中高端，反映了 Manus 运行虚拟机、处理大量 tokens 的高运营成本。

### 用户画像与地理分布

一个值得注意的特征是 Manus 的全球化用户结构 [S23]：

- **巴西**：33.37%（最大单一市场）
- 其他市场分布于东南亚、北美、欧洲

这种"非中国本土主导"的用户分布，一方面说明产品的全球化能力，另一方面也使得公司迁至新加坡的决策更具合理性。

### 基础设施合作

Manus 与阿里云建立了合作关系，优化其全球部署的可扩展性 [S23]。在模型层面，Manus 依赖第三方 LLM（如 Anthropic 的 Claude），这既是技术优势（可灵活切换最优模型），也是潜在风险（对供应商的依赖）。

---

## Meta 收购事件

### 收购时间线

| 日期 | 事件 |
|---|---|
| 2025年12月中旬 | Meta 与 Butterfly Effect 开始正式接触 [S10][S14] |
| 约10天后 | 双方达成最终协议 [S10][S14] |
| 2025年12月29日 | Meta 官方宣布收购 [S3][S9][S20] |
| 2025年12月30日 | 各大媒体密集报道 |

整个谈判过程仅约10天多——这在科技行业大型收购中极为罕见 [S10][S14]。

### 收购规模

- **交易价值**：超过20亿美元（部分报道称估值区间为$20-30亿）[S3][S10]
- **Meta 历史排名**：第三大收购，仅次于 WhatsApp（$190亿）和 Instagram [S22][S28]

> **注**：有来源称 Scale AI（$143亿）排在第二 [S28]，但 Instagram 收购（2012年约$10亿）从时间维度看排名更靠前。Meta 对 Manus 的收购金额显著高于 Instagram 当年的收购价。

### 收购结构：Acquihire 模式

这次收购的法律结构值得深入分析。据 Geopolitechs 报道，Meta 采用了 acquihire 模式 [S18]：

**Acquihire 的核心特征：**

1. **不购买公司股权**：Meta 没有获得 Butterfly Effect 的股权或控制权
2. **公司形式独立存续**：Butterfly Effect 作为独立实体继续存在 [S11]
3. **核心团队集体转移**：肖弘及核心团队整体加入 Meta
4. **技术许可转移**：通过技术许可或合作安排转移关键能力
5. **法律性质定位**：被界定为"普通商业合作和人才流动"而非"资产收购"

**为什么选择 Acquihire？**

这种结构设计的核心目的是规避监管审查 [S18]：

- **反垄断审查**：不构成传统意义上的市场集中，无需进行反垄断通报
- **外国投资审查**：不涉及外资对中国实体的控制权变更
- **出口管制审查**：在法律形式上不构成技术资产的跨境转让

然而，正如后续事件所证明的，中国监管机构仍然对此交易产生了高度关注。

### 收购后安排

- **肖弘**：出任 Meta VP，向 Meta COO Javier Olivan 汇报 [S11][S14]
- **运营**：继续在新加坡独立运营 [S11]
- **中国联系**：Business Insider 报道 Meta 将"完全切断 Manus 的中国联系" [S5]
- **产品整合**：Reuters 报道 Meta 将把 Manus 整合进其产品矩阵，包括 Meta AI [S3]
- **官方声明**：Manus 官网已更新为"Manus is now part of Meta" [S29]
- **Meta 首席 AI 官 Alexandr Wang**：在社交媒体上发文欢迎 Manus 团队 [S28]

---

## Meta 的战略动机：为什么买 Manus？

### AI 竞争格局中的 Meta

要理解这次收购，需要将其放在 Meta 的 AI 战略全景中审视。

**Meta 面临的竞争压力：**

| 竞争对手 | 核心优势 | Meta 的差距 |
|---|---|---|
| OpenAI | GPT 系列模型、ChatGPT 生态、Stargate 联盟 | Meta 的 Llama 模型虽开源领先，但产品化落后 |
| Google | Gemini 模型、搜索+云+硬件全栈、Android 生态 | Meta 缺乏操作系统和搜索入口 |
| Anthropic | Claude 模型、安全对齐研究领先、企业级 API | Meta 在 AI 安全研究上投入相对不足 |
| ByteDance | 豆包/Coze Agent 平台、TikTok 全球用户 | Meta 在 Agent 产品上缺乏成熟方案 |

**Meta 的 AI 三大支柱：**

1. **模型层**：Llama 系列开源模型，社区影响力大但商业化路径不清晰
2. **基础设施层**：承诺三年投入$6000亿建设美国 AI 基础设施 [S28]，成立 Meta Compute 顶级组织
3. **应用层**：Meta AI 助手、WhatsApp/Instagram AI 功能——但缺乏真正的 **Agent 能力**

### Manus 填补了什么空白？

Manus 恰好填补了 Meta 在 **Agent 执行层** 的关键空白：

1. **从对话到行动的跨越**：Meta AI 目前仍以对话式交互为主，Manus 能够将其升级为真正的任务执行引擎
2. **硬件协同**：Meta 的 Ray-Ban 智能眼镜、Quest 头显等硬件产品，需要能够自主执行任务的 Agent 才能释放潜力
3. **商业验证**：$1.25亿 ARR 证明用户愿意为 Agent 服务付费，这是 Meta 急需的商业化样本
4. **人才获取**：在全球 AI 人才争夺白热化的背景下，一次性获得一支经过市场验证的 Agent 团队 [S19]
5. **速度需求**：Mark Zuckerberg 多次强调 AI 是公司 #1 优先事项，内部从零构建 Agent 平台可能需要1-2年，收购 Manus 可以将时间压缩到几个月

### $6000亿基础设施计划的背景

Meta 在2025年11月宣布了未来三年投入$6000亿建设美国 AI 基础设施的计划 [S28]。这一金额超过了 Meta 上市15年以来的总收入。在如此激进的基础设施投资下，Meta 需要足够多的 AI 应用来"消化"这些算力——而 Manus 的虚拟机密集型架构正好是算力的重度消费者。

---

## 中国政府审查与地缘政治

### 审查时间线

| 日期 | 事件 |
|---|---|
| 2025年12月29日 | 收购宣布 |
| 2026年1月初 | 中国国内舆论关注升温 |
| 2026年1月8日 | 中国商务部发言人何亚东在例行记者会上宣布将进行审查 [S8][S16] |
| 2026年1月9日 | 商务部进一步说明：将与相关部门评估和调查此收购是否符合出口管制、技术进出口、海外投资等法律法规 [S16] |
| 2026年1月12日 | Trivium China 分析指出此交易已"不再仅是一次科技收购，而是演变为中美地缘政治的零和博弈" |
| 2026年1月23日 | Bloomberg 报道中国深化审查，扩展至跨境资金流动、税务核算和海外投资合规 [S6] |

### 审查涉及的法律框架

商务部明确提及的审查维度包括 [S16]：

1. **出口管制法**：AI Agent 技术是否属于受管制的技术出口
2. **技术进出口管理条例**：技术许可安排是否构成技术出口
3. **海外投资管理规定**：交易是否符合中国企业海外投资合规要求
4. **跨境数据传输**：用户数据是否可能被传输至美国公司（新增审查维度）[S6]
5. **跨境资金流动与税务**：交易资金安排是否合规（新增审查维度）[S6]

### Acquihire 结构能否规避审查？

Geopolitechs 的分析指出了一个深层矛盾 [S18]：

- **形式上**：Acquihire 不构成传统收购，不触发市场集中审查或外国投资审查门槛
- **实质上**：核心团队转移、技术许可安排在功能上等同于技术出口
- **监管回应**：中国商务部显然选择了"穿透"法律形式，审视交易实质

SCMP 报道称审查可能持续六个月以上 [S17]，这意味着即使交易在法律形式上已完成，后续仍可能面临合规修正甚至处罚要求。

### 更广泛的地缘政治背景

这次审查不是孤立事件，而是中美科技竞争大背景下的必然：

- **类比 TikTok**：正如 Trivium China 所指出的，Manus 交易与 TikTok 被强制出售形成了某种镜像——一个是中国要求审查"技术流出"，一个是美国要求审查"数据安全"
- **人才争夺**：National Interest 将此次收购放在"美中技术竞争中吸收 AI 人才"的框架下分析 [S19]
- **信号效应**：如果中国对 Manus 交易采取强硬立场，将对未来所有中国 AI 创业公司的国际化退出路径产生深远影响

---

## 行业影响与竞争格局

### 对 AI Agent 赛道的影响

Manus 被收购标志着 AI 行业从**"聊天机器人时代"向"Agent 时代"的范式转移**正式获得了资本市场的最高级别认可。

**Agent 赛道关键玩家对比：**

| 公司/产品 | 定位 | 状态 |
|---|---|---|
| Manus (Meta) | 通用 AI Agent | 已被 Meta 收购 |
| OpenAI Operator | ChatGPT Agent 功能 | 持续迭代中 |
| Anthropic Computer Use | Claude 桌面操控 | 已发布 |
| Google Gemini Agent | 多模态 Agent | 开发中 |
| Devin (Cognition) | AI 软件工程师 | 垂直 Agent |
| ByteDance Coze | Agent 开发平台 | 活跃运营 |

### 对中国 AI 创业生态的影响

1. **退出路径收窄**：中国审查可能使得未来中国 AI 创业公司被海外巨头收购变得更加困难
2. **新加坡模式受质疑**：Manus 迁册新加坡的策略是否能真正绕过监管，现在画上了问号
3. **盈利能力标杆**：Manus 是中国 AI 领域少数实现盈利的应用产品 [S14]，其商业模式验证对行业有标杆意义

### 对 Meta 的影响

- **短期**：获得 Agent 产品能力和经验团队，加速 Meta AI 产品矩阵升级
- **中期**：将 Manus 能力集成到 WhatsApp、Instagram、Ray-Ban 等产品中
- **长期**：如果中国审查导致团队分裂或技术受限，收购价值可能打折

### 客户反应

CNBC 报道部分 Manus 客户对收购表示不满 [S25]——主要担忧包括：产品独立性是否会丧失、数据是否会被 Meta 获取、定价是否会变化等。Manus 官网定价页面目前已显示 "© 2026 Meta" 字样 [S29]。

---

## 风险与争议

### 1. "Shell Product"质疑

Manus 在2025年3月首发时曾遭到大量质疑。部分行业人士认为其是"shell product"（空壳产品），将其视为对 API 的简单包装而非真正的技术突破。然而，$1.25亿 ARR 的商业数据和 GAIA benchmark SOTA 的表现在很大程度上回应了这些质疑 [S2][S24]。

### 2. 技术依赖风险

Manus 底层依赖第三方 LLM（尤其是 Anthropic 的 Claude Opus 系列），这意味着：

- 被 Meta 收购后，是否会转向 Meta 自研的 Llama 模型？
- 模型切换是否会导致性能下降？
- 对 Anthropic 的供应链依赖是否构成战略风险？

### 3. 中国审查的不确定性

审查的潜在结果包括 [S6][S17]：

- **最乐观**：认定交易合规，无进一步行动
- **中间场景**：要求补充合规手续、缴纳罚款或限制特定技术的转移
- **最悲观**：要求撤销交易或限制核心人员出境，类似反向 TikTok 场景

### 4. 团队稳定性

大型收购后的人才留存是经典挑战。肖弘虽然已出任 Meta VP，但：

- 核心团队成员是否会在 earnout 期结束后离开？
- Meta 的企业文化是否能留住创业型人才？
- 中国籍员工是否会受到签证或出入境限制？

### 5. 数据与隐私

Manus 处理的147万亿 tokens 中包含大量用户数据。Meta 将如何处理这些数据？用户（尤其是巴西等主要市场的用户）是否会对数据流向 Meta 表示担忧？ [S23][S24]

---

## 结论与展望

Manus AI 的故事是2025年 AI 行业最具戏剧性的创业叙事之一。一个江西90后创业者，从浏览器插件起步，9个月内打造出全球增速最快的 AI Agent 产品，最终以超20亿美元被 Meta 收购——这个故事浓缩了 AI 时代的速度、机遇与地缘政治的复杂性。

**三个关键判断：**

1. **Agent 时代已至**：Manus 的商业成功和 Meta 的天价收购共同验证了一个趋势——AI 的价值正从"生成内容"向"自主执行任务"迁移。对话式 AI 将让位于行动式 AI。

2. **地缘政治将重塑 AI 创业退出路径**：中国商务部的审查发出了明确信号——中国 AI 技术和人才的跨境流动将受到越来越严格的管控。未来中国 AI 创业者的国际化策略需要从一开始就考虑合规架构。

3. **Meta 的 AI 转型成败在此一举**：$6000亿基础设施投入 + Manus 收购 + Meta Compute 组织重构，Zuckerberg 正在对 AI 进行"all-in"式押注。如果 Agent 产品能够成功集成到 Meta 的30亿用户生态中，这笔交易将被证明物超所值；反之，则可能成为又一个昂贵的教训。

**需要持续关注的关键变量：**

- 中国商务部审查的最终结论（预计2026年年中）
- Manus 产品在 Meta 体系内的整合进度
- 竞品（尤其是 OpenAI Agent 和 Google Gemini Agent）的发展速度
- Manus 核心团队的留存情况

---

## 附录 A：证据表

| 证据编号 | 来源 | 关键信息 | 可信度 |
|---|---|---|---|
| S1 | Wikipedia | Butterfly Effect 注册信息、产品版本时间线 | 高 |
| S2 | Grokipedia | 公司创立时间、Benchmark 表现 | 中 |
| S3 | Reuters | 估值 $20-30亿、Benchmark 领投 $7500万 | 高 |
| S4 | Business Times | 总部迁移、ARR 数据 | 高 |
| S5 | Business Insider | Meta 将切断中国联系 | 高 |
| S6 | Bloomberg | 中国深化审查、扩展审查维度 | 高 |
| S7 | CNBC (12/30) | 公司起源与迁移 | 高 |
| S8 | CNBC (1/8) | 中国出口管制调查 | 高 |
| S9 | TechCrunch | 收购报道 | 高 |
| S10 | Techstrong.ai | 估值超 $20亿、谈判约10天 | 中 |
| S11 | VentureBeat | 运营安排、汇报关系、产品能力 | 高 |
| S12 | 36氪 | 创始人背景、9个月创业历程 | 高 |
| S13 | Asian Intelligence | 肖弘传记、公司2021年创立 | 中 |
| S14 | ChinaBizInsider | 谈判细节、创始人履历、商业表现 | 高 |
| S15 | AP News | 中国调查意向 | 高 |
| S16 | MOFCOM | 商务部官方声明 | 最高 |
| S17 | SCMP | 审查可能持续半年 | 高 |
| S18 | Geopolitechs | Acquihire 模式分析 | 中 |
| S19 | National Interest | 中美人才竞争框架 | 中 |
| S20 | Manus 官方 | 官方公告 | 最高 |
| S21 | 搜狐 | 团队身价、Meta AI 焦虑 | 中 |
| S22 | 澎湃 | 第三大收购、邀请码价格 | 高 |
| S23 | AInvest | ARR、定价、用户分布、阿里云合作 | 中 |
| S24 | CIW | ARR 增长速度、技术数据 | 中 |
| S25 | CNBC (1/21) | 客户不满反应 | 高 |
| S26 | Siberia Fund | 红杉中国、腾讯早期投资 | 中 |
| S27 | Medium | 创始人名字辨析 | 低 |
| S28 | en.taibo.cn | 收购排名、估值、Meta AI 战略 | 中 |
| S29 | Manus 官网 | 产品功能、当前状态 | 最高 |
| S30 | Manus 文档 | 技术架构描述 | 最高 |

---

## 附录 B：参考来源

1. [S1] Wikipedia. "Manus (AI agent)." https://en.wikipedia.org/wiki/Manus_(AI_agent)
2. [S2] Grokipedia. Manus AI 条目.
3. [S3] Reuters. "Meta to buy Chinese founded startup Manus to boost advanced AI." 2025-12-30. https://www.reuters.com/world/china/meta-acquire-chinese-startup-manus-boost-advanced-ai-features-2025-12-29/
4. [S4] Business Times. Manus 收购报道.
5. [S5] Business Insider. "What is Manus, the Chinese-founded AI startup Meta is buying for over $2 billion?" 2025-12-31. https://www.businessinsider.com/what-is-manus-ai-meta-acquisition-chinese-startup-singapore-agent-2025-12
6. [S6] Bloomberg. "China Deepens Review of Meta's Landmark $2 Billion Manus Buyout." 2026-01-23. https://www.bloomberg.com/news/articles/2026-01-23/china-deepens-review-of-meta-s-landmark-2-billion-manus-buyout
7. [S7] CNBC. "Meta acquires intelligent agent firm Manus." 2025-12-30. https://www.cnbc.com/2025/12/30/meta-acquires-singapore-ai-agent-firm-manus-china-butterfly-effect-monicai.html
8. [S8] CNBC. "Meta faces China probe over acquisition of AI agent startup Manus." 2026-01-08. https://www.cnbc.com/2026/01/08/china-investigate-meta-acquisition-manus-export.html
9. [S9] TechCrunch. "Meta just bought Manus, an AI startup everyone has been talking about."
10. [S10] Techstrong.ai. Manus 收购报道.
11. [S11] VentureBeat. Manus 运营安排报道.
12. [S12] 36氪. "9个月走完初创公司一生."
13. [S13] Asian Intelligence. 肖弘传记.
14. [S14] ChinaBizInsider. "Inside Meta's Multi-Billion-Dollar Acquisition of Manus." 2025-12-30. https://chinabizinsider.com/inside-metas-multi-billion-dollar-acquisition-of-manus-founder-xiao-hong-on-the-darkest-hour/
15. [S15] AP News. 中国调查报道.
16. [S16] SCIO/MOFCOM. "China's commerce ministry comments on investigation of Meta's acquisition of Manus." 2026-01-09. http://english.scio.gov.cn/pressroom/2026-01/09/content_118270474.html
17. [S17] SCMP. "Manus hits US$100 million revenue milestone." 2025-12-18.
18. [S18] Geopolitechs. "Expert Close to MOFCOM: Meta–Manus Deal May Have Violated China's Technology Export Controls." 2026-01-03. https://www.geopolitechs.org/p/expert-close-to-mofcom-metamanus
19. [S19] National Interest. 中美人才竞争分析.
20. [S20] Manus 官方. "Manus Joins Meta for Next Era of Innovation." https://manus.im
21. [S21] 搜狐. 团队身价与 Meta 收购分析.
22. [S22] 澎湃新闻. Manus 收购报道.
23. [S23] AInvest. "Manus AI's $125M ARR Run Rate." 2025-12-17. https://www.ainvest.com/news/manus-ai-125m-arr-run-rate-harbinger-ai-infrastructure-paradigm-2512/
24. [S24] CIW / Manus Blog. "Manus Update: $100M ARR, $125M revenue run-rate." 2025-12-17. https://manus.im/blog/manus-100m-arr
25. [S25] CNBC. 客户反应报道. 2026-01-21.
26. [S26] Siberia Fund. 红杉中国与腾讯投资信息.
27. [S27] Medium. 创始人名字辨析.
28. [S28] en.taibo.cn. 收购排名与 Meta 战略分析.
29. [S29] Manus 官网. https://manus.im / https://manus.im/pricing
30. [S30] Manus 文档. 技术架构描述.

---

*本报告基于公开信息撰写，截至2026年2月2日。中国商务部审查仍在进行中，最终结论尚未公布。*
