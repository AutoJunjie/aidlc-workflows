# AIDLC Dry Run (Brownfield): AwesomeShop — 亚马逊员工内部积分商城

> **场景假设**: 工作区中已存在一个运行中的 AwesomeShop V1 版本（基于 Python/Django 单体架构，使用 PostgreSQL，部署在 EC2 上）。用户希望在此基础上重构并扩展功能：引入微服务架构、新增 Recognition 社交功能、Manager Dashboard、以及与 Midway SSO / Phonetool / HR 系统的集成。

---

## PHASE 0: 启动

**触发**: AI 收到用户请求文本。

**动作**:
1. 加载 `core-workflow.md` 主编排规则
2. 加载通用规则: `process-overview.md`, `session-continuity.md`, `content-validation.md`, `question-format-guide.md`
3. 显示 `welcome-message.md` 中的欢迎信息（仅首次）
4. 将用户完整原始请求记录到 `aidlc-docs/audit.md`

```markdown
## Initial Request
**Timestamp**: 2026-02-03T10:00:00Z
**User Input**: "AwesomeShop — 亚马逊员工内部积分商城。员工通过日常工作表现、P2P 互认、
里程碑达成等方式获得积分，用积分在商城兑换 Amazon 周边、Gift Card、数字商品等奖励。
支持 Manager 管理视角、Recognition 社交功能。需要与 Midway SSO、Phonetool、HR 系统集成。"
**AI Response**: "Starting AI-DLC workflow..."
**Context**: Workflow initialization
```

---

## PHASE 1: INCEPTION（规划与架构）

---

### Stage 1: Workspace Detection [必须执行]

**判定逻辑**:
- 检查是否存在 `aidlc-docs/aidlc-state.md` → **不存在**（未有 AIDLC 记录）
- 扫描工作区代码文件:
  - 发现 `.py` 文件 → **Python**
  - 发现 `requirements.txt`, `manage.py`, `settings.py` → **Django**
  - 发现 `docker-compose.yml`, `Dockerfile` → **Docker 部署**
  - 发现 `templates/`, `static/` → **Django 模板渲染前端**
  - 发现 `migrations/` → **Django ORM + PostgreSQL**
- 结论: **Brownfield（已有代码库）**
- 检查 `aidlc-docs/inception/reverse-engineering/` → **不存在**（无先前逆向工程产物）

**产出文件**:
```
aidlc-docs/aidlc-state.md   ← 创建初始状态文件
aidlc-docs/audit.md         ← 记录初始请求 + 检测结果
```

**状态文件内容**:
```markdown
# AI-DLC State Tracking

## Project Information
- **Project Type**: Brownfield
- **Start Date**: 2026-02-03T10:00:00Z
- **Current Stage**: INCEPTION - Workspace Detection

## Workspace State
- **Existing Code**: Yes
- **Programming Languages**: Python 3.11
- **Build System**: pip / Docker
- **Project Structure**: Monolith (Django)
- **Reverse Engineering Needed**: Yes
- **Workspace Root**: /home/user/awesomeshop

## Code Location Rules
- **Application Code**: Workspace root (NEVER in aidlc-docs/)
- **Documentation**: aidlc-docs/ only
```

**展示信息**:
```markdown
# Workspace Detection Complete

Workspace analysis findings:
• **Project Type**: Brownfield project
• **Language**: Python 3.11 / Django 4.x
• **Structure**: Monolithic application
• **Database**: PostgreSQL (via Django ORM)
• **Deployment**: Docker / EC2
• **Next Step**: Proceeding to **Reverse Engineering** to analyze existing codebase...
```

**用户交互**: 无需审批，自动进入 Reverse Engineering。

---

### Stage 2: Reverse Engineering [条件执行 → 执行]

**触发条件**: Brownfield 项目 + 无已有逆向工程产物 → **执行**

#### Step 1: Multi-Package Discovery

**1.1 扫描工作区**:
```markdown
## Package Discovery
- awesomeshop/            → Django 主应用 (Application)
  - accounts/             → 用户模块
  - points/               → 积分模块
  - catalog/              → 商品目录模块
  - orders/               → 订单模块
  - core/                 → 通用工具/中间件
- templates/              → Django HTML 模板
- static/                 → CSS/JS 静态资源
- tests/                  → 测试目录
- infrastructure/         → 部署脚本
  - docker-compose.yml
  - Dockerfile
  - nginx.conf
  - deploy.sh
```

**1.2 业务上下文**:
- 核心业务：员工积分获取与兑换
- 已有业务事务：注册/登录、查看积分、浏览商品、兑换下单、订单查看

**1.3 基础设施发现**:
- Docker + docker-compose（Nginx + Django + PostgreSQL + Redis）
- 无 CDK/Terraform/CloudFormation
- 手动部署到 EC2

**1.4 构建系统发现**:
- pip + requirements.txt
- Docker build
- 无 CI/CD pipeline

**1.5 服务架构发现**:
- 单体 Django 应用
- Nginx 反向代理
- Celery + Redis 异步任务队列
- PostgreSQL 数据库
- 无 API 层（Django 模板直接渲染）

**1.6 代码质量分析**:
- Python 3.11 / Django 4.x
- 有基础 pytest 测试（覆盖率约 35%）
- 无 linting 配置
- 无 CI/CD

#### Step 2: 生成业务概览文档

**产出文件**: `aidlc-docs/inception/reverse-engineering/business-overview.md`

```markdown
# Business Overview

## Business Context Diagram
(Mermaid 图：Employee → AwesomeShop → PostgreSQL / Redis)

## Business Description
- **Business Description**: AwesomeShop V1 是一个员工积分商城单体应用，支持基础的积分管理和商品兑换功能
- **Business Transactions**:
  1. 员工登录（Django auth，无 SSO）
  2. 积分查询（余额/历史）
  3. 商品浏览（按分类筛选）
  4. 积分兑换下单
  5. 订单状态查看
  6. Admin 后台管理（Django Admin）
- **Business Dictionary**:
  - Points: 员工积分
  - Redemption: 积分兑换
  - Catalog Item: 商城商品

## Component Level Business Descriptions
### accounts
- **Purpose**: 用户注册、登录、个人资料管理
- **Responsibilities**: Django auth 扩展，用户 Profile 模型

### points
- **Purpose**: 积分余额管理和交易记录
- **Responsibilities**: 积分增减、交易日志、余额查询

### catalog
- **Purpose**: 商品目录管理
- **Responsibilities**: 商品 CRUD、分类管理、库存管理

### orders
- **Purpose**: 兑换订单管理
- **Responsibilities**: 下单、订单状态流转、订单历史
```

#### Step 3: 生成架构文档

**产出文件**: `aidlc-docs/inception/reverse-engineering/architecture.md`

```markdown
# System Architecture

## System Overview
Django 单体应用，通过 Nginx 反向代理服务，Celery 处理异步任务。

## Architecture Diagram
(Mermaid 图: Browser → Nginx → Django → PostgreSQL / Redis / Celery Worker)

## Component Descriptions
### awesomeshop (Django Project)
- **Purpose**: 主应用容器
- **Type**: Application (Monolith)
- **Dependencies**: PostgreSQL, Redis

### accounts
- **Purpose**: 用户管理
- **Type**: Django App
- **Dependencies**: Django auth framework

### points
- **Purpose**: 积分引擎
- **Type**: Django App
- **Dependencies**: accounts (ForeignKey to User)

### catalog
- **Purpose**: 商品目录
- **Type**: Django App
- **Dependencies**: 无外部依赖

### orders
- **Purpose**: 订单管理
- **Type**: Django App
- **Dependencies**: points, catalog, accounts

## Integration Points
- **External APIs**: 无（V1 无外部集成）
- **Databases**: PostgreSQL 13 (单实例)
- **Third-party Services**: 无

## Infrastructure Components
- **Deployment**: Docker on EC2 (手动部署)
- **Networking**: 单台 EC2, Nginx 80/443
- **No CDK/IaC**
```

#### Step 4: 生成代码结构文档

**产出文件**: `aidlc-docs/inception/reverse-engineering/code-structure.md`

```markdown
# Code Structure

## Build System
- **Type**: pip + Docker
- **Configuration**: requirements.txt, Dockerfile, docker-compose.yml

## Key Classes/Modules
(Mermaid class diagram)

### Existing Files Inventory
- `awesomeshop/accounts/models.py` - User Profile 模型扩展
- `awesomeshop/accounts/views.py` - 登录/注册/个人资料视图
- `awesomeshop/accounts/urls.py` - 用户相关路由
- `awesomeshop/points/models.py` - PointsAccount, PointsTransaction 模型
- `awesomeshop/points/views.py` - 积分查询/历史视图
- `awesomeshop/points/services.py` - 积分业务逻辑(earn/spend)
- `awesomeshop/catalog/models.py` - CatalogItem, Category 模型
- `awesomeshop/catalog/views.py` - 商品列表/详情视图
- `awesomeshop/orders/models.py` - Order, OrderItem 模型
- `awesomeshop/orders/views.py` - 下单/订单列表视图
- `awesomeshop/orders/services.py` - 订单业务逻辑
- `awesomeshop/core/middleware.py` - 自定义中间件
- `awesomeshop/core/utils.py` - 通用工具函数
- `manage.py` - Django 管理入口
- `awesomeshop/settings.py` - 项目配置
- `awesomeshop/urls.py` - 根路由
- `requirements.txt` - Python 依赖
- `Dockerfile` - Docker 构建文件
- `docker-compose.yml` - 容器编排

## Design Patterns
### Repository Pattern (Partial)
- **Location**: services.py in points/orders
- **Purpose**: 分离业务逻辑和数据访问
- **Implementation**: 部分实现，部分视图直接操作 ORM

## Critical Dependencies
### Django 4.2
- **Usage**: Web framework
### psycopg2 2.9
- **Usage**: PostgreSQL adapter
### celery 5.3
- **Usage**: 异步任务队列
### redis 5.0
- **Usage**: Celery broker + 缓存
```

#### Step 5: 生成 API 文档

**产出文件**: `aidlc-docs/inception/reverse-engineering/api-documentation.md`

```markdown
# API Documentation

## REST APIs
当前 V1 无 REST API，所有交互通过 Django 模板视图（Server-Side Rendering）。

## Internal APIs (Django Views as URLs)
### accounts
- GET /accounts/login/ - 登录页面
- POST /accounts/login/ - 提交登录
- GET /accounts/profile/ - 个人资料
- POST /accounts/register/ - 注册

### points
- GET /points/ - 积分余额页面
- GET /points/history/ - 积分交易历史

### catalog
- GET /catalog/ - 商品列表（支持 ?category= 筛选）
- GET /catalog/<id>/ - 商品详情

### orders
- POST /orders/create/ - 创建兑换订单
- GET /orders/ - 订单列表
- GET /orders/<id>/ - 订单详情

## Data Models
### User (accounts)
- username, email, first_name, last_name, department

### PointsAccount (points)
- user (FK), balance, lifetime_earned, lifetime_spent

### PointsTransaction (points)
- account (FK), type (EARN/SPEND), amount, description, created_at

### CatalogItem (catalog)
- name, description, category (FK), points_cost, stock, image, active

### Order (orders)
- user (FK), total_points, status (PENDING/CONFIRMED/SHIPPED/DELIVERED), created_at
```

#### Step 6: 生成组件清单

**产出文件**: `aidlc-docs/inception/reverse-engineering/component-inventory.md`

```markdown
# Component Inventory

## Application Packages
- awesomeshop.accounts - 用户管理
- awesomeshop.points - 积分管理
- awesomeshop.catalog - 商品目录
- awesomeshop.orders - 订单管理
- awesomeshop.core - 通用工具

## Infrastructure Packages
- infrastructure/ - Docker + Nginx 部署脚本

## Shared Packages
- 无独立共享包（core/ 充当共享工具）

## Test Packages
- tests/ - pytest 测试（覆盖率 ~35%）

## Total Count
- **Total Packages**: 6
- **Application**: 5
- **Infrastructure**: 1
- **Shared**: 0
- **Test**: 1
```

#### Step 7: 生成技术栈文档

**产出文件**: `aidlc-docs/inception/reverse-engineering/technology-stack.md`

```markdown
# Technology Stack

## Programming Languages
- Python 3.11

## Frameworks
- Django 4.2 - Web framework
- Celery 5.3 - Async task queue
- Jinja2/Django Templates - Server-side rendering

## Infrastructure
- PostgreSQL 13 - Primary database
- Redis 7.0 - Celery broker + cache
- Nginx 1.24 - Reverse proxy
- Docker / docker-compose - Containerization
- EC2 (t3.medium) - Compute

## Build Tools
- pip - Package management
- Docker - Build and deployment

## Testing Tools
- pytest 7.4 - Test runner
- pytest-django - Django integration
- factory-boy - Test fixtures
```

#### Step 8: 生成依赖关系文档

**产出文件**: `aidlc-docs/inception/reverse-engineering/dependencies.md`

```markdown
# Dependencies

## Internal Dependencies
(Mermaid: orders → points, catalog, accounts; points → accounts; catalog → 无)

### orders depends on points
- **Type**: Runtime
- **Reason**: 兑换时需扣减积分

### orders depends on catalog
- **Type**: Runtime
- **Reason**: 订单引用商品信息

### orders depends on accounts
- **Type**: Runtime
- **Reason**: 订单关联用户

### points depends on accounts
- **Type**: Runtime
- **Reason**: 积分账户关联用户
```

#### Step 9: 生成代码质量评估

**产出文件**: `aidlc-docs/inception/reverse-engineering/code-quality-assessment.md`

```markdown
# Code Quality Assessment

## Test Coverage
- **Overall**: ~35% (Poor)
- **Unit Tests**: 部分核心逻辑有测试
- **Integration Tests**: 无

## Code Quality Indicators
- **Linting**: Not configured
- **Code Style**: Inconsistent（混用 function-based views 和 class-based views）
- **Documentation**: Poor（缺少 docstring）

## Technical Debt
- 无 REST API（全模板渲染，不利于前后端分离）
- 无 SSO 集成（使用 Django 内置 auth）
- 无 CI/CD pipeline
- 部分业务逻辑在 views.py 中（未完全分离到 services.py）
- 无数据库迁移版本管理策略
- 硬编码配置项（未使用环境变量）

## Patterns and Anti-patterns
- **Good Patterns**: 部分 service layer 分离
- **Anti-patterns**: Fat views, 缺少 API 层, 无 logging 策略
```

#### Step 10: 生成时间戳文件

**产出文件**: `aidlc-docs/inception/reverse-engineering/reverse-engineering-timestamp.md`

#### Step 11: 展示完成消息

```markdown
# Reverse Engineering Complete

Reverse engineering analysis findings:
• **Architecture**: Django 4.2 monolith with 5 application modules
• **Database**: PostgreSQL 13, 5 core models (User, PointsAccount, PointsTransaction, CatalogItem, Order)
• **Frontend**: Server-side Django templates (no API layer)
• **Infrastructure**: Docker on EC2, no IaC
• **Test Coverage**: ~35%, no integration tests
• **Technical Debt**: No SSO, no REST API, no CI/CD, inconsistent code style
• **Key Gap**: V1 lacks P2P Recognition, Manager Dashboard, SSO, Phonetool/HR integration

> REVIEW REQUIRED:
> Please examine the reverse engineering artifacts at: aidlc-docs/inception/reverse-engineering/

> WHAT'S NEXT?
> Request Changes - Ask for modifications to the analysis
> Approve & Continue - Proceed to Requirements Analysis
```

**审批门控**: 等待用户审批 ← **Brownfield 特有审批点**

**逆向工程全部产出文件一览**:
```
aidlc-docs/inception/reverse-engineering/
├── business-overview.md
├── architecture.md
├── code-structure.md
├── api-documentation.md
├── component-inventory.md
├── technology-stack.md
├── dependencies.md
├── code-quality-assessment.md
└── reverse-engineering-timestamp.md
```

---

### Stage 3: Requirements Analysis [必须执行, Comprehensive 深度]

**Brownfield 差异**: Step 1 会加载逆向工程产物

**Step 1**: 加载逆向工程上下文
```markdown
- 加载 architecture.md → 理解现有单体架构
- 加载 component-inventory.md → 理解现有 5 个应用模块
- 加载 technology-stack.md → 理解 Python/Django/PostgreSQL 技术栈
- 加载 code-structure.md → 理解现有文件清单（brownfield 修改候选）
- 加载 code-quality-assessment.md → 理解技术债务
```

**Step 2**: Intent Analysis（与 Greenfield 对比有差异）
```markdown
- Request Clarity: Standard
- Request Type: **Migration + Enhancement**（从单体迁移到微服务 + 新增功能）
- Scope: **System-wide**（架构转型 + 新模块）
- Complexity: **Complex**（架构迁移 + 新集成 + 新功能）
```

**Step 6**: 生成澄清问题文件

**产出文件**: `aidlc-docs/inception/requirements/requirement-verification-questions.md`

```markdown
# Requirements Clarification Questions

## Question 1
现有 Django 单体应用的迁移策略是什么？

A) Big Bang 迁移 — 一次性重写为微服务架构
B) Strangler Fig — 逐步抽取服务，新旧系统并行运行
C) 保留现有单体核心，仅新增功能用微服务实现
D) 重构为 Modular Monolith（模块化单体），暂不拆分微服务
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 2
现有 Python/Django 技术栈是否保留？

A) 全部保留 Python/Django，新服务也用 Django
B) 现有模块保留 Python，新服务用 Java/Spring Boot
C) 现有模块保留 Python，新服务用 TypeScript/NestJS
D) 全部迁移到新技术栈（放弃现有 Django 代码）
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 3
现有 PostgreSQL 数据库的处理策略？

A) 保留单一 PostgreSQL 实例，所有服务共享
B) 每个微服务独立数据库（Database per Service）
C) 共享核心数据库 + 新服务独立数据库
D) 迁移到 Amazon Aurora PostgreSQL
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 4
现有的 Django 模板前端如何处理？

A) 保留 Django 模板渲染，不做前端变更
B) 引入 React SPA，逐步替换 Django 模板
C) 一次性替换为 React SPA + REST API
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 5
积分获取规则中，"日常工作表现"具体指什么？

A) Manager 手动分配积分（月度/季度考核后）
B) 系统自动基于绩效指标（如 CR 完成率、on-call 覆盖率等）
C) 两者结合：自动规则 + Manager 补充分配
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 6
P2P 互认 (Peer-to-Peer Recognition) 的积分机制如何设计？

A) 每人每月固定额度可分配给同事（如 100 积分/月）
B) 无额度限制，但需 Manager 审批
C) 固定额度 + 特别事件可申请额外额度
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 7
商城商品类型的优先级如何？

A) 以 Gift Card 为主（Amazon GC, 第三方 GC）
B) 以 Amazon 周边实物为主（T恤、背包、水杯等）
C) 以数字商品为主（课程、订阅、软件许可）
D) 三者并重，全部第一期上线
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 8
系统预期用户规模是多少？

A) < 10,000 员工（单个组织/部门）
B) 10,000 - 50,000 员工（多个组织）
C) 50,000 - 200,000 员工（大区级别）
D) 200,000+ 员工（全球级别）
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 9
与 Midway SSO 的集成方式偏好？

A) SAML 2.0 联合身份
B) OIDC (OpenID Connect)
C) 直接调用 Midway API
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 10
HR 系统集成需要获取哪些数据？

A) 仅基础员工信息（姓名、login、部门、Manager）
B) 基础信息 + 入职日期/工龄/级别
C) 基础信息 + 绩效数据（里程碑达成、晋升等）
D) 全量 HR 数据（包含组织架构树）
E) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 11
Manager 管理视角需要包括哪些功能？

A) 仅查看团队积分概览和消费报告
B) 积分概览 + 手动分配积分 + 审批流程
C) 全功能管理（概览 + 分配 + 审批 + 预算管理 + 分析仪表盘）
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 12
Recognition 社交功能的范围？

A) 简单的感谢卡 + 公开 Feed
B) 感谢卡 + Feed + 评论/点赞 + 标签/分类
C) 完整社交（Feed + 评论 + 点赞 + 排行榜 + 团队成就墙）
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 13
现有数据如何迁移？

A) 全量数据迁移到新系统（用户、积分、商品、订单历史）
B) 仅迁移活跃数据（近 12 个月），历史数据归档
C) 新旧系统并行，逐步迁移
D) Other (please describe after [Answer]: tag below)

[Answer]:

## Question 14
部署环境如何升级？

A) 从 EC2 迁移到 ECS Fargate（容器化微服务）
B) 从 EC2 迁移到 EKS（Kubernetes）
C) 从 EC2 迁移到 Lambda（Serverless）
D) 保持 EC2，但引入 CDK 做 IaC
E) Other (please describe after [Answer]: tag below)

[Answer]:
```

> **Brownfield 差异**: Question 1-4, 13-14 是 Brownfield 特有的迁移策略问题，Greenfield 不需要。

**Step 7**: 生成需求文档

**产出文件**: `aidlc-docs/inception/requirements/requirements.md`

内容比 Greenfield 多出以下部分：
- **Migration Requirements**（迁移策略、数据迁移、并行运行期）
- **Existing System Constraints**（现有 Django 代码约束、数据库 schema 兼容）
- **Backward Compatibility Requirements**（迁移期间旧系统可用性）
- **Technical Debt Resolution**（代码质量改善目标）

**审批门控**: 等待用户审批

---

### Stage 4: User Stories [条件执行 → 执行]

**判定**: 与 Greenfield 相同（High Priority），但 Story 内容会包含迁移相关的用户故事。

**Brownfield 特有 Stories 示例**:
```markdown
## Epic 0: 系统迁移（Brownfield 特有）
### US-0.1: 作为运维人员，我希望在迁移期间新旧系统可以并行运行
  Acceptance Criteria:
  - 旧系统数据实时同步到新系统
  - 用户可无缝切换到新系统
  - 回滚机制可在 15 分钟内恢复旧系统

### US-0.2: 作为员工，我希望迁移后保留所有历史积分和兑换记录
  Acceptance Criteria:
  - 积分余额 100% 一致
  - 订单历史完整可查

### US-0.3: 作为员工，我希望使用 Midway SSO 登录（替代 Django auth）
  Acceptance Criteria:
  - 无需单独注册/记忆密码
  - 首次 SSO 登录自动创建新系统 Profile
```

其余 Epic（积分获取、商城兑换、Recognition、Manager）与 Greenfield 基本相同。

**产出文件**:
- `aidlc-docs/inception/user-stories/personas.md`
- `aidlc-docs/inception/user-stories/stories.md`

**审批门控**: Plan 审批 + Stories 审批

---

### Stage 5: Workflow Planning [必须执行]

**Brownfield 特有步骤**:

#### Step 2: Detailed Scope and Impact Analysis

**2.1 Transformation Scope Detection（Brownfield Only）**:
```markdown
## Transformation Scope
- **Architectural Transformation**: Monolith → Microservices
- **Infrastructure Changes**: EC2 → ECS Fargate (or chosen target)
- **Deployment Model Change**: Docker on EC2 → CDK-managed ECS
- **Frontend Transformation**: Django Templates → React SPA
- **Auth Transformation**: Django auth → Midway SSO (OIDC)
- **Database Transformation**: Single PostgreSQL → Database per Service (or shared)
```

**2.2 Change Impact Assessment**:
```markdown
### Application Layer Impact
- **Code Changes**: 所有现有 Django 模块需重构或迁移
- **New Code**: Recognition Service, Manager Dashboard, Notification Service, Frontend App
- **Dependencies**: 新增 Midway SSO SDK, Phonetool API client, HR API client
- **Configuration**: 从 Django settings 迁移到环境变量 + AWS Secrets Manager

### Infrastructure Layer Impact
- **Deployment Model**: EC2 → ECS Fargate（全面容器化）
- **Networking**: 引入 VPC, ALB, Security Groups
- **Storage**: 可能从单一 PostgreSQL 拆分为多个 Aurora 实例
- **Scaling**: 引入 Auto Scaling

### Operations Layer Impact
- **Monitoring**: 从无监控 → CloudWatch + X-Ray
- **Logging**: 从本地文件日志 → CloudWatch Logs (结构化)
- **CI/CD**: 从手动部署 → CDK Pipelines
```

**2.3 Component Relationship Mapping（Brownfield Only）**:
```markdown
## Component Relationships
- **Primary Components Being Changed**: accounts, points, catalog, orders
- **New Components**: recognition, manager-dashboard, notification, frontend-app
- **Infrastructure Components**: New CDK stacks (to be created)
- **Shared Components**: core utilities (to be refactored)

## Per-Component Change Assessment
| Component | Change Type | Change Reason | Priority |
|-----------|------------|---------------|----------|
| accounts → user-profile-service | Major | SSO migration + Phonetool/HR integration | Critical |
| points → points-engine | Major | API layer + P2P/Manager/Milestone rules | Critical |
| catalog → catalog-service | Moderate | Add API layer + Gift Card/Digital types | Important |
| orders → order-service | Moderate | Add API layer + async fulfillment | Important |
| (new) recognition-service | New | P2P Recognition + social features | Important |
| (new) manager-dashboard | New | Manager view + budget management | Important |
| (new) notification-service | New | Multi-channel notifications | Optional |
| (new) frontend-app | New | React SPA | Critical |
| infrastructure/ | Major | EC2 → ECS Fargate + CDK | Critical |
```

#### Step 5: Multi-Module Coordination Analysis（Brownfield Only）

```markdown
## Module Update Strategy
- **Update Approach**: Sequential (dependency-order)
- **Critical Path**: user-profile-service → points-engine → catalog-service → order-service

## Recommended Update Sequence
1. **user-profile-service** (先建认证基础)
   - Midway SSO 集成
   - Phonetool / HR 集成
   - 数据迁移: accounts 表 → 新 user profile 表

2. **points-engine** (依赖 user-profile)
   - 迁移现有积分逻辑
   - 新增 P2P / Manager / Milestone 规则
   - 数据迁移: points_account, points_transaction 表

3. **catalog-service** (独立)
   - 迁移现有商品目录
   - 新增 Gift Card / Digital 类型
   - 数据迁移: catalog_item, category 表

4. **order-service** (依赖 points-engine, catalog-service)
   - 迁移订单逻辑
   - 新增异步履约
   - 数据迁移: order, order_item 表

5. **recognition-service** (依赖 points-engine, user-profile)
   - 全新开发

6. **manager-dashboard** (依赖 points-engine, user-profile)
   - 全新开发

7. **notification-service** (依赖所有服务)
   - 全新开发

8. **frontend-app** (依赖所有后端服务)
   - React SPA 全新开发

## Testing Checkpoints
- Checkpoint 1: user-profile-service 完成后验证 SSO 登录
- Checkpoint 2: points-engine 完成后验证积分迁移一致性
- Checkpoint 3: order-service 完成后验证端到端兑换流程
- Checkpoint 4: 全部服务完成后集成测试

## Rollback Strategy
- 每个服务独立回滚
- 数据迁移保留旧表（双写期间）
- Nginx 路由切换可秒级回退到旧系统
```

**阶段决策（与 Greenfield 相同，但 Rationale 不同）**:

| 阶段 | 决定 | Brownfield Rationale |
|------|------|---------------------|
| Application Design | **EXECUTE** | 需从单体重新设计组件边界和服务层 |
| Units Generation | **EXECUTE** | 需按依赖顺序拆分工作单元 |
| Functional Design | **EXECUTE** (per unit) | 迁移 + 新增业务逻辑均需设计 |
| NFR Requirements | **EXECUTE** (per unit) | 从无 NFR 到企业级 NFR |
| NFR Design | **EXECUTE** (per unit) | 引入设计模式替代现有技术债务 |
| Infrastructure Design | **EXECUTE** (per unit) | 从 EC2 迁移到 ECS/CDK |
| Code Generation | **EXECUTE** (always) | 修改现有文件 + 创建新文件 |
| Build & Test | **EXECUTE** (always) | — |

**产出文件**:
- `aidlc-docs/inception/plans/execution-plan.md` ← 含 Brownfield 特有的组件关系图和更新序列
- 更新 `aidlc-docs/aidlc-state.md`

**审批门控**: 等待用户审批

---

### Stage 6: Application Design [条件执行 → 执行]

与 Greenfield 类似，但会显式标注哪些组件是**迁移自现有模块**，哪些是**全新开发**。

**产出文件**:
- `aidlc-docs/inception/application-design/components.md`
- `aidlc-docs/inception/application-design/component-methods.md`
- `aidlc-docs/inception/application-design/services.md`
- `aidlc-docs/inception/application-design/component-dependency.md`

**Brownfield 差异示例**（components.md 节选）:
```markdown
## 1. Points Engine [MIGRATE from awesomeshop.points]
- **Origin**: 现有 points/ Django app
- **Change Type**: Major refactor
- **Existing Files to Modify**: points/models.py, points/services.py, points/views.py
- **New Capabilities**: P2P 规则引擎, Manager 分配, Milestone 触发, REST API 层
- **Data Migration**: points_account, points_transaction 表迁移

## 4. Recognition Service [NEW]
- **Origin**: 全新开发（V1 无此功能）
- **Change Type**: New component
- **Dependencies**: points-engine, user-profile-service
```

**审批门控**: 等待用户审批

---

### Stage 7: Units Generation [条件执行 → 执行]

与 Greenfield 类似，但 Unit 定义会标注迁移属性。

**Brownfield 差异**:
```markdown
## Unit 1: user-profile-service [MIGRATE from accounts]
- **Migration Source**: awesomeshop/accounts/
- **Migration Priority**: 1 (Critical Path)
- **Data Migration**: auth_user, accounts_profile 表
- **Breaking Changes**: Django auth → Midway SSO
- Stories: US-0.3, US-4.1, US-4.2

## Unit 2: points-engine [MIGRATE from points]
- **Migration Source**: awesomeshop/points/
- **Migration Priority**: 2 (Depends on user-profile)
- **Data Migration**: points_account, points_transaction 表
- **Existing Logic Preserved**: earn(), spend(), balance()
- **New Logic Added**: P2P rules, Manager allocation, Milestone triggers
- Stories: US-0.2, US-1.1, US-1.2, US-1.3, US-1.4

## Unit 3: catalog-service [MIGRATE from catalog]
- **Migration Source**: awesomeshop/catalog/
- **Migration Priority**: 3 (Independent)
- **Data Migration**: catalog_item, category 表
- Stories: US-2.1, US-2.2

## Unit 4: order-service [MIGRATE from orders]
- **Migration Source**: awesomeshop/orders/
- **Migration Priority**: 4 (Depends on points-engine, catalog-service)
- **Data Migration**: order, order_item 表
- Stories: US-0.2, US-2.3, US-2.4, US-2.5

## Unit 5: recognition-service [NEW]
- **Migration Priority**: 5
- Stories: US-3.1, US-3.2, US-3.3

## Unit 6: manager-dashboard [NEW]
- **Migration Priority**: 6
- Stories: US-5.1, US-5.2, US-5.3

## Unit 7: notification-service [NEW]
- **Migration Priority**: 7
- Stories: US-6.1

## Unit 8: frontend-app [NEW, replaces Django templates]
- **Migration Priority**: 8 (Last, depends on all APIs)
- **Replaces**: templates/, static/
- Stories: All user-facing stories
```

**审批门控**: 等待用户审批

---

## PHASE 2: CONSTRUCTION（设计、实现、构建与测试）

### Per-Unit Loop 的 Brownfield 关键差异

以 Unit 2 (points-engine, 迁移自现有 points/) 为例：

---

#### Per-Unit Loop — Unit 2: points-engine [MIGRATE]

##### (a) Functional Design [执行]

**Brownfield 差异**: 需先分析现有 `points/models.py`, `points/services.py` 的业务逻辑，在此基础上扩展。

**产出文件**:
- `aidlc-docs/construction/points-engine/functional-design/business-logic-model.md`
- `aidlc-docs/construction/points-engine/functional-design/business-rules.md`
- `aidlc-docs/construction/points-engine/functional-design/domain-entities.md`

```markdown
# Domain Entities (Brownfield)

## PointsAccount [MIGRATE from points.models.PointsAccount]
- **Existing fields (preserved)**: user_id, balance, lifetime_earned, lifetime_spent
- **New fields**: monthly_p2p_budget, p2p_budget_remaining, budget_period
- **Schema migration required**: ALTER TABLE points_account ADD COLUMN ...

## PointsTransaction [MIGRATE from points.models.PointsTransaction]
- **Existing fields (preserved)**: account_id, type, amount, description, created_at
- **New fields**: source_type(P2P/MANAGER/MILESTONE/AUTO/SPEND), source_reference_id
- **New type values**: EARN_P2P, EARN_MANAGER, EARN_MILESTONE, EARN_AUTO (previously just EARN)

## PointsRule [NEW]
- ruleId, type, conditions, pointsAmount, active

## P2PAllocation [NEW]
- allocationId, senderLogin, monthlyBudget, remaining, period

# Business Rules
## Preserved from V1:
- BR-001 (preserved): 兑换时需余额 >= 商品价格
- BR-002 (preserved): 积分事务 ACID

## New in V2:
- BR-003 (new): P2P 月度额度不可跨月累积
- BR-004 (new): 积分过期策略（获取后 12 个月）
- BR-005 (new): Manager 分配积分需从团队预算扣除
- BR-006 (new): Milestone 积分由系统自动触发
```

**审批门控**

##### (b) NFR Requirements [执行]

与 Greenfield 相同。

**审批门控**

##### (c) NFR Design [执行]

与 Greenfield 相同。

**审批门控**

##### (d) Infrastructure Design [执行]

与 Greenfield 类似，但需考虑数据迁移基础设施：
```markdown
# Infrastructure — points-engine (Brownfield)
- Compute: ECS Fargate
- Database: Aurora PostgreSQL（从现有 PostgreSQL 迁移）
- **Data Migration**: DMS (Database Migration Service) task for points tables
- **Dual-Write Period**: 2 weeks parallel write to old + new DB
- Cache: ElastiCache Redis
- CDK Stack: PointsEngineStack
```

**审批门控**

##### (e) Code Generation [必须执行]

**Part 1 - Planning**: Brownfield 代码生成计划的关键差异

```markdown
# Code Generation Plan — points-engine (Brownfield)

## Step 1: Review Existing Code
- [ ] Read awesomeshop/points/models.py (EXISTING)
- [ ] Read awesomeshop/points/services.py (EXISTING)
- [ ] Read awesomeshop/points/views.py (EXISTING)
- [ ] Identify reusable logic vs. needs-rewrite

## Step 2: Project Structure Setup
- [ ] Create points-engine/ directory (new microservice structure)
- [ ] Spring Boot / NestJS / Django REST Framework (based on tech decision)

## Step 3: Domain Entities (Migrate + Extend)
- [ ] Migrate PointsAccount model → add new fields
- [ ] Migrate PointsTransaction model → extend type enum
- [ ] Create NEW PointsRule model
- [ ] Create NEW P2PAllocation model

## Step 4: Domain Entity Unit Tests
- [ ] Tests for migrated models (verify backward compatibility)
- [ ] Tests for new models

## Step 5: Repository Layer
- [ ] Migrate existing queries → new repository pattern
- [ ] Add new repositories for PointsRule, P2PAllocation

## Step 6: Service Layer (Migrate + Extend)
- [ ] Migrate existing earn()/spend()/balance() logic from services.py
- [ ] Add NEW P2PAllocationService
- [ ] Add NEW PointsRuleEngine
- [ ] Add NEW MilestoneService

## Step 7: Service Unit Tests
- [ ] Regression tests for migrated logic
- [ ] New tests for P2P, Manager, Milestone logic

## Step 8: API Layer (NEW — V1 had no REST API)
- [ ] Create REST controllers (V1 was Django template views)
- [ ] OpenAPI documentation

## Step 9: API Unit Tests
- [ ] Controller tests

## Step 10: Data Migration Scripts
- [ ] Schema migration (ALTER TABLE for new columns)
- [ ] Data migration (transform existing EARN/SPEND → new granular types)
- [ ] Verification queries (row count + balance consistency check)

## Step 11: Configuration & Security
- [ ] Midway token validation (replaces Django auth)
- [ ] Environment-based configuration (replaces hardcoded settings)

## Step 12: CDK Infrastructure
- [ ] PointsEngineStack + DMS migration task

## Step 13: Documentation
- [ ] Migration runbook
- [ ] API specs
```

> **关键 Brownfield 差异**:
> - Step 1: 先 Review 现有代码（Greenfield 无此步骤）
> - Step 3: Migrate + Extend（而非全新创建）
> - Step 6: 迁移已有业务逻辑（而非从零编写）
> - Step 7: 包含 Regression tests（确保迁移不破坏现有功能）
> - Step 10: 数据迁移脚本（Greenfield 无此步骤）
> - Step 13: 迁移运行手册（Greenfield 无此步骤）

**Part 2 - Generation**:
- **修改现有文件**: 可能 in-place 修改 `awesomeshop/points/models.py` 等
- **创建新文件**: `points-engine/src/...`
- **永不创建副本**: 不会出现 `models_new.py` 或 `services_modified.py`

**审批门控**

---

### 对其余 Unit 重复循环

```
Unit 1: user-profile-service [MIGRATE] → Full cycle (特别注意 SSO 迁移)
Unit 3: catalog-service      [MIGRATE] → Full cycle (商品类型扩展)
Unit 4: order-service         [MIGRATE] → Full cycle (异步履约 + 数据迁移)
Unit 5: recognition-service   [NEW]     → Full cycle (同 Greenfield)
Unit 6: manager-dashboard     [NEW]     → Full cycle (同 Greenfield)
Unit 7: notification-service  [NEW]     → Full cycle (同 Greenfield)
Unit 8: frontend-app          [NEW]     → Full cycle (替代 Django templates)
```

**注意**: MIGRATE 类型的 Unit 每个都会额外包含：
- 现有代码 Review 步骤
- 数据迁移脚本
- Regression 测试
- 迁移运行手册

---

### Build & Test [必须执行, 所有 Unit 完成后]

**Brownfield 额外测试类型**:

**产出文件**:
```
aidlc-docs/construction/build-and-test/
├── build-instructions.md
├── unit-test-instructions.md
├── integration-test-instructions.md
├── performance-test-instructions.md
├── security-test-instructions.md
├── e2e-test-instructions.md
├── contract-test-instructions.md
├── data-migration-test-instructions.md    ← Brownfield 特有
├── regression-test-instructions.md        ← Brownfield 特有
├── rollback-test-instructions.md          ← Brownfield 特有
└── build-and-test-summary.md
```

**Brownfield 特有测试内容**:

```markdown
# Data Migration Test Instructions (Brownfield)
## Purpose
验证从旧系统到新系统的数据迁移完整性和一致性。

## Test Scenarios
### Scenario 1: 积分余额一致性
- 对比新旧系统每个用户的积分余额
- 验证：SUM(new) == SUM(old) for all accounts

### Scenario 2: 订单历史完整性
- 对比订单数量和状态分布
- 验证：COUNT(new_orders) == COUNT(old_orders)

### Scenario 3: 商品目录一致性
- 验证所有商品成功迁移
- 验证价格和库存一致

---

# Regression Test Instructions (Brownfield)
## Purpose
确保迁移后的核心功能与 V1 行为一致。

## Test Scenarios
### Scenario 1: 积分扣减
- 在新系统执行兑换操作
- 验证积分扣减逻辑与 V1 一致

### Scenario 2: 订单创建
- 在新系统创建订单
- 验证订单状态流转与 V1 一致

---

# Rollback Test Instructions (Brownfield)
## Purpose
验证回滚机制可靠性。

## Test Scenarios
### Scenario 1: 单服务回滚
- 模拟 points-engine 故障
- 验证可回退到旧系统 points 模块
- 验证数据无丢失

### Scenario 2: 全系统回滚
- 模拟全面回退
- 验证 Nginx 路由切换到旧系统
- 验证 15 分钟内完成
```

**审批门控**: "Build and test instructions complete. Ready to proceed to Operations stage?"

---

## PHASE 3: OPERATIONS [占位]

当前为占位阶段，流程在 Build & Test 完成后结束。

---

## Greenfield vs Brownfield 差异总结

| 维度 | Greenfield | Brownfield |
|------|-----------|------------|
| **Workspace Detection** | → Requirements Analysis | → **Reverse Engineering** |
| **Reverse Engineering** | 跳过 | **执行**（9 个产出文件） |
| **Requirements 问题** | 10 个问题 | **14 个问题**（+4 迁移策略问题） |
| **User Stories** | 纯新功能故事 | **+迁移故事**（Epic 0） |
| **Workflow Planning** | 范围分析 | **+转型范围检测 + 组件关系映射 + 模块更新序列** |
| **Application Design** | 全新组件 | **标注 MIGRATE / NEW** |
| **Units Generation** | 全新 Unit | **标注迁移源 + 迁移优先级 + 依赖更新序列** |
| **Functional Design** | 全新业务逻辑 | **现有逻辑分析 + 保留/扩展标注** |
| **Code Generation Plan** | 全新创建 | **+现有代码 Review + 数据迁移脚本 + Regression 测试 + 迁移手册** |
| **Code Generation** | 创建新文件 | **修改现有文件 in-place + 创建新文件（永不创建副本）** |
| **Build & Test** | 标准测试 | **+数据迁移测试 + 回归测试 + 回滚测试** |
| **审批门控** | ~55 次 | **~56 次**（+1 Reverse Engineering 审批） |

### 额外审批门控统计（Brownfield）

| 阶段 | 审批次数 |
|------|---------|
| Inception: Reverse Engineering | 1 次 ← **Brownfield 新增** |
| Inception: Requirements + Stories + Workflow + AppDesign + Units | 6 次 |
| Construction per Unit | 6 次 |
| Construction total (8 units) | 48 次 |
| Build & Test | 1 次 |
| **总计** | **~56 次用户审批** |

### Brownfield 特有的产出文件

```
aidlc-docs/inception/reverse-engineering/     ← 整个目录为 Brownfield 特有
├── business-overview.md
├── architecture.md
├── code-structure.md                         ← 含现有文件清单（修改候选）
├── api-documentation.md
├── component-inventory.md
├── technology-stack.md
├── dependencies.md
├── code-quality-assessment.md                ← 技术债务评估
└── reverse-engineering-timestamp.md

aidlc-docs/construction/build-and-test/
├── data-migration-test-instructions.md       ← Brownfield 特有
├── regression-test-instructions.md           ← Brownfield 特有
└── rollback-test-instructions.md             ← Brownfield 特有

aidlc-docs/construction/plans/
├── *-code-generation-plan.md                 ← 含数据迁移步骤
```

### 关键 Brownfield 原则

1. **修改而非复制**: 现有文件 in-place 修改，永不创建 `_new` / `_modified` 副本
2. **先 Review 后修改**: Code Generation 必须先读取现有代码再决定修改方案
3. **数据迁移优先**: 每个 MIGRATE Unit 必须包含数据迁移脚本和验证
4. **回滚安全**: 必须有可验证的回滚机制
5. **依赖顺序更新**: Multi-Module Coordination 确保按依赖链顺序执行
6. **双重验证**: Regression 测试确保旧功能不被破坏，新测试验证新功能
