# CyberLab

**集 CTF 知识管理与网络流量分析于一体的安全平台。**

CyberLab 是一个基于 Web 的网络安全学习与分析平台，旨在整合 CTF 题目管理、Writeup 知识沉淀和 PCAP 网络流量分析功能，帮助安全学习者高效整理解题经验，并通过可视化方式查看流量分析结果。

## 项目目标

- 构建结构化的 CTF 题目与解题知识库。
- 实现网络流量文件的上传、解析与结果展示。
- 通过异步任务处理耗时的文件分析操作。
- 实践前后端分离、数据库设计、接口开发、自动化测试与容器化部署。

## 核心功能规划

> 当前项目处于开发规划与初始化阶段，以下功能为计划实现的内容，不代表已经完成。

### 1. CTF 知识管理

- CTF 题目创建、编辑、删除与查询。
- 按题目类别、难度和标签进行分类管理。
- Markdown 格式的 Writeup 编写与展示。
- 题目搜索、分页及相关信息统计。

### 2. 网络流量分析

- PCAP 文件上传与基础校验。
- 计算文件 SHA-256 哈希值。
- 提取数据包数量、协议分布、IP 地址、端口及时间范围等信息。
- 通过图表展示分析结果。

### 3. 异步分析任务

- 创建并提交流量分析任务。
- 查看任务排队、执行、成功和失败状态。
- 保存分析结果，支持历史任务查询。
- 记录分析异常，提供必要的错误信息。

### 4. 用户与权限管理

- 用户注册、登录与身份认证。
- 用户数据隔离与资源所有权校验。
- 限制未经授权的题目、文件和分析任务访问。

### 5. 工程化与部署

- 自动化测试与持续集成。
- Docker Compose 容器化运行。
- 数据库迁移与持久化存储。
- 项目文档、接口文档和演示环境。

## 技术栈

| 模块 | 技术 |
|---|---|
| 前端框架 | React |
| 开发语言 | TypeScript |
| 前端构建 | Vite |
| UI 样式 | Tailwind CSS、shadcn/ui |
| 数据可视化 | Recharts |
| 后端框架 | Python、FastAPI |
| ORM | SQLAlchemy 2.x |
| 数据库 | PostgreSQL |
| 数据库迁移 | Alembic |
| 身份认证 | JWT、密码哈希 |
| 异步任务 | Redis、Celery |
| 流量解析 | Scapy |
| 后端测试 | Pytest、HTTPX |
| 前端测试 | Vitest |
| 容器化 | Docker、Docker Compose |
| 持续集成 | GitHub Actions |

## 系统架构

项目采用前后端分离架构。

```text
Browser
   |
   v
React + TypeScript
   |
   v
FastAPI REST API
   |
   +------ PostgreSQL
   |          |
   |          +-- Users
   |          +-- Challenges
   |          +-- Writeups
   |          +-- Analysis Jobs
   |
   +------ Redis
   |          |
   |          v
   |       Celery Worker
   |          |
   |          v
   |       Scapy Parser
   |
   +------ File Storage
```

前端负责用户交互和数据展示；后端负责业务逻辑、身份认证和权限校验；PostgreSQL 用于保存业务数据；Redis 与 Celery 用于处理异步分析任务；Scapy 用于提取网络流量的基础信息。

## 项目目录

```text
cyberlab/
├── frontend/                 # React 前端
│   ├── src/
│   ├── package.json
│   └── Dockerfile
├── backend/                  # FastAPI 后端
│   ├── app/
│   │   ├── api/              # API 路由
│   │   ├── core/             # 配置、认证等基础模块
│   │   ├── models/           # 数据库模型
│   │   ├── schemas/          # 请求与响应模型
│   │   ├── services/         # 业务逻辑
│   │   └── workers/          # 后台任务
│   ├── tests/
│   ├── alembic/
│   ├── requirements.txt
│   └── Dockerfile
├── docs/                     # 设计文档与截图
├── scripts/                  # 辅助脚本
├── .github/
│   └── workflows/            # CI 工作流
├── compose.yaml
├── .env.example
├── .gitignore
├── LICENSE
└── README.md
```

## 开发计划

项目计划分为四个阶段。

- [ ] **阶段一：基础架构** — 初始化前后端、数据库连接、数据模型与迁移。
- [ ] **阶段二：知识管理** — 实现用户认证、CTF 题目管理、分类标签和 Writeup。
- [ ] **阶段三：流量分析** — 实现文件上传、PCAP 解析、异步任务和结果可视化。
- [ ] **阶段四：工程交付** — 补充自动化测试、CI、Docker 部署与项目文档。

## 本地开发

项目初始化完成后，将在此处补充经过实际验证的本地启动步骤。

计划支持使用 Docker Compose 启动应用及其依赖服务。具体环境变量、数据库迁移命令、初始化方式和启动指令将在相应配置文件完成后更新。

## 测试

项目计划使用以下工具进行质量验证：

- **Pytest**：后端业务逻辑与 API 测试。
- **HTTPX**：HTTP 接口测试。
- **Vitest**：前端组件与业务逻辑测试。
- **GitHub Actions**：自动执行测试与构建检查。

测试结果和覆盖率将在实际运行测试后记录。

## 安全注意事项

- 密码仅以安全哈希形式存储。
- 密钥和敏感配置通过环境变量管理，不提交至 Git 仓库。
- 上传文件需执行类型、大小和存储路径校验。
- 文件名由服务端生成，避免路径穿越等问题。
- 所有私有资源访问均需进行身份与所有权校验。
- 网络流量文件应视为不可信输入，并限制解析资源。
- 本项目定位于合法的学习、研究与授权环境下的网络安全分析。

## 项目状态

当前状态：**开发规划与初始化阶段**

项目功能、自动化测试、性能指标和部署状态将随着实际开发进度持续更新。

## License

本项目计划采用 MIT License，正式发布前将补充对应的 `LICENSE` 文件。

---

CyberLab — Learn, Analyze, and Organize.
