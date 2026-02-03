# AIDLC Dry Run: AwesomeShop — 亚马逊员工内部积分商城

---

## PHASE 0: 启动

**触发**: AI 收到用户请求文本。

**动作**:
1. 加载 `core-workflow.md` 主编排规则
2. 加载通用规则: `process-overview.md`, `session-continuity.md`, `content-validation.md`, `question-format-guide.md`
3. 显示 `welcome-message.md` 中的欢迎信息（仅首次）
4. 将用户完整原始请求记录到 `aidlc-docs/audit.md`

---

## PHASE 1: INCEPTION（规划与架构）

---

### Stage 1: Workspace Detection [必须执行]

**判定逻辑**:
- 检查是否存在 `aidlc-docs/aidlc-state.md` → **不存在**（新项目）
- 扫描工作区代码文件 → **无源代码**
- 结论: **Greenfield（全新项目）**

**产出文件**:
```
aidlc-docs/aidlc-state.md   ← 创建初始状态文件
aidlc-docs/audit.md         ← 记录初始请求 + 检测结果
```

**状态文件内容概要**:
```markdown
- Project Type: Greenfield
- Existing Code: No
- Reverse Engineering Needed: No
- Current Stage: INCEPTION - Workspace Detection
```

**用户交互**: 无需审批，自动进入下一阶段。

**下一步**: 跳过 Reverse Engineering（Greenfield），直接进入 Requirements Analysis

---

### Stage 2: Reverse Engineering [跳过]

**原因**: Greenfield 项目，无已有代码库。

---

### Stage 3: Requirements Analysis [必须执行, Comprehensive 深度]

**深度判定**:
- 请求清晰度: **Standard**（有明确功能描述，但缺少细节）
- 请求类型: **New Project**
- 范围估计: **System-wide**（多组件系统）
- 复杂度估计: **Complex**（多集成点、多用户角色、社交功能）
- → 深度决定: **Comprehensive**

**Step 1**: 无 Reverse Engineering 产物可加载（Greenfield）

**Step 2**: Intent Analysis
```markdown
- Request Clarity: Standard（有方向但缺细节）
- Request Type: New Project
- Scope: System-wide（积分引擎 + 商城 + 社交 + 管理后台 + 多系统集成）
- Complexity: Complex
```

**Step 3-5**: 评估需求完整性，识别以下缺失信息：
- 技术栈偏好？
- 积分规则的精确业务逻辑？
- Gift Card 面额/供应商？
- 审批流程？
- 数据隐私/合规要求？
- 预期用户规模？
- etc.

**Step 6**: 生成澄清问题文件

**产出文件**: `aidlc-docs/inception/requirements/requirement-verification-questions.md`

```markdown
# Requirements Clarification Questions

## Question 1
AwesomeShop 的主要技术栈偏好是什么？

A) Java/Spring Boot + React
B) TypeScript/Node.js (NestJS) + React
C) Python/FastAPI + React
D) Kotlin/Spring Boot + React
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 2
积分获取规则中，"日常工作表现"具体指什么？

A) Manager 手动分配积分（月度/季度考核后）
B) 系统自动基于绩效指标（如 CR 完成率、on-call 覆盖率等）
C) 两者结合：自动规则 + Manager 补充分配
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 3
P2P 互认 (Peer-to-Peer Recognition) 的积分机制如何设计？

A) 每人每月固定额度可分配给同事（如 100 积分/月）
B) 无额度限制，但需 Manager 审批
C) 固定额度 + 特别事件可申请额外额度
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 4
商城商品类型的优先级如何？

A) 以 Gift Card 为主（Amazon GC, 第三方 GC）
B) 以 Amazon 周边实物为主（T恤、背包、水杯等）
C) 以数字商品为主（课程、订阅、软件许可）
D) 三者并重，全部第一期上线
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 5
系统预期用户规模是多少？

A) < 10,000 员工（单个组织/部门）
B) 10,000 - 50,000 员工（多个组织）
C) 50,000 - 200,000 员工（大区级别）
D) 200,000+ 员工（全球级别）
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 6
与 Midway SSO 的集成方式偏好？

A) SAML 2.0 联合身份
B) OIDC (OpenID Connect)
C) 直接调用 Midway API
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 7
HR 系统集成需要获取哪些数据？

A) 仅基础员工信息（姓名、login、部门、Manager）
B) 基础信息 + 入职日期/工龄/级别
C) 基础信息 + 绩效数据（里程碑达成、晋升等）
D) 全量 HR 数据（包含组织架构树）
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 8
Manager 管理视角需要包括哪些功能？

A) 仅查看团队积分概览和消费报告
B) 积分概览 + 手动分配积分 + 审批流程
C) 全功能管理（概览 + 分配 + 审批 + 预算管理 + 分析仪表盘）
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 9
Recognition 社交功能的范围？

A) 简单的感谢卡 + 公开 Feed
B) 感谢卡 + Feed + 评论/点赞 + 标签/分类
C) 完整社交（Feed + 评论 + 点赞 + 排行榜 + 团队成就墙）
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 10
部署环境偏好？

A) AWS（全套 AWS 服务）
B) 内部 Amazon 平台（Brazil/Apollo/Pipelines）
C) 混合（核心服务 AWS，CI/CD 用内部工具）
D) Other (please describe after [Answer]: tag below)

[Answer]:
```

**用户交互**: 等待用户在文件中填写 `[Answer]:` 标签 → 收到回答后分析矛盾/歧义 → 如有问题生成 `requirement-clarification-questions.md` → 直到所有歧义解决。

**Step 7**: 生成需求文档

**产出文件**: `aidlc-docs/inception/requirements/requirements.md`

内容包含：
- Intent Analysis Summary
- Functional Requirements（积分引擎、商城、Recognition、Manager 视角）
- Non-Functional Requirements（性能、安全、可用性）
- Integration Requirements（Midway SSO、Phonetool、HR）
- Acceptance Criteria

**审批门控**: 显示完成消息，等待用户审批：
```
> REVIEW REQUIRED: 请检查 aidlc-docs/inception/requirements/requirements.md
>
> Request Changes
> Approve & Continue → 进入 User Stories
```

---

### Stage 4: User Stories [条件执行 → 执行]

**判定**:
- 新用户功能？ → **是**（商城购物、Recognition 社交、Manager 管理台）
- 多用户角色？ → **是**（普通员工、Manager、Admin）
- 复杂业务逻辑？ → **是**（积分规则、兑换流程、审批）
- 跨团队协作？ → **是**（HR集成、SSO集成）
- → **High Priority: ALWAYS Execute**

**Part 1 - Planning**:

1. 创建评估文档: `aidlc-docs/inception/plans/user-stories-assessment.md`
2. 创建故事生成计划: `aidlc-docs/inception/plans/story-generation-plan.md`（含嵌入问题）

**计划中的嵌入问题示例**:
```markdown
## Question 1
用户故事按什么方式组织？

A) User Journey-Based（按用户旅程：注册→获取积分→浏览商城→兑换→查看记录）
B) Feature-Based（按功能模块：积分系统、商城、Recognition、管理台）
C) Persona-Based（按角色：员工故事、Manager 故事、Admin 故事）
D) Other (please describe after [Answer]: tag below)

[Answer]:
```

3. 等待用户回答 → 分析歧义 → 解决后请求审批

**Part 2 - Generation**:

**产出文件**:
- `aidlc-docs/inception/user-stories/personas.md`
- `aidlc-docs/inception/user-stories/stories.md`

**Personas 示例**:
```markdown
# Personas

## Persona 1: Alex — 普通员工 (Individual Contributor)
- 角色：L5 SDE
- 目标：获取 Recognition、兑换奖励
- 痛点：希望被同事认可、积分规则透明

## Persona 2: Sarah — Manager
- 角色：L7 Manager, 管理 15 人团队
- 目标：激励团队、管理积分预算、查看团队参与度
- 痛点：缺少可见性，手动流程繁琐

## Persona 3: Admin — 系统管理员
- 角色：平台运营人员
- 目标：管理商品目录、配置积分规则、生成报告
```

**Stories 示例（INVEST 准则）**:
```markdown
## Epic 1: 积分获取
### US-1.1: 作为员工，我希望通过 P2P Recognition 获得积分
  Acceptance Criteria:
  - 每月可用额度可见
  - 发送时需选择 Recognition 类别
  - 接收方实时收到通知

### US-1.2: 作为 Manager，我希望为团队成员分配里程碑积分
  ...

## Epic 2: 商城兑换
### US-2.1: 作为员工，我希望浏览积分商城并按类别筛选商品
  ...
```

**审批门控**: 等待用户审批

---

### Stage 5: Workflow Planning [必须执行]

**加载所有前序产物**: requirements.md, stories.md, personas.md

**Scope & Impact Analysis**:
```markdown
- 用户面变更: 是（全新用户界面 + 社交功能）
- 结构性变更: 是（全新系统架构）
- 数据模型变更: 是（用户、积分、商品、订单、Recognition）
- API 变更: 是（全新 API 设计）
- NFR 影响: 是（SSO 安全、性能、可用性）
- Risk Level: Medium-High（多集成点、企业级系统）
```

**阶段决策**:

| 阶段 | 决定 | 理由 |
|------|------|------|
| Application Design | **EXECUTE** | 全新多组件系统，需要组件识别和服务层设计 |
| Units Generation | **EXECUTE** | 复杂系统需拆分为多个可管理的工作单元 |
| Functional Design | **EXECUTE** (per unit) | 复杂业务逻辑（积分规则、兑换流程） |
| NFR Requirements | **EXECUTE** (per unit) | 性能/安全/可用性需求明确 |
| NFR Design | **EXECUTE** (per unit) | 需将 NFR 模式融入设计 |
| Infrastructure Design | **EXECUTE** (per unit) | 需映射到 AWS 服务 |
| Code Generation | **EXECUTE** (always) | — |
| Build & Test | **EXECUTE** (always) | — |

**产出文件**:
- `aidlc-docs/inception/plans/execution-plan.md`
- 更新 `aidlc-docs/aidlc-state.md`

**Mermaid 可视化**: 生成包含所有阶段状态的流程图（全绿色 EXECUTE）

**审批门控**: 等待用户审批执行计划

---

### Stage 6: Application Design [条件执行 → 执行]

**产出文件**:
- `aidlc-docs/inception/application-design/components.md`
- `aidlc-docs/inception/application-design/component-methods.md`
- `aidlc-docs/inception/application-design/services.md`
- `aidlc-docs/inception/application-design/component-dependency.md`

**组件识别示例**:
```markdown
# Components

## 1. Points Engine
- 积分发放、扣减、过期、余额查询
- 规则引擎（P2P/Manager/Milestone/Auto）

## 2. Catalog Service
- 商品管理（CRUD）、分类、库存
- 商品类型：Physical / Gift Card / Digital

## 3. Order Service
- 兑换下单、订单状态、履约跟踪

## 4. Recognition Service
- P2P Recognition 发送/接收
- 社交 Feed、评论、点赞

## 5. User Profile Service
- 与 Midway SSO 集成认证
- 与 Phonetool 集成获取头像/组织信息
- 与 HR 系统集成获取员工数据

## 6. Manager Dashboard Service
- 团队积分概览、预算管理
- 审批工作流、分析报告

## 7. Notification Service
- 多渠道通知（Email/Push/In-app）

## 8. Admin Service
- 平台配置、商品管理、规则管理、报告
```

**审批门控**: 等待用户审批

---

### Stage 7: Units Generation [条件执行 → 执行]

**Part 1 - Planning**: 生成 `unit-of-work-plan.md` 含问题

**问题示例**:
```markdown
## Question 1
部署模型偏好？

A) Microservices（每个组件独立部署）
B) Modular Monolith（单体但模块化）
C) 混合（核心服务独立，辅助功能合并）
D) Other

[Answer]:
```

**Part 2 - Generation**:

**产出文件**:
- `aidlc-docs/inception/application-design/unit-of-work.md`
- `aidlc-docs/inception/application-design/unit-of-work-dependency.md`
- `aidlc-docs/inception/application-design/unit-of-work-story-map.md`

**可能的工作单元拆分**（假设选择 Microservices）:
```markdown
# Units of Work

## Unit 1: points-engine
- 职责：积分引擎核心（获取/消费/余额/规则）
- Stories: US-1.1, US-1.2, US-1.3, US-1.4

## Unit 2: catalog-service
- 职责：商品目录管理
- Stories: US-2.1, US-2.2

## Unit 3: order-service
- 职责：兑换订单与履约
- Stories: US-2.3, US-2.4, US-2.5
- Dependencies: points-engine, catalog-service

## Unit 4: recognition-service
- 职责：P2P Recognition + 社交功能
- Stories: US-3.1, US-3.2, US-3.3
- Dependencies: points-engine, user-profile

## Unit 5: user-profile-service
- 职责：用户认证/授权 + 外部集成
- Stories: US-4.1, US-4.2
- Dependencies: Midway SSO, Phonetool, HR

## Unit 6: manager-dashboard
- 职责：Manager 视角管理功能
- Stories: US-5.1, US-5.2, US-5.3
- Dependencies: points-engine, user-profile

## Unit 7: notification-service
- 职责：通知服务
- Stories: US-6.1
- Dependencies: All services (event consumer)

## Unit 8: frontend-app
- 职责：React 前端应用
- Stories: All user-facing stories
- Dependencies: All backend services
```

**审批门控**: 等待用户审批

---

## PHASE 2: CONSTRUCTION（设计、实现、构建与测试）

**对每个 Unit 循环执行以下 4+1 阶段**，以 Unit 1 (points-engine) 为例展示完整流程：

---

### Per-Unit Loop — Unit 1: points-engine

#### (a) Functional Design [执行]

**产出文件**:
- `aidlc-docs/construction/points-engine/functional-design/business-logic-model.md`
- `aidlc-docs/construction/points-engine/functional-design/business-rules.md`
- `aidlc-docs/construction/points-engine/functional-design/domain-entities.md`

**内容示例**:
```markdown
# Domain Entities
## PointsAccount
- accountId, employeeLogin, balance, lifetimeEarned, lifetimeSpent

## PointsTransaction
- transactionId, accountId, type(EARN/SPEND/EXPIRE), amount, source, timestamp

## PointsRule
- ruleId, type(P2P/MANAGER/MILESTONE/AUTO), conditions, pointsAmount, active

## P2PAllocation
- allocationId, senderLogin, monthlyBudget, remaining, period

# Business Rules
- BR-001: P2P 月度额度不可跨月累积
- BR-002: 积分过期策略（获取后 12 个月）
- BR-003: 兑换时需余额 >= 商品价格
- BR-004: Manager 分配积分需从团队预算扣除
```

**审批门控**: Request Changes / Continue to Next Stage

#### (b) NFR Requirements [执行]

**产出文件**:
- `aidlc-docs/construction/points-engine/nfr-requirements/nfr-requirements.md`
- `aidlc-docs/construction/points-engine/nfr-requirements/tech-stack-decisions.md`

**内容示例**:
```markdown
# NFR Requirements — points-engine
- 性能: 积分查询 < 100ms P99, 积分操作 < 200ms P99
- 可用性: 99.95% uptime
- 安全: 所有操作需 Midway token 验证
- 数据一致性: 积分事务强一致（ACID）
- 审计: 所有积分变动可追溯

# Tech Stack Decisions
- Language: Java 21 / Spring Boot 3.x
- Database: Amazon Aurora PostgreSQL（积分事务强一致）
- Cache: ElastiCache Redis（余额查询加速）
- API: REST + OpenAPI 3.0
```

**审批门控**

#### (c) NFR Design [执行]

**产出文件**:
- `aidlc-docs/construction/points-engine/nfr-design/nfr-design-patterns.md`
- `aidlc-docs/construction/points-engine/nfr-design/logical-components.md`

**内容示例**:
```markdown
# NFR Design Patterns
- Circuit Breaker（外部 HR 集成调用）
- Cache-Aside Pattern（积分余额查询）
- Optimistic Locking（并发积分操作）
- Event Sourcing（积分变动全量事件流）
- Retry with Exponential Backoff（外部调用）

# Logical Components
- Redis Cache Layer
- SQS Event Queue（积分事件发布）
- CloudWatch Metrics + Alarms
```

**审批门控**

#### (d) Infrastructure Design [执行]

**产出文件**:
- `aidlc-docs/construction/points-engine/infrastructure-design/infrastructure-design.md`
- `aidlc-docs/construction/points-engine/infrastructure-design/deployment-architecture.md`

**内容示例**:
```markdown
# Infrastructure — points-engine
- Compute: ECS Fargate (2 vCPU, 4GB RAM, min 2 tasks)
- Database: Aurora PostgreSQL Serverless v2
- Cache: ElastiCache Redis (cache.t3.medium)
- Messaging: Amazon SQS (points-events queue)
- API: ALB → ECS Fargate
- Networking: VPC Private Subnets
- Monitoring: CloudWatch + X-Ray
- CDK Stack: PointsEngineStack
```

**审批门控**

#### (e) Code Generation [必须执行]

**Part 1 - Planning**: 生成 `aidlc-docs/construction/plans/points-engine-code-generation-plan.md`

```markdown
# Code Generation Plan — points-engine

## Step 1: Project Structure Setup
- [ ] Create points-engine/ directory with Spring Boot project structure

## Step 2: Domain Entities
- [ ] PointsAccount.java, PointsTransaction.java, PointsRule.java, P2PAllocation.java

## Step 3: Domain Entity Unit Tests
- [ ] PointsAccountTest.java, PointsTransactionTest.java

## Step 4: Repository Layer
- [ ] PointsAccountRepository, PointsTransactionRepository

## Step 5: Repository Unit Tests
- [ ] Repository tests with @DataJpaTest

## Step 6: Service Layer (Business Logic)
- [ ] PointsService.java (earn/spend/balance/expire)
- [ ] P2PAllocationService.java
- [ ] PointsRuleEngine.java

## Step 7: Service Unit Tests
- [ ] PointsServiceTest.java, P2PAllocationServiceTest.java

## Step 8: API Layer (Controllers)
- [ ] PointsController.java (REST endpoints)
- [ ] API documentation (OpenAPI annotations)

## Step 9: API Unit Tests
- [ ] PointsControllerTest.java (@WebMvcTest)

## Step 10: Configuration & Security
- [ ] application.yml, SecurityConfig (Midway token validation)

## Step 11: CDK Infrastructure
- [ ] PointsEngineStack (Fargate + Aurora + Redis + SQS)

## Step 12: Documentation
- [ ] README.md, API specs summary
```

**审批门控**: 审批计划后执行 Part 2

**Part 2 - Generation**: 按计划逐步生成代码到 `<workspace-root>/points-engine/src/...`，每完成一步标记 [x]。

**审批门控**: Request Changes / Continue to Next Unit

---

### 对其余 Unit 重复同样循环

```
Unit 2: catalog-service     → [Functional Design] → [NFR Req] → [NFR Design] → [Infra Design] → [Code Gen]
Unit 3: order-service       → [Functional Design] → [NFR Req] → [NFR Design] → [Infra Design] → [Code Gen]
Unit 4: recognition-service → [Functional Design] → [NFR Req] → [NFR Design] → [Infra Design] → [Code Gen]
Unit 5: user-profile-service→ [Functional Design] → [NFR Req] → [NFR Design] → [Infra Design] → [Code Gen]
Unit 6: manager-dashboard   → [Functional Design] → [NFR Req] → [NFR Design] → [Infra Design] → [Code Gen]
Unit 7: notification-service→ [Functional Design] → [NFR Req] → [NFR Design] → [Infra Design] → [Code Gen]
Unit 8: frontend-app        → [Functional Design] → [NFR Req] → [NFR Design] → [Infra Design] → [Code Gen]
```

**注意**: 每个 Unit 完成全部阶段后才进入下一个 Unit。

---

### Build & Test [必须执行, 所有 Unit 完成后]

**产出文件**:
```
aidlc-docs/construction/build-and-test/
├── build-instructions.md           ← 构建步骤（mvn/gradle/npm）
├── unit-test-instructions.md       ← 单元测试执行指令
├── integration-test-instructions.md← 集成测试（服务间交互）
├── performance-test-instructions.md← 性能测试（JMeter/k6）
├── security-test-instructions.md   ← 安全测试（依赖扫描、认证测试）
├── e2e-test-instructions.md        ← 端到端测试（用户旅程）
├── contract-test-instructions.md   ← API 契约测试（服务间）
└── build-and-test-summary.md       ← 汇总报告
```

**审批门控**: "Build and test instructions complete. Ready to proceed to Operations stage?"

---

## PHASE 3: OPERATIONS [占位]

当前为占位阶段，流程在 Build & Test 完成后结束。

---

## 全流程汇总

```
                          用户请求: "AwesomeShop..."
                                  │
        ┌─────────── INCEPTION ───┴──────────────────────┐
        │ 1. Workspace Detection [AUTO]                   │
        │ 2. Requirements Analysis [APPROVE]              │  ← 10个澄清问题
        │ 3. User Stories [APPROVE x2]                    │  ← Plan审批 + Stories审批
        │ 4. Workflow Planning [APPROVE]                  │  ← 执行计划审批
        │ 5. Application Design [APPROVE]                 │  ← 8个组件设计
        │ 6. Units Generation [APPROVE]                   │  ← 8个工作单元
        └─────────────────────────────────────────────────┘
                                  │
        ┌────────── CONSTRUCTION ─┴──────────────────────┐
        │ Per Unit (x8):                                  │
        │   a. Functional Design [APPROVE]                │
        │   b. NFR Requirements [APPROVE]                 │
        │   c. NFR Design [APPROVE]                       │
        │   d. Infrastructure Design [APPROVE]            │
        │   e. Code Generation [APPROVE x2]               │  ← Plan + Code
        │                                                 │
        │ Build & Test [APPROVE]                          │
        └─────────────────────────────────────────────────┘
```

### 审批门控统计

| 阶段 | 审批次数 |
|------|---------|
| Inception | 6 次（Req + Stories Plan + Stories + Workflow + AppDesign + Units） |
| Construction per Unit | 6 次（FD + NFR-Req + NFR-Design + Infra + CodePlan + Code） |
| Construction total (8 units) | 48 次 |
| Build & Test | 1 次 |
| **总计** | **~55 次用户审批** |

### 生成文件总览

```
aidlc-docs/
├── aidlc-state.md                              ← 全程状态跟踪
├── audit.md                                    ← 完整审计轨迹
├── inception/
│   ├── requirements/
│   │   ├── requirement-verification-questions.md
│   │   └── requirements.md
│   ├── user-stories/
│   │   ├── personas.md
│   │   └── stories.md
│   ├── plans/
│   │   ├── user-stories-assessment.md
│   │   ├── story-generation-plan.md
│   │   ├── execution-plan.md
│   │   ├── application-design-plan.md
│   │   └── unit-of-work-plan.md
│   └── application-design/
│       ├── components.md
│       ├── component-methods.md
│       ├── services.md
│       ├── component-dependency.md
│       ├── unit-of-work.md
│       ├── unit-of-work-dependency.md
│       └── unit-of-work-story-map.md
├── construction/
│   ├── plans/
│   │   ├── points-engine-functional-design-plan.md
│   │   ├── points-engine-nfr-requirements-plan.md
│   │   ├── points-engine-nfr-design-plan.md
│   │   ├── points-engine-infrastructure-design-plan.md
│   │   ├── points-engine-code-generation-plan.md
│   │   ├── ... (x8 units)
│   ├── points-engine/
│   │   ├── functional-design/
│   │   ├── nfr-requirements/
│   │   ├── nfr-design/
│   │   ├── infrastructure-design/
│   │   └── code/
│   ├── catalog-service/ ...
│   ├── order-service/ ...
│   ├── recognition-service/ ...
│   ├── user-profile-service/ ...
│   ├── manager-dashboard/ ...
│   ├── notification-service/ ...
│   ├── frontend-app/ ...
│   └── build-and-test/
│       ├── build-instructions.md
│       ├── unit-test-instructions.md
│       ├── integration-test-instructions.md
│       ├── performance-test-instructions.md
│       ├── security-test-instructions.md
│       ├── contract-test-instructions.md
│       ├── e2e-test-instructions.md
│       └── build-and-test-summary.md

<workspace-root>/
├── points-engine/src/...           ← 实际 Java 代码
├── catalog-service/src/...
├── order-service/src/...
├── recognition-service/src/...
├── user-profile-service/src/...
├── manager-dashboard/src/...
├── notification-service/src/...
└── frontend-app/src/...            ← React 前端代码
```

---

## 关键要点

1. **Greenfield 项目**：跳过 Reverse Engineering，所有其他阶段全部执行
2. **Comprehensive 深度**：因多组件、多集成点、多用户角色，所有产物均为最详尽级别
3. **8 个工作单元**：每个经历完整的 6 阶段 Construction 循环
4. **~55 次审批门控**：人类始终掌握控制权
5. **问题驱动**：所有问题通过 `.md` 文件以多选格式呈现，不在聊天中提问
6. **双重检查点**：Plan-level checkbox + Stage-level state tracking 同步更新
