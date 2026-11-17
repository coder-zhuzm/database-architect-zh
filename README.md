# Database Architect 中文版

[原始项目](https://github.com/davila7/claude-code-templates) · [原始 Agent](https://github.com/davila7/claude-code-templates/blob/main/cli-tool/components/agents/database/database-architect.md) · [组件介绍](https://www.aitmpl.com/component/agents/database/database-architect)

这是 Claude Code 的 **Database Architect（数据库架构专家）** Subagent 中文化版本，包含完整的数据库设计、迁移、安全验证和审查规范。以 **PostgreSQL 16+** 为默认环境；额外补充了 Java / MyBatis / Flyway / Liquibase 场景下的项目检查提示。

> 本仓库是社区中文翻译与少量适配，**不是官方中文版本**，也不代表原作者认可后续修改。

## 能做什么

- **Schema 设计**：主键、数据类型、非空约束、唯一约束、外键和索引。
- **多租户隔离**：复合外键、RLS、最小权限与连接池下的租户上下文。
- **线上安全迁移**：锁超时、并发建索引、分批回填、Expand/Contract、回滚策略。
- **数据库选型**：优先 PostgreSQL，在需求明确时才增加其他存储。
- **迁移验证**：在独立临时数据库实际执行 SQL，并核实 RLS 是否真正阻断跨租户访问。
- **上线前审查**：优先查数据泄漏与数据丢失，其次查完整性、类型、锁和性能。

## 安装

将仓库中的 [`.claude/agents/database-architect.md`](.claude/agents/database-architect.md) 复制到目标项目的同名路径：

```bash
mkdir -p .claude/agents
cp /path/to/database-architect-zh/.claude/agents/database-architect.md .claude/agents/database-architect.md
```

如果希望作为个人全局 Agent 使用，可以放到 `~/.claude/agents/database-architect.md`。

**不要**执行上游 `npx claude-code-templates@latest --agent database/database-architect` 来安装本翻译版；该命令安装的是上游英文版。

## 使用示例

在 Claude Code 中提出任务：

```text
请使用 database-architect 帮我设计多租户任务管理系统的数据表，
包含租户隔离、外键约束和首个 PostgreSQL Migration，
并在隔离数据库验证实际执行结果。
```

```text
请使用 database-architect 审查本次订单表结构变更：
现有表约 5000 万行，服务滚动部署，要求不停机迁移。
请给出 Expand/Contract 发布步骤、锁风险、回滚方案和验证结果。
```

```text
请使用 database-architect 检查本项目 MyBatis Mapper 和数据库迁移文件，
注意现有数据库类型、实际 DDL 和公司规范，不要直接把 PostgreSQL 语法用于 MySQL。
```

## 文件结构

```text
database-architect-zh/
├── README.md
├── LICENSE
└── .claude/
    └── agents/
        └── database-architect.md
```

## 使用边界

1. 这是 **Agent 提示词文件**，不是独立运行的数据库服务，也不自带数据库连接或测试环境。
2. 默认规则为 PostgreSQL 16+；使用 MySQL 等引擎时必须核实其 DDL、事务、锁与权限差异。
3. 原版要求严格的测试闭环，但若环境无法执行，必须明确列出未验证项；不能宣称已测试。
4. 不允许对生产或共享数据库做测试；生产变更应经过人工审核与既有发布流程。
5. `postgres-pro`、`backend-developer` 等移交目标是其他可选 Agent，并非本仓库附带组件。

## 译者调整

- 将 Agent 描述、触发示例和主体操作规范翻译为中文。
- 保留 SQL、PostgreSQL 特有术语以及明确的安全限制，尽量不改变原意。
- 在「工作方式」增加 Java 常见的 MyBatis Mapper/XML、JPA、Flyway、Liquibase 等检查对象。
- 明确区分上游英文版安装命令与本仓库手工安装方式。

## 来源与版权

- **原作者 / 版权所有者**：Daniel (San) Ávila
- **原始仓库**：[davila7/claude-code-templates](https://github.com/davila7/claude-code-templates)
- **原始文件**：[database-architect.md](https://github.com/davila7/claude-code-templates/blob/main/cli-tool/components/agents/database/database-architect.md)
- **原始组件页**：[Database Architect - AI Templates](https://www.aitmpl.com/component/agents/database/database-architect)
- **原始许可证**：MIT License（见本仓库 `LICENSE`）

本翻译基于上述开源文件。保留原始版权及 MIT 许可声明，中文翻译和补充说明亦在 MIT 条款下提供。若用于商业或二次分发，请继续保留原作者版权及许可证。
