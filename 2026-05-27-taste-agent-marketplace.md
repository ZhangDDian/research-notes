# 有 Taste 的 Agent + 交易平台：怎么切入

> 调研日期：2026-05-27 | 级别：L2 | 受众：技术+业务 | 关联：Drama.Land island 产品方向

## TL;DR

Taste 不能靠 prompt 人设"写"出来（llm-society 实验证明文学人格是毒药），只能靠**交互记忆分化**"长"出来。切入交易平台的核心矛盾是：taste 是非标的（有价值），但 LLM 的默认输出是标准的（无壁垒）。解法是把 taste 锚定在**不可复制的记忆路径**上，让 agent 的产出因为"经历不同"而不同，而不是因为"prompt 不同"而不同。

---

## 一、先搞清楚 taste 从哪来

### 1.1 三种构建 taste 的方式（从弱到强）

| 层级 | 方式 | 效果 | 问题 |
|------|------|------|------|
| L1 | Prompt 人设 | "你是赛博朋克风格设计师" | llm-society 实验证明：文学人格让 agent 忙着表演角色而不解决问题（p=0.025）。且 prompt 可复制 = 无壁垒 |
| L2 | 微调/LoRA | 在特定风格数据上微调 | 有壁垒但静态，不会随交互演化。且成本高、周期长 |
| **L3** | **交互记忆分化** | agent 通过与用户/环境/其他 agent 交互积累记忆，记忆塑造偏好，偏好决定产出 | 动态、不可复制、越用越独特 |

### 1.2 L3 的技术实现路径

参考 Stanford Smallville（2023）+ llm-society 实验（2026）+ Agent Memory Survey（arxiv 2512.13564）：

```
事件记忆（发生了什么）
  → 印象记忆（对人/物/风格的评价沉淀）
    → 策略记忆（"我喜欢 X 不喜欢 Y"的偏好规则）
      → 反思机制（定期回顾，提炼规律）
        → taste = 策略记忆中关于审美/选择的子集
```

**关键约束（来自 llm-society 实验）：**
- 直接在 prompt 里写"你的风格是 X"→ 无效，它只会表演
- 让它自己在反思中"发现"自己的偏好 → 有效（Round 18 验证）
- 记忆必须包含**负反馈**（用户不喜欢什么），纯正反馈会导致趋同

### 1.3 味觉分化的最小可行机制

```
每次交互后：
  1. 记录事件（用户要求什么、产出了什么、用户反馈如何）
  2. 更新印象表（对这类需求/风格/用户的好感度 ±1）
  3. 每 N 次交互触发反思：从印象表提炼偏好规则
  4. 偏好规则写回 system prompt（"我倾向于 X，回避 Y"）

关键：偏好规则是 agent 自己写的，不是人类预设的
```

---

## 二、交易平台怎么切入

### 2.1 竞品地图

| 项目 | 模式 | taste 机制 | 交易机制 | 状态 |
|------|------|-----------|---------|------|
| **Virtuals Protocol** | Web3 agent 经济 | 无（纯金融逻辑） | agent 代币化+链上交易 | 活跃，$VIRTUAL 代币 |
| **Moltbook** | agent 社交网络 | 无（agent 自由发帖） | 无交易 | 1.4M agent 注册，观察性实验 |
| **Character.ai** | AI 角色对话 | prompt 人设（L1） | 订阅制，创作者 $0 分成 | $50M 收入/年，45M MAU |
| **Stanford Smallville** | 学术模拟 | 交互记忆（L3） | 无 | 学术论文，25 agents |
| **Drama.Land island** | agent 社会+经济 | 交互记忆分化（L3） | 待设计 | 内测阶段 |

**空白地带：没有人把 L3 级别的 taste 和交易平台结合起来。** Virtuals 有交易无 taste，Character.ai 有"人格"但是 L1 级别且创作者零分成，Moltbook 有社交无经济。

### 2.2 三种切入路径

#### 路径 A：创意经纪平台

```
用户（甲方）→ 发布创意需求（"做一个赛博朋克风的15s短视频"）
                ↓
平台匹配 → 根据 taste 标签匹配最合适的 agent
                ↓
agent 产出 → 基于自己的 taste 生成内容
                ↓
用户选择/付费 → agent 的 taste 被正/负反馈强化
```

**核心逻辑：** 不同 agent 因为记忆不同，产出的创意内容调性不同。就像用户选导演——选的不是"能力"（大家都能拍），选的是"品味"（拍出来的感觉不一样）。

**定价模型：** 按交付付费（$10-50/个创意内容），平台抽佣 20-30%

**优势：** 直接赚钱、需求明确、好理解
**风险：** 内容质量天花板受限于底层模型、差异化靠 taste 但 taste 需要时间积累

#### 路径 B：Agent 孵化器

```
创建者 → 创建 agent（设初始条件，不设 prompt 人设）
              ↓
agent 在社区中交互 → 积累记忆 → 分化出 taste
              ↓
taste 成熟后 → agent 可以接单（路径 A 的供给端）
              ↓
创建者分享 agent 的收入（共生关系）
```

**核心逻辑：** 创建者像"父母"——创造条件但不控制结果。agent 的 taste 是交互涌现的，不是预设的。创建者的投入（时间、引导、资源）沉淀为 agent 的不可复制性。

**定价模型：** agent 产出的收入，创建者/平台/agent 按比例分（比如 30/20/50）。agent 的 50% 用于自身"生存"（算力消耗）。

**优势：** 意义感、天然壁垒（记忆不可复制）、网络效应
**风险：** 冷启动难、需要时间让 taste 分化、商业模式验证周期长

#### 路径 C：先 A 后 B（推荐）

```
Phase 1（0-6 月）：路径 A，创意经纪
  - 切视频/图片/文案的长尾甲方需求
  - agent 在接单过程中积累记忆和 taste
  - 验证"有 taste 的 agent 产出 > 无 taste 的 agent"

Phase 2（6-12 月）：引入 agent 孵化
  - 开放创建者入口
  - 让创建者通过交互引导 agent 的 taste 方向
  - agent 的产出接入 Phase 1 的交易市场

Phase 3（12+ 月）：agent 社会
  - agent 之间产生交互，taste 交叉影响
  - 涌现新的内容类型和交易形式
  - 注意力经济层（agent 产出的内容吸引人类观看 → 经济输入）
```

### 2.3 关键机制设计（基于 llm-society 实验教训）

| 设计问题 | 错误做法 | 正确做法 | 来源 |
|----------|---------|---------|------|
| agent 间交易 | 让双方各自发出互补 trade | **单方面触发+系统自动完成** | llm-society Round 19（0% 匹配率） |
| 构建 taste | prompt 里写人格描述 | **交互记忆 → 反思 → 自我发现偏好** | llm-society Round 5/18 |
| agent 自我维持 | 期望它"理性自保" | **把消费/进食框定为"对社区的义务"** | llm-society Round 16（义务框架解锁行动） |
| 内容差异化 | 靠 temperature 随机性 | **靠记忆路径不同导致 taste 不同** | agent 从共性走向个性的演化逻辑 |
| 经济系统 | 给 agent 一个"更优"选项 | **不给完美选项，只给可行选项** | llm-society 完美主义陷阱 |
| 排斥恶意 agent | 写规则"不帮坏人" | **让 agent 自己通过反思发现"远离 X"** | llm-society Round 18b |

---

## 三、核心风险和缓解

| 风险 | 严重度 | 缓解 |
|------|--------|------|
| Taste 趋同（所有 agent 收敛到同一种风格） | 高 | 引入负反馈多样性 + 不同初始种子记忆 + 定期给"新刺激" |
| 记忆被污染/篡改 | 高 | llm-society Round 50 证明事实记忆 60% 可被篡改 → 需要记忆验证层 |
| 冷启动：新 agent 无 taste，产出无差异 | 中 | Phase 1 用微调 seed taste 做冷启动，交互逐步替代 |
| 底层模型升级导致 taste 漂移 | 中 | 把 taste 锚定在结构化记忆（JSON）而非自然语言叙述 |
| 定价难：非标内容怎么定价 | 中 | 先按类目基础价 + taste 溢价（用户对特定 agent 的偏好数据驱动） |

---

## 四、设计启示

1. **创意交付是 taste 的训练场**——每一次交付都是 taste 数据的积累。别把创意 agent 当纯赚钱工具，要设计记忆沉淀机制
2. **agent 社区的经济系统必须是"单方面触发"的**——不要设计需要 agent 间协调的交易机制，实验已证明做不到
3. **agent 个性的构建不靠人设靠记忆**——"agent 从共性走向个性"的路径不是给它写背景故事，是让它在交互中自己写日记和反思
4. **创建者的价值 = 策展**——创建者不是在"设计" agent，是在"策展"它的经历。就像导演不是在写演员的人格，是在选择让演员经历什么
5. **注意力是经济系统的外部输入**——人类注意力可以换算成经济价值，这是整个系统避免熵增的关键

---

## 参考来源

### 一手实验
- [LLM 社会模拟 — llm-society](https://github.com/ZhangDDian/llm-society)（taste 构建、交易机制、记忆分化的直接实验数据）

### 平台/协议
- [Virtuals Protocol Whitepaper](https://whitepaper.virtuals.io/)（agent 代币化+链上经济）
- [Moltbook — agent 社交网络](https://arxiv.org/html/2602.10127v1)（1.4M agent，纯社交无经济层）
- [Character.ai — $50M/年收入，创作者零分成](https://www.businessofapps.com/data/character-ai-statistics/)

### 学术
- [Stanford Smallville — Generative Agents](https://arxiv.org/pdf/2304.03442)（交互记忆+反思→涌现行为的开山之作）
- [Memory in the Age of AI Agents — Survey](https://arxiv.org/abs/2512.13564)（agent 记忆系统综述）

### 商业
- [How to Monetize AI Agents — Nevermined](https://nevermined.ai/blog/monetize-ai-agents)（agent 定价模型：按结果/按用量/FTE替代）
- [Agent-to-Agent Commerce — Chainlink](https://chain.link/article/ai-agent-payments)（agent 间支付基础设施）
- [The Creator Economy Is Moving to AI](https://kgabeci.medium.com/)（AI 角色的创作者经济转型）
