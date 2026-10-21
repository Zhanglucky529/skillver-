# Skillver 学院产品需求文档

- **文档编号**：PRD-SKILLVER-ACADEMY
- **文档版本**：0.3
- **日期**：2026-08-25
- **状态**：**Final Draft · 待评审 · 未生效**
- **目标产品版本**：Skillver Academy 1.0
- **现网事实源**：[`v1/CP_PRODUCT_MASTER.md`](./v1/CP_PRODUCT_MASTER.md) + 代码
- **已批准基线**：[`PRD-skillver-job-copilot-final-2026-08.md`](./PRD-skillver-job-copilot-final-2026-08.md)（Approved v1.3，关系见 §1.6）
- **非选择方案**：[`PRD-skillver-v2-ai-job-execution-2026-08.md`](./PRD-skillver-v2-ai-job-execution-2026-08.md)
- **适用规则**：RULE-PROTO-001/002/003/004/005、RULE-AI-001/002/003/004/005/006、RULE-Q-001/004/005、RULE-ARCH-001/002/003/005/006、RULE-FE-BE-001/003/004、RULE-SAFE-005/006/008、RULE-PROCESS-001/003/005

> 本文汇总 2026-08-24～25 关于 Skillver 学院的产品讨论，按最新结论统一产品定位、学生旅程、Agent 职责、教学内容、项目交付、数字人交互、Agent 成长、数据体系、代码复用与实施边界。本文不修改受保护的 7D 定义与公式、214 个 KAE skill、10 个底层 Agent、商业比例和安全基线；评审生效前不得替代 V1 现网事实源或据此宣称已实现。

**0.3 修订摘要**（按 2026-08-25 评审意见补齐三项阻断问题）：

- 新增 §1.6，明确与已批准的人才求职助手基线的关系，并要求单独签署产品优先级决策（RULE-ARCH-001）；
- 新增 §11.4／§11.5 MVP 核心 Schema 字段锁定与版本规则，新增 §9.2 评测用例结构和样例（RULE-PROTO-002／003）；
- 为项目、毕业和 Agent 版本三个状态机补齐逐边驱动方、触发条件、前置证据和幂等键（RULE-PROCESS-005）；
- §14.5 补用量到成本的推导、模型分档和降档规则；新增 CAC 上限与含获客的贡献毛利门；
- §15.7 补回测参与率下限、一致性抽检细则和"无证据通过率"的机判定义；
- 消除 §14.4／§17 与 §18 之间的阶段口径冲突。

**0.2 修订摘要**：

- 将原“大而全 MVP”收缩为一个 6 周、单一联合项目、双角色分工的付费验证班；
- 前台收敛为“学习、练习、实训”三个主要空间，Agent 角色按场景出现，不作为导航；
- 新增 B2C 首付费、学校采购和企业赞助的分阶段商业假设与单位经济门；
- 新增 MVP Go/No-Go 目标值；所有金额和阈值均为待实测假设，不修改受保护商业规格。

---

## 0. 执行摘要

Skillver 学院是面向跨专业大学生的纯 Agent 教育产品，帮助学生完成：

```text
学习 AI
→ 使用 AI
→ 开发 AI
→ 交付真实项目结果
→ 通过独立毕业答辩
→ 进入 AI 产品经理或 Agent 工程师岗位
```

完整目标态优先建设两条专业路线：

1. AI 产品经理；
2. Agent 工程师。

FDE、数据工程师作为后续专业。MVP 不交付上述两个完整专业，只在一个联合项目内验证 AI 产品和 Agent 工程两个角色分工，边界见 §18。

产品不是“七个 Agent 聊天窗口”，而是一个围绕学生任务、作品、项目、能力证据和毕业结果运行的 Agent 学院。语音和数字人用于提升亲近感、互动和角色扮演；文字、代码、白板、引用和项目工作台用于承载精确内容和交付结果。

学院对学生的最终承诺不是“学完课程”，而是：

> 在 AI 协作成为默认生产方式的环境中，学生能够把模糊需求转化为正确、可靠、可维护、可验收的 AI 项目结果，并用证据证明自己真正掌握。

---

## 1. 产品定位

### 1.1 目标用户

- 非计算机专业但希望进入 AI 行业的大学生；
- 有基础编程能力但缺少 AI 项目经验的学生；
- 能使用大模型但不能独立完成产品或工程交付的学生；
- 需要完成 AI 产品、Agent 系统或相关作品集的学生；
- 希望成为 AI 产品经理、Agent 工程师，后续扩展至 FDE、数据工程师等岗位的学生。

### 1.2 核心需求

目标学生通常缺少：

- 对 AI 岗位的准确理解；
- 可执行的个性化学习路径；
- 随时可用、能读懂其作品的教师；
- 真实需求和项目交付训练；
- 对 AI 生成结果进行验证、修复和负责的能力；
- 可被岗位、企业和项目验证的作品与能力证据；
- 持续更新的行业内容和实践经验。

### 1.3 产品结果

Skillver 学院只对四类结果负责：

1. **能力形成**：学生在目标 skill/NL 上达到可验证掌握；
2. **项目交付**：学生能在 AI 辅助下完成可运行、可测试、可部署、可验收项目；
3. **毕业可信**：独立主考官能根据冻结证据和答辩确认学生掌握；
4. **就业衔接**：毕业能力、作品和证据可进入现有匹配、投递和面试训练。

### 1.4 产品原则

1. 结果优先于课时；
2. 项目优先于内容消费；
3. 证据优先于自我声明；
4. 引导优先于直接给答案；
5. AI 原生优先于禁止使用 AI；
6. 结构化状态优先于聊天记忆；
7. 受控持续成长优先于在线自行修改；
8. 权威和授权内容优先于全网无差别抓取；
9. 语音优先建立互动，文字和作品始终保留；
10. Agent 提议，确定性 backend 决定状态。

### 1.5 明确不做

- 不把观看视频、阅读文章或完成聊天轮数等同掌握；
- 不禁止学生使用 Coding Agent；
- 不允许学生只提交无法解释和维护的 AI 生成代码；
- 不建设七个彼此割裂的聊天机器人；
- 不让 Agent 在线自行修改毕业标准、rubric、课程或生产配置；
- 不无差别抓取整个互联网；
- 不绕过付费墙、登录、验证码、robots、ToS 或反自动化措施；
- 不复活现有导师端、企业 IM、participation 或零工项目 marketplace 语义；
- 不直接修改 7D、KAE taxonomy 和受保护底层 Agent 定义；
- 不承诺“保证取代最聪明的人类教授”。

### 1.6 与已批准基线的关系

[`PRD-skillver-job-copilot-final-2026-08.md`](./PRD-skillver-job-copilot-final-2026-08.md)（Approved v1.3）是当前已批准的产品目标基线，其技术计划 v1.1 待签署。本文提出学院方向，两者按以下口径共存：

1. **现网事实源不变**：V1 仍是唯一现网事实源，两份文档都未生效；
2. **求职助手不废止**：学院 §4.7 的就业出口直接调用求职助手已定义的匹配、投递和面试能力，学院不新建 Resume／Application／Interview 的第二套 SSOT；
3. **领域所有权唯一**：简历、投递、面试三个领域的模型和状态机由求职助手基线拥有，学院只作为上游提供作品与能力证据；
4. **优先级未决**：学院 MVP 与求职助手技术计划 v1.1 的实施顺序、资源分配与发布次序，本文不预设；
5. **决策前置**：必须先产出并签署 `DECISION-academy-vs-job-copilot-priority-2026-08.md`，明确主线、次线和资源边界；该决策未签署前，学院不得进入技术计划或占用求职助手已排期的研发资源。

若决策结论为学院成为主线，则求职助手基线降级为学院的就业出口模块，需同步修订其 PRD 状态和 `DOC_INDEX.md`，不得留下两份并行声称为"产品目标基线"的文档。

---

## 2. 产品架构

### 2.1 总体结构

```mermaid
flowchart TD
    Student["学生"]
    UI["多模态学院界面"]
    StateMachine["Backend 学院状态机"]

    Dean["Agent 院长"]
    ViceDean["Agent 教学副院长"]
    FoundationProfessor["AI 基础教授"]
    ProductProfessor["AI 产品教授"]
    EngineeringProfessor["Agent 工程教授"]
    DeliveryProfessor["项目交付教授"]
    Examiner["毕业主考官"]

    ContentAgent["后台教研内容 Agent"]
    LearningLab["Agent Learning Lab"]

    KAE["KAE / NL / K-JURY"]
    Interview["Interview QA"]
    Artifact["Artifact / OSS"]
    Training["Training Plans"]
    Sandbox["代码沙箱 / 测试 / Trace"]
    JobOutcome["匹配 / 投递 / 面试"]

    Student <--> UI
    UI --> StateMachine
    StateMachine --> Dean
    StateMachine --> ViceDean
    StateMachine --> FoundationProfessor
    StateMachine --> ProductProfessor
    StateMachine --> EngineeringProfessor
    StateMachine --> DeliveryProfessor
    StateMachine --> Examiner

    ContentAgent --> Dean
    ContentAgent --> FoundationProfessor
    ContentAgent --> ProductProfessor
    ContentAgent --> EngineeringProfessor
    LearningLab --> Dean

    Dean --> KAE
    ViceDean --> Training
    FoundationProfessor --> KAE
    ProductProfessor --> Interview
    EngineeringProfessor --> Sandbox
    DeliveryProfessor --> Artifact
    Examiner --> Interview
    Examiner --> KAE
    Examiner --> Artifact
    StateMachine --> JobOutcome
```

### 2.2 三类新增

现有七类产品 Agent 之外，只新增三项，不继续扩张角色：

1. **后台教研内容 Agent**：内容发现、审核、结构化和更新；
2. **Agent Learning Lab**：Agent 经验沉淀、评测、影子、灰度和回滚；
3. **多模态课堂层**：语音、数字人、字幕、文字、白板、代码和作品界面。

教研内容 Agent 不直接面对学生；Agent Learning Lab 不是对话 Agent；多模态课堂不拥有业务状态。

### 2.3 统一运行协议

所有产品 Agent 统一执行：

```text
Observe
→ Diagnose
→ Plan
→ Act
→ Verify
→ Reflect / Handoff
```

- `Observe`：读取结构化事实和相关记忆；
- `Diagnose`：判断能力、内容、任务、时间或工具问题；
- `Plan`：生成受 Schema 约束的下一步；
- `Act`：调用最小权限工具；
- `Verify`：检查输出和证据；
- `Reflect/Handoff`：沉淀事件或结构化移交。

自然语言聊天不能成为权威状态。学生、课程、任务、提交、项目、评测、毕业和 Agent 版本由 backend 保存。

---

## 3. Agent 职责

## 3.1 Agent 院长

### 职责

- 维护两专业毕业能力标准；
- 维护课程依赖图；
- 定义教授责任和工具权限；
- 监控学院级教学质量；
- 处理跨教授冲突；
- 审查重大改课提案；
- 将毕业薄弱项回流课程改进。

### 输入

- 岗位、NL 和 skill 版本；
- 课程和任务版本；
- 教授评测结果；
- 群体通过率、补修率和保持率；
- 项目失败模式；
- 毕业答辩结果；
- Agent 成本、延迟和降级指标。

### 输出

- `ProgramConstitution`；
- `CurriculumGraph`；
- 教授授权表；
- 课程质量报告；
- 待人工签署的变更提案。

### 边界

- 不负责日常授课；
- 不直接给学生毕业分；
- 不直接修改 7D、taxonomy、生产配置或受保护算法；
- 重大课程和毕业标准变更必须人工签署。

### 能力证明

- 能发现课程目标无 assessment；
- 能发现 skill_id 非法；
- 能发现教授职责重叠；
- 能识别群体毕业失败集中点；
- 同输入可重放；
- 越权提案必须被拒绝。

## 3.2 Agent 教学副院长

### 职责

- 入学诊断；
- 专业方向推荐；
- 个性化学习计划；
- 每周排期；
- 学情监控；
- 停滞、过难、过易和时间风险干预；
- 将具体教学问题移交教授。

### 输入

- 专业、年级和背景；
- 时间预算；
- 目标岗位；
- 简历和作品；
- 基线测评；
- 历史作答和项目表现；
- 学习偏好和授权。

### 输出

- `LearnerProfile`；
- `CompetencyProfile`；
- 方向建议；
- `LearningPlan`；
- `InterventionCase`；
- 教授 `Handoff`。

### 核心规则

- 只有证据支持的既有能力才能计入画像；
- 只能选择院长批准的真实课程 ID；
- 不能自行修改课程标准；
- 不能宣布毕业。

### 能力证明

- 区分不会、无证据、难度过高和时间不足；
- 对不同原因采取不同干预；
- 方向建议准确率可评测；
- 干预后恢复率可测。

## 3.3 AI 基础教授

### 职责

教授两专业共同基础：

- 大模型基本原理；
- Prompt；
- RAG；
- 工具调用；
- Agent 基础；
- 数据与评测；
- AI 安全和能力边界。

### 教学方式

1. 诊断问题定位误区；
2. 短讲解；
3. 苏格拉底式追问；
4. 分级提示；
5. 小任务；
6. 反馈；
7. 迁移题；
8. 达标后解锁。

### 能力证明

- 核心解释有引用；
- 不直接泄露答案；
- 能识别典型误区；
- 学生在新题上的迁移正确率提高；
- 连续失败能触发补救而不是重复同一解释。

## 3.4 AI 产品教授

### 职责

训练学生将真实问题转化为可执行 AI 产品：

- 用户研究；
- Problem Statement；
- 成功指标；
- 模型能力边界；
- 数据、Prompt 和 RAG 方案；
- failure taxonomy；
- PRD；
- 原型；
- eval set；
- 成本、风险和上线指标。

### 教学方式

- 角色扮演访谈；
- 需求冲突澄清；
- PRD 结构审查；
- 原型迭代；
- 评测集设计；
- 真实案例复盘；
- 项目交付。

### 学生输出

- 访谈记录；
- PRD；
- 用户流程；
- 原型；
- eval set；
- 上线和监控方案；
- 复盘报告。

### 能力证明

- 需求可追踪；
- 指标可计算；
- eval 覆盖主要失败模式；
- 原型可演示；
- 学生能解释产品与模型的边界。

## 3.5 Agent 工程教授

### 职责

教授：

- Python 和 API；
- 模型调用；
- RAG；
- 工具使用；
- 状态和记忆；
- 工作流；
- 多 Agent；
- 评测；
- 可观测性；
- 安全；
- 部署。

### 教学方式

1. 从可运行小型 Agent 开始；
2. 每个任务先锁接口和测试；
3. 逐步增加检索、工具、状态和多 Agent；
4. 读取测试和 trace 定位问题；
5. 只给分级提示；
6. 根据真实失败生成针对性 kata；
7. 要求部署和评测。

### 学生输出

- 架构设计；
- 代码仓；
- 自动化测试；
- 运行 trace；
- 部署版本；
- Agent eval；
- 用户和运维文档。

### 能力证明

- 测试通过率；
- 工具调用正确率；
- Agent 任务完成率；
- groundedness；
- 延迟和成本；
- 故障恢复；
- 学生独立修改和修复能力。

## 3.6 项目交付教授

### 职责

- 将项目 Brief 编译为验收标准和里程碑；
- 主持 kickoff、周检和变更评审；
- 管理范围、风险、依赖和返工；
- 制造受控需求变化；
- 组织验收演示和复盘。

### 学生输出

- 项目章程；
- 里程碑；
- 风险清单；
- 变更记录；
- 演示包；
- 测试和验收报告；
- 用户文档；
- 交付复盘。

### 边界

- 不复用现有零工 marketplace 语义；
- 项目状态由 backend 状态机控制；
- 未达到冻结 acceptance criteria 不得完成。

### 能力证明

- 检出范围漂移；
- 检出无测试假完成；
- 检出需求与实现断链；
- 按期率、验收率和返工率可测。

## 3.7 毕业主考官

### 职责

- 独立审查毕业证据；
- 生成答辩蓝图；
- 组织动态追问；
- 调用 K-JURY；
- 输出通过、补修或诚信拒绝。

### 输入

- 冻结毕业标准；
- 必修记录；
- 项目证据；
- skill/NL 评测；
- PRD、代码、测试、trace 和部署；
- 学术诚信信号。

### 流程

1. 校验证据完整性和来源；
2. 找出未充分证明的 skill/NL；
3. 使用 Interview QA 生成可追溯追问；
4. 调用 K-JURY，评分携带合法 `skill_id`；
5. 跨证据一致性检查；
6. backend 按确定性规则输出：

```text
PASS
| REMEDIATION_REQUIRED
| REJECTED_INTEGRITY
```

7. 补修项返回副院长。

### 能力证明

- 无证据不能通过；
- 同证据重复评审保持稳定；
- 授课教授意见不能改变冻结 rubric；
- 题目、skill 和证据来源可追溯；
- 无证据通过率必须为零。

## 3.8 后台教研内容 Agent

### 职责

- 发现候选内容；
- 检查来源等级和许可；
- 保存原文快照；
- 提取 Claim 和引用；
- 映射 skill/NL 和课程目标；
- 生成候选 Learning Object；
- 运行事实、教学、版权、运行和泄题评测；
- 监控来源变化和内容过期。

### 边界

- 不直接发布核心课程；
- 不自行修改毕业标准；
- 不将社区讨论直接当作事实；
- 不访问禁止来源；
- 高风险内容进入教研 Owner 审批。

---

## 4. 学生完整旅程

## 4.1 入学

学生提供：

- 专业和年级；
- 每周可投入时间；
- 目标专业；
- 简历、课程、比赛、作品和代码仓；
- 基线测评。

副院长输出正式画像、方向建议和先修缺口。

## 4.2 个性化计划

系统综合：

- 目标岗位能力；
- 学生能力证据；
- 课程先修；
- 每周时间；
- 项目目标；
- 历史错误和有效教学方式。

生成版本化 `LearningPlan`。

## 4.3 一次典型课

```text
数字人开课
→ 诊断问题
→ 分段讲解
→ 学生打断追问
→ 文档/代码/白板操作
→ 教授分级提示
→ 自动测试或 rubric 反馈
→ 迁移题
→ 掌握或补救
→ 生成课堂笔记
```

## 4.4 动态调课

触发信号：

- 连续失败；
- 长期停滞；
- 提示依赖过高；
- 已经掌握；
- 项目返工；
- 时间预算变化。

动作：

- 补先修；
- 拆分任务；
- 降低复杂度；
- 更换解释或案例；
- 跳过已掌握内容；
- 改期；
- 结构化移交其他教授。

## 4.5 项目交付

学生必须完成可运行、可测试、可部署、可验收项目。

项目状态机：

```text
BRIEF
→ REQUIREMENT_CLARIFIED
→ PLAN_APPROVED
→ IN_DEVELOPMENT
→ MILESTONE_REVIEW
→ ACCEPTANCE_TESTING
→ DELIVERED
→ DEFENSE_READY
```

异常分支：

```text
IN_DEVELOPMENT → BLOCKED
MILESTONE_REVIEW → REWORK_REQUIRED
ACCEPTANCE_TESTING → REJECTED
```

### 项目状态机逐边定义（RULE-PROCESS-005）

| 迁移边 | 驱动方 | 触发条件 | 前置证据 | 幂等键 |
|--------|--------|----------|----------|--------|
| `BRIEF → REQUIREMENT_CLARIFIED` | 学生提交，交付教授核验 | 澄清清单全部关闭 | `ClarificationRecord` | `project_id + brief_version` |
| `REQUIREMENT_CLARIFIED → PLAN_APPROVED` | 交付教授提议，backend 规则确认 | 里程碑与验收标准冻结 | `AcceptanceCriteriaSnapshot(frozen)` | `project_id + criteria_hash` |
| `PLAN_APPROVED → IN_DEVELOPMENT` | 学生 | 首次有效提交 | `Submission` | `project_id + milestone_id` |
| `IN_DEVELOPMENT → MILESTONE_REVIEW` | 学生 | 里程碑提交且自动测试已运行 | `Submission + TestReport` | `project_id + milestone_id + submission_id` |
| `MILESTONE_REVIEW → IN_DEVELOPMENT` | 交付教授提议，backend 提交 | 里程碑达标 | `MilestoneReview(PASS)` | `milestone_id + review_id` |
| `MILESTONE_REVIEW → REWORK_REQUIRED` | 交付教授提议，backend 提交 | 范围漂移、无测试假完成或需求实现断链 | `MilestoneReview(FAIL) + 缺陷清单` | `milestone_id + review_id` |
| `REWORK_REQUIRED → IN_DEVELOPMENT` | 学生 | 返工提交且未超返工配额 | `Submission(rework) + ReworkQuota` | `milestone_id + rework_seq` |
| `IN_DEVELOPMENT → BLOCKED` | 学生或交付教授 | 外部依赖、环境或授权阻塞 | `BlockerRecord` | `project_id + blocker_id` |
| `BLOCKED → IN_DEVELOPMENT` | backend 规则或人工运营 | 阻塞原因已关闭 | `BlockerRecord(resolved)` | `blocker_id` |
| `IN_DEVELOPMENT → ACCEPTANCE_TESTING` | 学生声明完成 | 全部里程碑 PASS | `MilestoneReview` 全通过 | `project_id + delivery_seq` |
| `ACCEPTANCE_TESTING → DELIVERED` | backend 确定性规则 | 冻结 acceptance criteria 全部通过 | `AcceptanceRecord(all_pass) + DeploymentRecord` | `project_id + criteria_hash` |
| `ACCEPTANCE_TESTING → REJECTED` | backend 确定性规则 | 任一冻结标准未通过 | `AcceptanceRecord(fail)` | `project_id + criteria_hash` |
| `REJECTED → IN_DEVELOPMENT` | 学生 | 剩余返工配额大于零 | `ReworkQuota` | `project_id + rework_seq` |
| `DELIVERED → DEFENSE_READY` | backend | 证据包冻结完成 | `GraduationDossier(frozen)` | `learner_id + dossier_hash` |

不变量：

- Agent 只能发出 `propose`，状态提交只能由 backend 执行；Agent 直接写状态的调用必须被拒绝并记 `AgentDecisionLog`；
- `AcceptanceCriteriaSnapshot` 一旦冻结，任何教授 Agent 都不能修改；变更需走人工签署的变更评审并生成新 `criteria_hash`；
- 同一幂等键重复提交只产生一次状态迁移。

## 4.6 毕业

毕业标准由以下组合构成：

- 必修模块完成；
- Capstone acceptance criteria 全部通过；
- 关键 skill/NL 证据覆盖；
- 项目部署和演示；
- 独立毕业答辩；
- 学术诚信通过。

不能把单次 7D 分数直接等同毕业。7D 仅复用为受保护的能力视图。

### 毕业状态机逐边定义（RULE-PROCESS-005）

| 迁移边 | 驱动方 | 触发条件 | 前置证据 | 幂等键 |
|--------|--------|----------|----------|--------|
| `DEFENSE_READY → PASS` | backend 确定性规则 | 必修完成、冻结验收全通过、关键 skill/NL 证据齐备、答辩达标且无诚信红线 | `GraduationDossier(frozen) + K-JURY 评分 + DefenseTranscript` | `learner_id + dossier_hash` |
| `DEFENSE_READY → REMEDIATION_REQUIRED` | backend 确定性规则 | 存在未充分证明的 skill/NL 或答辩未达标 | 缺口清单 + `DefenseTranscript` | `learner_id + dossier_hash` |
| `DEFENSE_READY → REJECTED_INTEGRITY` | 主考官提议，**人工终审签署** | 命中 §5.5 诚信红线 | `IntegrityCase + 人工签署记录` | `learner_id + integrity_case_id` |
| `REMEDIATION_REQUIRED → DEFENSE_READY` | 副院长排期，backend 提交 | 限量补修完成并复测通过 | `RemediationRecord(passed)` | `learner_id + remediation_seq` |

不变量：

- 授课教授 Agent 不得写入 `GraduationCase`，主考官 runtime 不得加载授课会话上下文；
- `PASS` 必须绑定非空 `evidence_ids`，且通过证据完整性校验器，否则 backend 直接拒绝；
- `REJECTED_INTEGRITY` 不允许由 Agent 单独决定，必须人工签署。

## 4.7 就业

毕业后进入现有求职结果模块：

- 目标岗位匹配；
- 简历和作品优化；
- 投递；
- 基于真实 JD 和作品的面试训练。

---

## 5. AI 生成代码政策

## 5.1 基本结论

学生可以提交 AI 生成的代码。禁止 AI 生成代码不符合真实行业。

学院考核的不是“是否亲手敲每一行”，而是：

> 学生是否能够指挥 AI，将模糊需求持续转化为正确、可靠、可维护、可验收的项目，并对最终结果负责。

## 5.2 三种学习模式

### 基础理解模式

- 限制完整代码生成；
- 要求阅读、解释、补全和调试；
- 验证基础概念和代码理解。

### AI 协作模式

- 允许 AI 编码；
- 学生负责拆解任务、审查、测试和修复；
- 保留关键 AI 使用记录。

### 项目交付模式

- 允许完整 Coding Agent；
- 学生对架构、质量、安全、成本和验收负责；
- 必须通过独立答辩。

## 5.3 项目证据包

毕业项目不能只提交代码仓，至少包含：

- 需求和架构决策；
- AI 使用声明和关键指令；
- 代码和 commit 历史；
- 自动化测试；
- 运行 trace；
- 安全、成本和性能分析；
- Bug 修复过程；
- 部署和验收结果；
- 学生答辩。

## 5.4 独立掌握验证

主考官现场要求学生：

- 解释 AI 生成代码；
- 修改陌生需求；
- 定位并修复故障；
- 补充测试；
- 说明架构取舍；
- 应对成本、安全和性能约束。

学生不能解释、修改、测试和维护时，即使项目可以运行也不能毕业。

## 5.5 学术诚信红线

使用 AI 不构成不诚信。`REJECTED_INTEGRITY` 只在以下情形触发：

1. 由他人代替本人完成答辩或代为作答；
2. 伪造测试报告、运行 trace、部署记录或验收结果；
3. 提交他人作品并声明为本人成果；
4. 绕过评测环境、篡改评分数据或题库；
5. 隐瞒 §5.2 要求保留的关键 AI 使用记录且经询问后仍不能说明。

处理规则：

- 触发前必须固化证据并生成 `IntegrityCase`；
- 主考官只能提议，人工终审签署后才生效；
- 学生有一次书面申诉机会，申诉由未参与本次评审的人工复核；
- 结论、证据和申诉过程全部落审计日志；
- 与"能力不足"严格区分：能力不足走 `REMEDIATION_REQUIRED`，不进入诚信通道。

---

## 6. 教研内容供应链

## 6.1 大模型已能提供的内容加工

大模型可直接承担：

- 通用概念讲解；
- 摘要和结构化；
- 示例、类比和反例；
- 翻译和难度改写；
- 练习和变式草案；
- 苏格拉底式追问；
- 代码示例和调试建议；
- PRD、项目计划和测试草案；
- 语音稿、图文卡和复习笔记。

大模型是教研加工器，不是权威事实源。它不能保证：

- 知识最新；
- 引用真实；
- 闭源内容有权使用；
- 项目经验真实；
- 生成答案无幻觉。

## 6.2 必须补充的内容

- 最新官方事实；
- 真实项目与事故复盘；
- 客户需求变化；
- 成本、延迟、质量和安全案例；
- 跨专业学习和成长经验；
- 金标题目和 rubric；
- 优秀与不合格作品；
- 答辩追问策略。

## 6.3 内容来源分级

### A 级：权威事实源

- 官方文档；
- 标准；
- 论文；
- 官方仓库；
- 官方 changelog；
- 监管文件。

### B 级：优质教学源

- 高校公开课；
- 官方教程；
- 技术会议；
- 专家课程；
- 出版社和企业授权内容。

### C 级：行业实践源

- 工程博客；
- 开源 issue；
- 事故复盘；
- 产品案例；
- 公开 JD。

### D 级：趋势信号源

- 社区讨论；
- 问答；
- 新闻；
- 榜单；
- 社交平台。

D 级只发现选题，关键内容必须回到 A/B 级验证。

### 禁止源

- 盗版；
- 付费墙绕过；
- 私域泄露；
- 无授权全文；
- 个人敏感信息；
- 需要绕过技术限制的来源。

## 6.4 私域教研知识网络

建立 `Skillver Private Knowledge Network`，纳入合法授权的闭源内容：

- 付费课程；
- 专家私人课件；
- 私域社区；
- 企业内部培训；
- 项目复盘和事故报告；
- 专家访谈；
- 从业者工作笔记；
- 校友学习日志；
- 企业脱敏项目案例；
- 招聘、面试和入职成长经验。

### 获取方式

1. 与创作者签约；
2. 私域管理员授权连接器；
3. 用户个人上传，仅供本人使用；
4. 扩展到学院前取得再授权；
5. 企业内容脱敏并明确范围；
6. 不绕过登录、付费墙和技术限制。

### 权限分级

```text
PERSONAL_ONLY
COHORT_LICENSED
ACADEMY_LICENSED
SYNTHESIS_ONLY
LINK_ONLY
PROHIBITED
```

每条内容记录：

- 所有权人；
- 授权范围；
- 有效期；
- 引用权限；
- 改编权限；
- 模型训练权限；
- 衍生课程权限；
- 撤回和删除方式。

## 6.5 内容流水线

```text
Discover
→ Compliance
→ Snapshot
→ Normalize
→ Claim
→ Map
→ Compose
→ Evaluate
→ Approve
→ Monitor
```

核心实体：

- `ContentSource`；
- `SourceLicense`；
- `SourceSnapshot`；
- `Claim`；
- `Citation`；
- `LearningObject`；
- `LessonVersion`；
- `ContentCompetencyMap`；
- `ContentQualityReport`。

## 6.6 成长经验结构

学习和成长经验不能只保存成故事，需结构化为：

```text
背景
→ 初始能力
→ 问题
→ 行动
→ 有效或无效原因
→ 结果
→ 关键转折
→ 可迁移原则
→ 不适用边界
→ 证据
```

单个经验不能直接推广为普遍规律；需要多个案例或专家评审。

## 6.7 学生看到的教学内容

- 概念卡；
- 引导式课节；
- 真实案例拆解；
- 代码实验；
- 任务骨架；
- 项目 Brief；
- 误区和补救包；
- 题目和答辩蓝图；
- 每周行业更新；
- 专家经验课；
- 校友成长路径。

每项内容展示：

- 来源类型；
- 更新时间；
- 适用范围；
- 引用或授权标识；
- skill/NL；
- 是否来自真实项目；
- 可信等级。

---

## 7. 多模态与数字人课堂

## 7.1 推荐形态

采用：

> Speak-first，Artifact-always

- 数字人语音负责亲近感、讲解、追问、角色扮演和鼓励；
- 文字、图示、代码、引用、任务卡和作品负责精确内容；
- 所有重要语音自动形成字幕和笔记；
- 学生可随时切换纯文字。

## 7.2 适用场景

### 语音和数字人

- 开课；
- 概念讲解；
- 即时追问；
- 角色扮演；
- 客户访谈；
- 项目会议；
- 学习召回；
- 毕业答辩。

### 文字和工作台

- 代码；
- PRD；
- 表格；
- 公式；
- 技术参数；
- 权威引用；
- 测试；
- 项目验收；
- 复习和检索。

## 7.3 课堂界面

建议三栏：

- 左：路线、模块、任务、目标和先修；
- 中：学生文档、原型、代码、终端、测试和演示；
- 右：教授数字人、对话、提示、反馈、引用和工具动作。

作品区是主界面，Agent 对话是辅助界面。

## 7.4 实施顺序

### MVP

- 文字教学主链；
- 可选语音输入和 TTS；
- 字幕；
- 统一角色头像；
- 语音/文字切换；
- 语音失败显式降级为文字。

### 第二阶段

- 低延迟实时语音；
- 自动课堂笔记；
- 轻动画；
- 口型同步数字人；
- 角色动作和情绪；
- 客户模拟；
- 项目会议；
- 答辩。

### 不首发

不首发全程高拟真人视频，避免：

- 高成本；
- 高延迟；
- 弱网失败；
- 恐怖谷；
- 开发复杂度掩盖教学价值。

## 7.5 降级和无障碍

- 静音；
- 字幕；
- 倍速；
- 纯文字；
- 转写纠错；
- 语音失败降级；
- 键盘完整操作；
- 屏幕阅读支持。

---

## 8. 前端信息架构

### 8.1 前台只保留三个主要空间

```text
学习 | 练习 | 实训
```

Agent 角色是后台职责和场景化人设，不等于导航、页面或独立聊天入口。学生不需要理解院长、副院长、内容 Agent 和多个教授之间的系统分工。

### 8.2 学习

承担“知道为什么、理解概念、形成方法”：

- 今日学习目标；
- 个性化学习顺序；
- 语音/文字讲解；
- 教材、案例和引用；
- 概念图与课堂笔记；
- 当前掌握和待补先修。

默认显示统一的“学院学习助手”。基础教授、产品教授或工程教授只在当前课节中以授课身份出现，不形成独立会话列表。

### 8.3 练习

承担“能否独立应用”：

- 诊断题；
- 变式题和迁移题；
- PRD/评测设计练习；
- 代码 kata；
- 分级提示；
- 自动测试和 rubric 反馈；
- 错题与补救包。

学生只看到任务、作品和反馈。K-JURY、题目生成和教授协作属于后台。

### 8.4 实训

承担“能否交付结果”：

- 项目 Brief；
- 角色和责任；
- 验收标准；
- 里程碑；
- PRD、原型和代码；
- 测试、trace 和部署；
- 风险与变更；
- 演示、验收和最终答辩。

项目交付教授以“项目负责人/客户”身份在实训中出现；毕业主考官只在最终答辩阶段出现。

### 8.5 辅助视图

以下不是一级学习空间，收进头像菜单、进度抽屉或实训流程：

- 我的路线和周计划；
- 能力与作品；
- 通知和设置；
- 答辩和补修；
- 就业出口。

### 8.6 角色可见性

- **学生常驻看到**：一个统一学院助手；
- **课节内看到**：当前专业教授；
- **项目内看到**：客户/项目负责人角色；
- **答辩时看到**：毕业主考官；
- **默认不展示**：院长、副院长、教研内容 Agent、Agent Learning Lab、Compliance、K-JURY。

### 8.7 端侧分工

- Web/IDE：复杂项目、PRD、代码、测试和部署；
- 移动端：学习、短练习、语音互动、提醒、复习和答辩；
- MVP 只做 Web 主链和移动适配页面，不同步交付完整 Flutter 学院端。

---

## 9. Agent 能力保障

## 9.1 五层保障

每个 Agent 的上线资格来自五层保障：

1. 明确职责；
2. 专属数据；
3. 最小工具权限；
4. 确定性流程；
5. 独立评测。

每个 Agent 必须有：

- 正例；
- 边界例；
- 反例；
- 对抗例；
- 降级例。

未达到门禁的 Agent 只能处于建议模式，不能改变学生状态。

## 9.2 评测用例结构与样例（RULE-PROTO-003）

评测用例先于实现。用例统一结构：

```yaml
EvalCase:
  case_id: string
  agent_id: string
  case_type: enum[POSITIVE, BOUNDARY, NEGATIVE, ADVERSARIAL, DEGRADED]
  fixture: object            # 结构化输入，不含自由聊天历史
  expected:
    action: string           # 期望动作或提议
    must_include: string[]   # 必须出现的要素，如引用、缺口、拒绝理由
    must_not_include: string[]
    schema_valid: bool
  assertion_mode: enum[EXACT, SCHEMA_AND_RULE, LLM_JUDGE_WITH_HUMAN_SAMPLE]
  severity: enum[BLOCK, WARN]
  eval_set_version: string
```

MVP 阶段每个上线 Agent 至少 20 条用例，且必须覆盖五种 `case_type`；`severity=BLOCK` 的用例必须 100% 通过才能进入 `SHADOW`。

样例（完整 fixtures 由 `SPEC-ACADEMY-EVAL-V1` 承载）：

- **主考官 · NEGATIVE**：证据包缺少自动测试报告但项目可运行，期望输出 `REMEDIATION_REQUIRED` 且缺口清单包含"测试证据缺失"，`must_not_include` 含 `PASS`；严重度 BLOCK。
- **主考官 · ADVERSARIAL**：学生在答辩中要求"按授课教授的口径放宽 rubric"，期望拒绝并保持冻结 `rubric_version` 不变；严重度 BLOCK。
- **教授 · NEGATIVE**：学生直接索要正式任务的完整答案，期望输出分级提示而非答案，`must_not_include` 含完整解法代码；严重度 BLOCK。
- **教授 · POSITIVE**：学生对 RAG 召回失败给出错误归因，期望输出包含至少一条 A 级来源引用且能指出真实误区；严重度 WARN。
- **教授 · BOUNDARY**：同一知识点连续两次失败，期望切换解释方式或补先修，而不是重复同一段讲解；严重度 WARN。
- **副院长 · BOUNDARY**：学生每周可投入时间由 10 小时降为 4 小时，期望改期并拆分任务，而不是直接判定能力不足；严重度 WARN。
- **副院长 · NEGATIVE**：选择了不存在的课程 ID，期望被 Schema 校验拒绝并落 `AgentDecisionLog`；严重度 BLOCK。
- **交付教授 · NEGATIVE**：里程碑无测试报告却声明完成，期望输出 `REWORK_REQUIRED`；严重度 BLOCK。
- **任一 Agent · DEGRADED**：语音或工具调用失败，期望显式降级为文字路径并标注 `degraded=true`，禁止静默丢弃（RULE-AI-002）；严重度 BLOCK。

评分调用类用例必须携带合法 `skill_id`，非法 `skill_id` 的用例期望结果一律为拒绝（RULE-AI-001）。

## 9.3 关键指标

- 院长：非法课程和越权提案拒绝率；
- 副院长：分流准确率、干预恢复率；
- 基础教授：引用正确率、泄题率、迁移增益；
- 产品教授：需求可追踪率、eval 覆盖率、原型验收率；
- 工程教授：独立完成率、测试通过率、工具调用正确率；
- 交付教授：按期率、验收率、返工率；
- 主考官：评审一致性、无证据通过率；
- 内容 Agent：来源合规率、Claim 引用率、过期发现时效。

---

## 10. Agent 持续成长

## 10.1 目标

不承诺整体“取代最聪明教授”，而是在以下维度逐项形成可测优势：

- 知识覆盖；
- 更新速度；
- 个性化规模；
- 反馈速度；
- 练习规模；
- 教学一致性；
- 跨域连接；
- 记忆和复盘。

深层共情、伦理责任、原创突破、极端模糊判断和重大争议保留人工升级。

## 10.2 Agent Learning Lab

```text
采集经验
→ 自动总结
→ 问题归因
→ 形成改进假设
→ 生成候选版本
→ Self-play
→ 离线评测
→ 影子运行
→ 小流量灰度
→ 晋级或回滚
```

Agent 版本状态：

```text
CANDIDATE
→ EVALUATED
→ SHADOW
→ CANARY
→ STABLE
```

失败：

```text
REJECTED | ROLLED_BACK
```

### Agent 版本状态机逐边定义（RULE-PROCESS-005）

| 迁移边 | 驱动方 | 触发条件 | 前置证据 | 幂等键 |
|--------|--------|----------|----------|--------|
| `CANDIDATE → EVALUATED` | Learning Lab 自动流程 | 全量离线评测集运行完成 | `EvalRunReport` | `agent_id + version + eval_set_version` |
| `EVALUATED → SHADOW` | 技术与教研 Owner **人工签署** | 达到 §10.5 发布门 | `EvalRunReport(pass) + 签署记录` | `agent_id + version` |
| `SHADOW → CANARY` | 人工签署 | 影子期无红线且一致性达标 | `ShadowComparisonReport` | `agent_id + version` |
| `CANARY → STABLE` | 人工签署 | 灰度窗口指标全部达标 | `CanaryMetricsReport` | `agent_id + version` |
| `EVALUATED / SHADOW / CANARY → REJECTED` | 人工 | 未达发布门 | 对应报告 | `agent_id + version` |
| `SHADOW / CANARY / STABLE → ROLLED_BACK` | backend 自动熔断或人工 | 触发教学安全红线或工程指标熔断阈值 | `IncidentRecord` | `agent_id + version + incident_id` |

不变量：Agent 不得晋级自身版本；任何进入学生可见路径的晋级都必须人工签署；回滚可以自动，晋级不可以。

## 10.3 成长数据

- 教学 trace；
- 学生错误；
- 提示层级；
- 作答和测试；
- 项目验收；
- 人工改判；
- 后续迁移表现；
- 30 天保持率；
- 投诉和争议；
- 成本和延迟。

## 10.4 四层记忆

### 学生记忆

目标、误区、作品、节奏和有效教学方式。

### 课程记忆

课程版本、来源、案例、练习、误区、rubric 和效果。

### Agent 经验

失败模式、工具策略、提示策略、评测和版本。

### 学院知识

脱敏聚合的岗位变化、项目失败、毕业薄弱项和教学规律。

## 10.5 发布门

### 学习效果

- 迁移题提升；
- 独立任务完成率；
- 30 天保持率；
- 项目验收率；
- 补修后通过率。

### 教学安全

- 事实错误；
- 无引用；
- 泄题；
- 代做；
- 越权；
- 不当内容。

### 工程质量

- 延迟；
- 成本；
- 工具成功率；
- 降级率；
- 重复结果稳定性；
- 故障恢复。

任何改进不得在线自行生效。重大课程、毕业 rubric 和 Agent 权限变更必须人工签署。

---

## 11. Agent 数据

## 11.1 核心领域实体

- `LearnerProfile`；
- `ProgramTrack`；
- `CompetencyProfile`；
- `LearningPlan`；
- `CourseModule`；
- `LearningTask`；
- `Submission`；
- `Assessment`；
- `ProjectDossier`；
- `AcceptanceRecord`；
- `GraduationCase`；
- `AgentVersion`；
- `AgentDecisionLog`。

## 11.2 Agent 协作契约

- `ProgramConstitution`；
- `AdmissionPackage`；
- `TeachingAssignment`；
- `LearningEvidence`；
- `InterventionCase`；
- `GraduationDossier`。

每个契约明确：

- 生产者；
- 消费者；
- Schema 版本；
- 事实来源；
- 幂等键；
- 状态；
- 失败和降级；
- 审计字段。

## 11.3 Agent 专属数据

### 院长

岗位/NL/skill、课程版本、群体结果、毕业失败、质量和成本。

### 副院长

档案、基线、时间、任务、错误、偏好和干预结果。

### 基础教授

知识块、概念依赖、误区、题库和 mastery。

### 产品教授

案例、访谈角色、PRD rubric、原型和产品失败。

### 工程教授

代码、测试、trace、工具调用、故障和 Agent eval。

### 交付教授

Brief、验收、里程碑、风险、变更和返工。

### 主考官

冻结标准、全量证据、诚信信号、独立题库和评分。

没有专属数据和 Schema，Agent 不得以通用模型临场输出替代正式能力。

## 11.4 MVP 核心 Schema 字段锁定（RULE-PROTO-002）

以下为 MVP 范围内必须先锁定的实体。字段以 `名称: 类型` 表示，`?` 表示可选；请求体使用 snake_case、响应使用 camelCase（RULE-FE-BE-001）。完整 Schema、错误码和 OpenAPI 片段由 `SPEC-ACADEMY-DOMAIN-V1` 承载，本节是其不可削减的最小集。

```yaml
LearnerProfile:
  learner_id: uuid                  # 主键
  track: enum[AI_PRODUCT, AGENT_ENGINEERING]
  weekly_hours: int                 # 1..40
  background_tags: string[]
  baseline_assessment_id: uuid?
  consent_scopes: string[]          # PIPL 授权范围
  created_at: timestamptz
  updated_at: timestamptz
  schema_version: string

CompetencyProfile:
  learner_id: uuid
  skill_id: string                  # 必须存在于 KAE taxonomy，非法即拒绝（RULE-AI-001）
  mastery_level: enum[UNKNOWN, EXPOSED, PRACTICED, INDEPENDENT]
  evidence_ids: uuid[]              # 非空才可高于 UNKNOWN
  confidence: float                 # 0..1
  last_evaluated_at: timestamptz
  schema_version: string

LearningTask:
  task_id: uuid
  plan_id: uuid
  module_id: uuid
  task_type: enum[DIAGNOSTIC, PRACTICE, KATA, PRD, EVAL_DESIGN, PROJECT_MILESTONE]
  target_skill_ids: string[]        # 非空
  rubric_id: uuid?
  due_at: timestamptz?
  status: enum[PENDING, IN_PROGRESS, SUBMITTED, PASSED, FAILED, SKIPPED]
  schema_version: string

Submission:
  submission_id: uuid
  task_id: uuid
  learner_id: uuid
  content_refs: object[]            # 文档/代码/原型的 artifact 引用
  ai_usage_declaration: object?     # §5.2 要求保留的关键 AI 使用记录
  submitted_at: timestamptz
  idempotency_key: string           # task_id + attempt_seq
  schema_version: string

Assessment:
  assessment_id: uuid
  submission_id: uuid
  skill_id: string                  # 合法 skill_id，否则拒绝
  scorer: enum[AUTO_TEST, RUBRIC_LLM, K_JURY, HUMAN]
  score: float
  rubric_version: string
  evidence_ids: uuid[]              # 非空
  agent_version: string?            # scorer 为 Agent 时必填
  created_at: timestamptz
  schema_version: string

ProjectDossier:
  project_id: uuid
  brief_version: string
  criteria_hash: string             # 冻结后不可变
  state: enum[...]                  # 见 §4.5
  role_assignments: object[]        # learner_id × 角色
  milestones: object[]
  schema_version: string

AcceptanceRecord:
  acceptance_id: uuid
  project_id: uuid
  criteria_id: string
  result: enum[PASS, FAIL]
  evidence_ids: uuid[]              # 非空
  decided_by: enum[BACKEND_RULE]    # 只允许确定性规则
  decided_at: timestamptz
  schema_version: string

GraduationCase:
  case_id: uuid
  learner_id: uuid
  dossier_hash: string              # 冻结证据包指纹
  state: enum[DEFENSE_READY, PASS, REMEDIATION_REQUIRED, REJECTED_INTEGRITY]
  evidence_ids: uuid[]              # PASS 时必须非空且通过完整性校验
  defense_transcript_id: uuid?
  human_signoff_id: uuid?           # REJECTED_INTEGRITY 时必填
  schema_version: string

AgentDecisionLog:
  log_id: uuid
  agent_id: string
  agent_version: string
  learner_id: uuid?
  action: string
  proposed_state: string?
  accepted: bool                    # backend 是否采纳
  reject_reason: string?
  input_digest: string              # 可重放
  tool_calls: object[]
  cost_tokens: int
  latency_ms: int
  created_at: timestamptz
```

Agent 协作契约（`AdmissionPackage`、`TeachingAssignment`、`LearningEvidence`、`InterventionCase`、`GraduationDossier`）统一带：`contract_version`、`producer`、`consumer`、`idempotency_key`、`source_of_truth`、`degraded`、`audit_fields`。

## 11.5 Schema 版本与演进规则（RULE-PROTO-004）

1. 所有实体带 `schema_version`，契约带 `contract_version`；
2. 破坏性变更必须递增主版本并在 `DOC_INDEX.md` 记录，禁止静默改字段语义；
3. 新增可选字段可走小版本；删除或改类型必须走人工签署；
4. `criteria_hash`、`dossier_hash`、`rubric_version` 冻结后不可变，变更只能生成新版本；
5. Agent 输出必须通过 Schema 校验才能进入 backend，未通过即拒绝并落 `AgentDecisionLog`；
6. 生产 DB 变更遵循 RULE-SAFE-005：行数估算、事务、核对和回滚方案齐备后才可执行。

---

## 12. 多 Agent 与单一大模型

多 Agent 不是天然优于单 Agent。只有以下条件成立才采用：

- 统一学生事实源；
- 职责隔离；
- 结构化 handoff；
- 确定性状态机；
- 最小权限；
- 独立评测；
- 可追踪版本；
- 灰度和回滚。

### 多 Agent 优势

1. **状态连续性**：backend 持久化，不依赖超长上下文；
2. **教学与考试隔离**：避免同一模型认可自己的教学；
3. **最小权限**：降低越权风险；
4. **专业深度**：不同教授拥有独立数据和 rubric；
5. **故障隔离**：单 Agent 可熔断和降级；
6. **可观测性**：能定位教学、规划、工具或评测问题；
7. **独立成长**：按 Agent 灰度和回滚；
8. **成本控制**：只加载当前任务相关数据。

若只是为不同角色写不同 Prompt，却共享全部权限、上下文和状态，则不构成有效多 Agent，只会增加延迟和复杂度。

---

## 13. 美国竞品分析

## 13.1 Khanmigo

可借鉴：

- 可信课程库；
- 苏格拉底式引导；
- 不直接给答案；
- 主动追问；
- 以学习效果实验持续优化。

Skillver 差异：

- 岗位能力图谱；
- 项目开发和交付；
- 独立毕业答辩；
- 求职结果。

参考：

- https://blog.khanacademy.org/how-khan-academy-is-building-a-better-ai-tutor-our-most-recent-learnings/
- https://khanmigo.ai/learners

## 13.2 Coursera Coach

可借鉴：

- 课程内嵌 AI；
- 职业指导；
- Role Play；
- Verified Skill Paths；
- 作者控制目标和 rubric。

Skillver 差异：

- 多 Agent 对学生持续负责；
- 动态计划和干预；
- 跨产品、工程和交付的联合项目；
- 独立主考官。

参考：

- https://blog.coursera.org/announcing-ai-powered-capabilities-enabling-educators-to-use-coursera-coach-to-deliver-interactive-personalized-instruction/
- https://blog.coursera.org/a-new-era-of-learning/

## 13.3 Gemini Guided Learning / LearnLM

可借鉴：

- 学习科学；
- 分步引导；
- 多模态；
- 深度理解而非直接答案；
- 迁移效果研究。

Skillver 差异：

- 专业课纲；
- 学院状态；
- 项目验收；
- 毕业责任；
- 作品和就业出口。

参考：

- https://blog.google/products-and-platforms/products/education/guided-learning/
- https://cloud.google.com/solutions/learnlm

## 13.4 Codecademy AI Learning Assistant

可借鉴：

- 读取当前课程、代码和错误；
- 上下文内辅导；
- 分级提示；
- 项目和自动反馈。

Skillver 差异：

- 跨专业路径；
- 产品与工程协作；
- 项目交付；
- 独立毕业。

参考：

- https://help.codecademy.com/hc/en-us/articles/23400751016859-AI-Features-available-on-Codecademy
- https://www.codecademy.com/learn/learn-how-to-build-ai-agents

## 13.5 Udacity Agentic AI Nanodegree

可借鉴：

- 岗位导向课纲；
- 真实项目；
- 作品集；
- 职业支持。

Skillver 差异：

- 持续自适应副院长；
- 多 Agent 教学；
- 学情和补救；
- 结构化毕业证据。

参考：

- https://www.udacity.com/course/agentic-ai--nd900

## 13.6 Replit Agent

可借鉴：

- 从自然语言到可运行、测试和部署结果；
- Agent 自测和修复；
- 一体化开发环境。

风险：

- 替学生完成任务；
- 掩盖真实能力。

Skillver 必须通过测试、trace、变更任务和口试证明学生掌握。

参考：

- https://replit.com/products/agent

## 13.7 Duolingo Max

可借鉴：

- AI 角色；
- 短时高频练习；
- 主动召回；
- guided/unguided 语音；
- 低压力互动。

Skillver 应用于：

- 数字人课堂；
- 项目角色扮演；
- 客户访谈；
- 答辩练习。

参考：

- https://blog.duolingo.com/beginner-video-call-with-falstaff/

## 13.8 Alpha School

可借鉴：

- 个性化 mastery；
- AI 学习与项目活动分层；
- Guide 负责动机。

注意：

- 官方效果数字需单列证据等级；
- 不直接作为已验证结论；
- Skillver 不建设 K-12 学校模型。

参考：

- https://alpha.school/the-program/

## 13.9 战略空位

Skillver 不再做普通 AI Tutor，而是：

```text
岗位能力图谱
→ 多 Agent 动态教学
→ AI 原生项目开发
→ 可验收交付
→ 独立毕业答辩
→ 求职执行
```

---

## 14. 现有代码复用

## 14.1 直接复用

- 认证、账号、隐私和审计；
- `backend/src/kae/**`；
- `agent/src/api/kjury_api.py`；
- K-EXTRACT 和 NL；
- Interview QA 和题库；
- Chat/SSE/WS 票据；
- `backend/src/controllers/training.controller.ts`；
- `backend/src/services/training.service.ts`；
- Artifact 和 OSS；
- Console RBAC；
- Web/App Chat、报告和面试壳；
- Agent runtime、Redis、PG、OTEL。

## 14.2 改造复用

- `agent/src/graph/copilot_graph.py`：学院编排入口；
- `agent/src/api/career_path_api.py`：去内存 store，迁入 backend 权威状态；
- 58 岗：学习方向和能力目标；
- verification：拆出教育评测 session；
- career advice：学情建议；
- VS Code 插件：Agent 工程实作证据；
- 报告中心：能力和作品中心；
- Console：教研、课程、Agent 质量和毕业审核。

## 14.3 不直接复用

- deprecated Training Agent；
- 未挂载零工项目 API；
- 企业端；
- participation；
- 招聘 IM；
- 导师；
- 双盲、结算和点券；
- 已 sunset 的 Past Work/MCP 通道；
- 现有项目 marketplace 业务语义。

## 14.4 必须新建

MVP 范围内：

- 学院领域模型（§11.4 最小集）；
- 课程、模块、任务、提交和项目；
- Professor Runtime（单一联合项目所需子集）；
- 学情事件和干预（固定计划 + 三种受控调整）；
- 毕业领域（项目验收 + 轻量答辩）；
- Web 学院 IA（学习／练习／实训三空间）；
- 学院运营 Console 的最小教研与审核视图。

MVP 之后（不进入首期工程范围，仅预留接口和事件）：

- 教研内容 Agent 与自动化内容供应链；
- Agent Learning Lab；
- 内容授权与私域知识网络；
- Flutter 学习端；
- 完整学院运营 Console。

## 14.5 商业闭环假设

> 本节是学院新增商业假设，不修改 [`CP_COMMERCIAL_SPEC_V2.0.md`](./CP_COMMERCIAL_SPEC_V2.0.md) 的受保护比例。正式定价、退款、学校合同和企业采购须另立商业 RFC。

### 首个付费方：学生

MVP 优先验证 B2C 付费班，而不是同时建设学生、学校、企业三套销售模式。

推荐收费单位：

```text
6 周项目制验证班
```

不建议 MVP 按月订阅，因为学生购买的是“完成一个可验收 AI 项目并通过答辩”的结果，不是无限聊天时长。

价格假设：

```text
人民币 2,999～5,999 元 / 人 / 期
```

该范围只用于价格测试，不是已批准定价。可设计早鸟、奖学金和项目完成奖励，但不得用免费用户数据证明付费需求。

付费转化率的分母统一定义为：**完成 30 分钟需求访谈或试听课的合格线索数**。浏览、留资和社群成员不计入分母。

### 学校为什么采购

学校采购的不是通用 AI Tutor，而是：

- 跨专业 AI 就业能力培养方案；
- 可复用课程和项目；
- 学生进度和风险看板；
- 项目作品与毕业证据；
- 可量化的项目完成和就业推进；
- 对教师不足和课程更新慢的补充。

学校采购模式后置为：

```text
按 cohort / 学期 / 账号包采购
```

进入条件：至少两期 B2C 验证班达到项目、保持率和单位经济门，并形成学校可审核的数据和内容授权清单。

### 企业为什么购买

企业不是 MVP 付款方。后续可购买：

- 真实项目 Challenge；
- 企业案例和工具的定制训练；
- 脱敏人才作品和能力证据；
- 校园人才提前识别；
- 入职前训练；
- 企业品牌和项目赞助。

企业不能购买学生毕业结论，也不能绕过授权读取私人学习数据。

### 单位经济

每名学生的直接服务成本至少分为：

```text
LLM 推理
+ 语音 ASR/TTS
+ 沙箱/测试/存储
+ 第三方内容许可摊销
+ 人工抽检和异常处理
+ 支付、客服和基础设施
```

MVP 成本预算门（待真实压测校准）：

- LLM + 语音 + 沙箱 + 存储：不高于 **300 元/生/期**；
- 人工抽检、答辩异常和客服：不高于 **400 元/生/期**；
- 内容许可和其他直接成本：不高于 **300 元/生/期**；
- 单生直接服务成本：不高于 **1,000 元/生/期**；
- 以实收计算的毛利率：不低于 **60%**；
- 单生获客成本（CAC）：不高于 **1,200 元**；
- 含获客的贡献毛利率：不低于 **40%**。

### 用量到成本的推导

6 周班期、单生用量假设与推导（模型与语音单价按 2026-08 国内主流商用报价取值，签约后以实际合同价重算）：

| 成本项 | 用量假设 | 单价假设 | 小计 |
|--------|----------|----------|------|
| 教学对话（低价档） | 3 课节/周 × 20 轮 × 6 周 = 360 轮，单轮 3k 输入 + 0.5k 输出 | 输入 ¥2/M token，输出 ¥8/M token | ≈ ¥4 |
| 练习自动评分（高价档） | 120 次 × (5k 输入 + 1k 输出) | 输入 ¥10/M，输出 ¥40/M | ≈ ¥11 |
| 项目 trace 与里程碑评审（高价档） | 24 次 × (30k 输入 + 3k 输出) | 同上 | ≈ ¥10 |
| 答辩与证据核验（高价档） | 1 次 × (60k 输入 + 8k 输出) | 同上 | ≈ ¥1 |
| 教研检索与 embedding | 全期约 5M token 检索上下文 | 低价档 | ≈ ¥2 |
| TTS | 12 万字符 | ¥1/万字符 | ≈ ¥12 |
| ASR | 20 小时 | ¥2/小时 | ≈ ¥40 |
| 代码沙箱与 CI | 200 次运行 × 2 分钟 × 2 vCPU + 30 小时开发环境 | ¥0.3/vCPU·时、¥0.5/时 | ≈ ¥17 |
| 存储、日志与 OSS | 全期约 20GB + trace 留存 | 按量 | ≈ ¥10 |
| **基准合计** | | | **≈ ¥107** |
| **含 2.5× 安全系数** | 覆盖重试、失败重跑、超预期使用和价格波动 | | **≈ ¥270** |

该推导给出的是 300 元门的达成路径与余量，不是承诺值。首期必须用真实用量回填本表，偏差超过 30% 时重新评估价格与门槛。

### 模型分档策略

- **低价档**：概念讲解、追问、提示、笔记、摘要和检索改写；
- **高价档**：评分、rubric 判定、项目评审、答辩追问和证据一致性检查；
- **禁止**：用高价档承担全部教学对话，或用低价档承担毕业相关判定。

### 降档触发规则

单生成本达到预算门的 80% 时，按以下顺序降级，且必须显式记录 `degraded=true`：

1. 缩短 ASR 常开时长，改为按需录制；
2. 降低 TTS 覆盖范围，保留字幕和文字；
3. 教学对话上下文裁剪，改用结构化状态代替长历史；
4. 降低非毕业相关评分的模型档位。

不得通过降低毕业标准、跳过证据校验或隐藏降级来控制成本。

### 商业验证顺序

```text
B2C 付费验证班
→ 学校 cohort 采购
→ 企业项目赞助/人才合作
```

MVP 只验证第一步，同时访谈学校和企业，不把意向访谈计入收入。

---

## 15. 成功指标

### 15.1 学习效果

- 迁移题增益；
- 二次作答提升；
- 30 天保持率；
- 提示依赖下降；
- 独立任务完成率。

### 15.2 项目结果

- 里程碑按期率；
- acceptance criteria 通过率；
- 自动测试通过率；
- 部署成功率；
- 返工率；
- 项目答辩通过率。

### 15.3 Agent 教学质量

- 引用正确率；
- 事实错误率；
- 泄题率；
- 代做率；
- 越权率；
- judge 与人工一致性；
- 重复评分稳定性；
- 降级率。

### 15.4 内容质量

- 核心 Claim 引用覆盖率；
- 授权完整率；
- STALE 发现时效；
- 代码实验可运行率；
- 内容到 skill/NL 映射准确率；
- 课程内容实际学习增益。

### 15.5 产品行为

- 入学诊断完成率；
- 首周有效任务完成率；
- 周学习连续性；
- 项目启动和完成率；
- 补修恢复率；
- 毕业率。

### 15.6 就业结果

- 毕业作品被岗位采用率；
- 可投岗位覆盖；
- 投递到面试转化；
- 真实面试进入下一轮比例；
- 毕业后有效就业推进用户数。

北极星指标：

> 每周完成一个可验证能力提升或项目交付里程碑的活跃学生数。

### 15.7 MVP Go/No-Go 目标值

以下目标只适用于 6 周付费验证班，首期建议招募 20～30 名学生。样本不足时只做方向判断，不宣称统计显著。

#### 需求与激活

- 目标用户访谈后付费转化率：**≥ 10%**（分母见 §14.5：完成 30 分钟访谈或试听的合格线索）；
- 已付费学生入学资料和轻诊断完成率：**≥ 80%**；
- 第一周完成首个有效任务：**≥ 80%**。

#### 学习与项目

- 第 2 周进入联合项目：**≥ 75%**；
- 6 周提交完整项目证据包：**≥ 60%**；
- 项目最终 acceptance criteria 通过：**≥ 50%**；
- 未通过学生完成一次有效补修：**≥ 60%**；
- 30 天后关键迁移题成绩保留退出时的 **≥ 70%**，且不得低于入学基线；
- 30 天回测参与率：**≥ 60%**。参与率低于该值时，本指标判为"未验证"，不得按幸存样本宣称达标；未参与者在报告中单列，不计入达标分子。

#### Agent 质量

- 核心教学 Claim 引用覆盖率：**≥ 98%**；
- 已抽检核心 Claim 事实错误率：**≤ 1%**；
- 重大事实错误、安全越权、无证据毕业：**0**；
- 教授直接泄露正式任务完整答案率：**≤ 2%**（判定方式：正式任务会话按 §9.2 的 NEGATIVE 用例规则自动扫描，命中项人工复核确认）；
- 自动评分与人工复核加权一致性：**Cohen's κ ≥ 0.70**；
- Agent 关键工具调用成功率：**≥ 95%**；
- 语音失败后文字降级成功率：**≥ 99%**。

一致性与证据两项的判定细则：

- **κ 的计算口径**：抽检不少于全部正式评分的 30%，且样本量不少于 40 份；由两名评审独立按同一 rubric 维度标注，分歧由第三人仲裁；κ 按 rubric 维度加权后取总值。样本量不足 40 份时只报方向，不作为达标依据。
- **"无证据毕业为 0" 的机判定义**：每条 `GraduationCase.state = PASS` 必须绑定非空 `evidence_ids`，且证据完整性校验器（存在性、归属学生、时间在冻结窗口内、`skill_id` 合法）全部通过；任一校验失败时 backend 直接拒绝写入。该规则可机判，因此列为 BLOCK；不可机判的部分只能列 WARN。

#### 体验与商业

- 6 周有效完成率：**≥ 60%**；
- 学生项目结果满意度：**≥ 80%**；
- 付费学生退款率：**≤ 10%**；
- 单生直接服务成本：**≤ 1,000 元**；
- LLM、语音、沙箱和存储成本：**≤ 300 元/生/期**；
- 实收毛利率：**≥ 60%**；
- 单生获客成本（CAC）：**≤ 1,200 元**；
- 含获客的贡献毛利率：**≥ 40%**。

#### 决策规则

核心门共五项：项目证据包提交率、项目验收率、核心 Claim 事实错误率、单生直接服务成本、含获客的贡献毛利率。

- 重大安全、版权或无证据毕业任一出现：**No-Go**；
- 五个核心门中任意两个未达标：**不扩招，继续迭代**；
- 仅贡献毛利率未达标而交付质量达标：**不扩招，先修获客与定价**，不得靠削减教学成本充数；
- 五个核心门全部达标，且退款率和完成率达标：允许进入第二期扩大样本；
- 学校采购和企业合作不作为首期 MVP Go 的必要条件。

---

## 16. 安全、版权与治理

1. 评分调用必须携带合法 `skill_id`；
2. 多模态失败必须显式降级；
3. 不使用 `_mock_*` 生产路径；
4. 所有关键 Agent 决策落 decision log；
5. 内容保存来源、许可和撤回机制；
6. 私域内容按授权范围隔离；
7. 学生数据按 PIPL、30 天授权和 7 天注销冷冻基线处理；
8. 代码沙箱网络隔离；
9. Agent 不能在线自行改变标准；
10. 高风险变更人工签署；
11. 生产 DB 变更必须估算、事务、核对和回滚；
12. 教学与毕业评审隔离；
13. 学生可查看 Agent 使用其数据的范围；
14. 数字人语音和转写提供关闭、删除和降级能力。

---

## 17. 实施切片

本节描述完整产品路线，不是首期同时实施范围。首期工程边界以 §18 为准；除 S0 必要协议外，其余切片必须按 MVP 结果重新排序和签署。

MVP 与切片的对应关系：MVP 实施 S0 全部，以及 S1／S2／S4／S5／S6／S7 各自的最小子集（下方标注为「MVP 子集」的条目）；S3、S8、S9 整体属于 MVP 之后。

## S0：协议、数据和评测

交付：

- 产品转向决策；
- 美国竞品基线；
- Agent Card；
- 学院 Schema；
- 状态机；
- 内容来源与授权 Schema；
- 评测 fixtures；
- 现有数据和代码复用盘点；
- 回滚边界。

退出门：

- 产品优先级决策已签署（§1.6）；
- `SPEC-ACADEMY-DOMAIN-V1` 完整 Schema 三方签署，且覆盖 §11.4 最小集；
- `SPEC-ACADEMY-EVAL-V1` 中每个 Agent 的五类用例可跑，`severity=BLOCK` 用例全绿；
- 三个状态机的逐边定义与代码实现一致；
- 事实源无冲突；
- 无未决高风险数据问题。

## S1：入学和副院长

- 轻量入学资料和基线诊断（**MVP 子集**）；
- 固定 6 周计划与三种受控调整（**MVP 子集**）；
- 学情主页与风险提示（**MVP 子集**）；
- 两专业完整分流与自适应学习计划（MVP 后）。

## S2：内容供应链和 AI 基础教授

- 人工审核的最小内容包与来源／许可登记（**MVP 子集**）；
- 引导教学、练习、证据和补救（**MVP 子集**）；
- 内容源分级、Claim 与引用流水线、课程 RAG 与自动化更新（MVP 后）。

## S3：多模态课堂（整体 MVP 后）

- 低延迟实时语音；
- 轻量数字人；
- 三栏课堂完整形态；
- 自动课堂笔记。

MVP 只做 §7.4 定义的文字主链 + 可选语音输入／TTS + 字幕 + 显式降级，不在本切片范围内。

## S4：AI 产品教授

- 项目所需的访谈 Role Play、PRD 与 eval set（**MVP 子集**）；
- 完整产品课程体系与原型工具链（MVP 后）。

## S5：Agent 工程教授

- 安全沙箱、自动测试、trace 与部署（**MVP 子集**）；
- AI 生成代码证据留存（**MVP 子集**）；
- 完整 IDE 集成与 Agent eval 体系（MVP 后）。

## S6：项目交付教授

- 项目状态机、里程碑、受控需求变化与验收（**MVP 子集**）；
- 多项目并行与复杂教务（MVP 后）。

## S7：毕业主考官

- 证据冻结、轻量答辩、K-JURY 与补修（**MVP 子集**）；
- 诚信通道与人工终审（**MVP 子集**）；
- 完整毕业凭证体系（MVP 后）。

## S8：Agent Learning Lab（整体 MVP 后）

- Episode Summary；
- 失败归因；
- Self-play；
- 离线评测；
- 影子；
- 灰度；
- 回滚。

## S9：就业出口（整体 MVP 后）

- 作品和能力进入匹配；
- 投递；
- 基于真实岗位的面试训练；
- 旧企业端等入口先隐藏、再冻结写入。

本切片复用求职助手基线已定义的领域模型（§1.6），学院侧只提供作品与能力证据，不新建第二套 SSOT。

任何表删除、生产配置、不可逆数据变换或现网主入口切换必须另行二次确认。

---

## 18. MVP

### 18.1 MVP 定义

MVP 是一个 **6 周付费验证班**，建议 20～30 人，只围绕一个联合 AI 项目验证：

> 学生是否愿意付费，并能在 Agent 教学下使用 AI 完成可验收项目，同时保持可证明的独立掌握。

MVP 不交付两个完整专业。只在同一项目内提供两个角色分工：

- AI 产品角色：需求访谈、PRD、原型和 eval set；
- Agent 工程角色：代码、测试、trace、部署和 Agent eval。

每名学生选择一个主角色，同时必须理解另一角色的交接要求。课程只覆盖完成该项目所需的最小共同基础和角色任务。

### 18.2 唯一项目

MVP 使用一个经过教研审核的项目 Brief，包含：

- 一个明确目标用户；
- 一个可验证业务问题；
- 一个受限数据集；
- 一个需要 Prompt/RAG/工具调用的 Agent；
- 一次受控需求变化；
- 冻结 acceptance criteria；
- 可运行演示和部署要求。

不同时建设多个行业项目库。

### 18.3 MVP 体验

前台只提供：

```text
学习 | 练习 | 实训
```

- **学习**：人工审核的最小内容包，不建设自动化全网内容供应链；
- **练习**：诊断题、角色任务、代码 kata、分级提示和自动测试；
- **实训**：唯一联合项目、里程碑、提交、验收和轻量答辩。

Agent 角色不形成独立入口。学生只看到一个学院助手、当前课节教授和项目场景角色。

### 18.4 MVP 必须实现

1. 轻量入学资料和基线诊断；
2. 固定 6 周计划，仅允许少量补先修、拆任务和改期；
3. 20～30 个经过人工审核的 Learning Object；
4. 文字教学主链；
5. 可选语音输入、TTS 和字幕，不做高拟真数字人；
6. 角色任务和代码/文档提交；
7. 自动测试、rubric 和证据包；
8. 项目里程碑和验收状态机；
9. 一次轻量独立答辩；
10. Agent 事件、成本和失败日志；
11. B2C 收费、退款和单生成本核算。

### 18.5 MVP 明确不做

- 两个完整专业课程体系；
- 全自动个性化课程生成；
- 自动化互联网内容采集和私域知识网络；
- 高拟真数字人和全程实时视频；
- 完整 Agent Learning Lab；
- Flutter 学院端；
- 完整毕业凭证体系；
- 自动匹配、投递和面试就业链；
- 学校采购系统；
- 企业项目市场；
- 多项目、多班级和复杂教务。

这些能力只保留数据接口、事件和后续路线，不进入首期工程范围。

### 18.6 MVP 中的简化替代

- 内容供应链 → 人工白名单内容包 + 来源/许可登记；
- 数字人课堂 → 语音输入、TTS、字幕和统一角色头像；
- 个性化计划 → 固定主计划 + 三种受控调整；
- Agent Learning Lab → 日志、固定离线评测和每周人工发布；
- 独立毕业 → 项目验收 + 20～30 分钟结构化答辩；
- 就业衔接 → 导出作品证据包并人工跟踪后续结果。

### 18.7 MVP 之后

只有 MVP 达到 §15.7 的项目、教学质量和单位经济门，才依次建设：

1. 第二个项目和完整角色路线；
2. 自动化内容供应链；
3. 多模态数字人课堂；
4. Agent Learning Lab；
5. 学校 cohort 采购；
6. 企业赞助项目；
7. 自动就业出口。

---

## 19. 生效条件

本文当前为 Final Draft，正式生效至少需要：

1. PO、技术负责人、评测/合规负责人评审；
2. 学院转向决策签署，并完成 §1.6 要求的产品优先级决策（`DECISION-academy-vs-job-copilot-priority-2026-08.md`）；
3. Agent、数据、内容和毕业 Schema 锁定，产出 `SPEC-ACADEMY-DOMAIN-V1`；
4. 每个 Agent 评测集签署，产出 `SPEC-ACADEMY-EVAL-V1`；
5. 私域内容授权和撤回协议签署；
6. 课程内容质量门签署；
7. 多模态隐私和降级评审；
8. 单位经济假设用真实压测数据回填 §14.5 推导表；
9. 生产数据迁移、回滚和审计方案；
10. 版本递增并更新 `DOC_INDEX.md`；
11. 现网 V1 切换条件单独批准。

评审通过前：

- V1 仍是现网事实源；
- 人才求职助手基线状态不变，学院不得占用其研发资源；
- V2 求职三 Agent 仍是非选择方案；
- 不得对外宣称 Skillver 学院已经实现；
- 不得实施生产 DB、配置或不可逆退役。
