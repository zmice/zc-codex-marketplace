# Codex zc-toolkit 插件入口

这是 `zc-toolkit` Codex 插件的薄入口文件。

它负责保留插件安装后的全局 / 项目级默认规则，并指向插件内 skill。详细方法不写在这里，完整内容都在 `plugins/zc-toolkit/skills/<skill>/SKILL.md`。

## 全局规则

- 默认先判断任务属于哪条 workflow，再决定入口
- 不确定入口时，先用 `$zc-toolkit:start`
- 中文优先，命令名、参数名、文件名、JSON 键和平台产物名保持原样
- 证据先于断言，完成前必须给出实际验证结果
- 不做超出任务边界的顺手修改
- 多 agent 触发以 `agent_opportunity.dispatch_now` 为准；为 `yes` 时必须真实派发插件内可用 agent，或说明平台能力不足并降级
- 写入型 agent 必须有文件所有权、loop budget 和 fan-in 验证

## 入口选择

这里控制的是推荐入口顺序，不裁剪 Codex 安装资产。所有匹配 Codex 的 assets 都会生成到插件或项目目录中。

### 默认入口

- `$zc-toolkit:start`: 统一任务开始入口。用于先评估任务类型、清晰度、阶段与风险，再在 6 条固定 workflow 中选择默认入口；适用于不确定该先分析、实现、调试、审查、补文档还是调查摸底的请求。

### 固定 workflow 入口

- `$zc-toolkit:product-analysis`: 将模糊需求收敛为可落地的执行方案，先明确价值、范围、验收标准，再决定是否进入完整交付。
- `$zc-toolkit:sdd-tdd`: 启动完整的 SDD+TDD 开发流程，从需求分析到规格编写、任务拆解、TDD 增量构建和代码审查，全流程门控推进。
- `$zc-toolkit:debug`: 使用系统化方法诊断和修复 Bug，遵循 Prove-It 模式：先复现，再定位，最后修复。
- `$zc-toolkit:quality-review`: 对代码变更进行五维度系统化审查（正确性/可读性/架构/安全/性能），输出结构化中文审查报告。
- `$zc-toolkit:doc`: 生成项目文档或架构决策记录（ADR），确保知识可传递。
- `$zc-toolkit:onboard`: 系统化地理解陌生代码库，通过目录扫描、入口定位、依赖分析和模式识别快速建立全局认知。
- `$zc-toolkit:ctx-health`: 管理和刷新对话上下文，防止长会话质量下降，执行上下文健康检查并输出压缩摘要。

### 专项入口（按需召回）

阶段、专项、治理与防护能力按任务需要读取下方 skills 目录中的对应 SKILL.md；入口摘要不重复展开全部描述。已授权范围内持续执行，遇到授权缺失或关键决策时再暂停。


## Codex 调用方式

- Codex 中通过插件 namespace 调用 skill，例如 `$zc-toolkit:start`、`$zc-toolkit:context-init`、`$zc-toolkit:quality-review`
- 每个 command 以同名 skill 提供；不再分发旧 slash command 与 source-command-* 迁移入口
- 如果旧文档或跨平台说明里出现 `zc:*`，它是稳定兼容语义名
- 常见兼容示例：
- `zc:api` -> `$zc-toolkit:api`
- `zc:build` -> `$zc-toolkit:build`
- `zc:careful` -> `$zc-toolkit:careful`
- `zc:ci` -> `$zc-toolkit:ci`
- `zc:commit` -> `$zc-toolkit:commit`

## 详细内容在哪里

- 插件 skills：`plugins/zc-toolkit/skills/<command-or-skill>/SKILL.md`
- 插件 agents：`plugins/zc-toolkit/agents/<agent>.md`
- 当前入口文件：`AGENTS.md`

## 已安装能力

此安装当前包含：

- 清单来源：`/home/runner/work/zc-ai-coding-toolkit/zc-ai-coding-toolkit/packages/toolkit/src/content`
- 匹配到的资产：80
- command-alias skills：30 个
- skills：41 个
- plugin agents：9 个
