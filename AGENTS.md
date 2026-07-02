# Codex zc-toolkit 插件入口

这是 `zc-toolkit` Codex 插件的薄入口文件。

它负责保留插件安装后的全局 / 项目级默认规则，并把统一 `zc:*` 语义映射到插件内 skill。详细方法不写在这里，完整内容都在 `plugins/zc-toolkit/skills/<skill>/SKILL.md`。

## 全局规则

- 默认先判断任务属于哪条 workflow，再决定入口
- 不确定入口时，先用 `$start`
- 中文优先，命令名、参数名、文件名、JSON 键和平台产物名保持原样
- 证据先于断言，完成前必须给出实际验证结果
- 不做超出任务边界的顺手修改
- 多 agent 触发以 `agent_opportunity.dispatch_now` 为准；为 `yes` 时必须真实派发可用 Codex custom agents，或说明平台能力不足并降级
- 写入型 agent 必须有文件所有权、loop budget 和 fan-in 验证

## 入口选择

这里控制的是推荐入口顺序，不裁剪 Codex 安装资产。所有匹配 Codex 的 assets 都会生成到插件或项目目录中。

### 默认入口

- `$start`: 统一任务开始入口。用于先评估任务类型、清晰度、阶段与风险，再在 6 条固定 workflow 中选择默认入口；适用于不确定该先分析、实现、调试、审查、补文档还是调查摸底的请求。

### 固定 workflow 入口

- `$product-analysis`: 将模糊需求收敛为可落地的执行方案，先明确价值、范围、验收标准，再决定是否进入完整交付。
- `$sdd-tdd`: 启动完整的 SDD+TDD 开发流程，从需求分析到规格编写、任务拆解、TDD 增量构建和代码审查，全流程门控推进。
- `$debug`: 使用系统化方法诊断和修复 Bug，遵循 Prove-It 模式：先复现，再定位，最后修复。
- `$quality-review`: 对代码变更进行五维度系统化审查（正确性/可读性/架构/安全/性能），输出结构化中文审查报告。
- `$doc`: 生成项目文档或架构决策记录（ADR），确保知识可传递。
- `$onboard`: 系统化地理解陌生代码库，通过目录扫描、入口定位、依赖分析和模式识别快速建立全局认知。
- `$ctx-health`: 管理和刷新对话上下文，防止长会话质量下降，执行上下文健康检查并输出压缩摘要。

### 阶段 / 收尾入口

- `$task-plan`: 将需求或 Spec 拆解为可执行的原子任务列表，并标注依赖、上下文和验证步骤。
- `$spec`: 先澄清需求与假设，再为给定的功能或场景编写结构化技术规格说明。
- `$build`: 按 TDD 的 Red-Green-Refactor 循环进行增量构建，确保每次只完成一个任务且系统始终可编译、可测试。
- `$verify`: 在声明工作完成之前运行验证命令确认实际状态，遵循“证据先于断言”铁律。
- `$plan-review`: 从产品、工程、设计、DevEx 多个角度评审 Spec/Plan，发现单一视角容易遗漏的问题。
- `$idea`: 将模糊的想法或需求细化为具体、可执行的技术方案，通过结构化提问帮你理清思路。
- `$ship`: 发布上线前的系统化检查清单，确保代码已准备好部署到生产环境。

### 专项入口（按需召回）

- `$api`: 设计和审查 API 接口，确保一致性、易用性和向后兼容性。
- `$migrate`: 规划和执行代码迁移或 API 废弃，确保平滑过渡，不破坏现有功能。
- `$perf`: 分析代码性能瓶颈并提供优化方案，从测量开始，用数据驱动优化决策。
- `$qa`: 执行真实浏览器自动化 QA 测试，覆盖用户流程、交互、可访问性。
- `$secure`: 对代码进行安全审计和加固，识别漏洞并提供修复方案。
- `$simplify`: 分析代码并进行简化重构，降低复杂度，提升可读性和可维护性。
- `$ui`: 前端 UI 开发辅助，涵盖组件设计、样式实现、响应式布局和可访问性。

### 上下文、发布和治理入口

- `$context-init`: 初始化 Codex 项目上下文索引。用于在项目根生成薄入口和渐进式披露的 .codex/context 资料，让 AI 能快速理解项目并主动维护上下文。
- `$learn`: 手动触发当前会话的模式提取与学习，分析观察数据，提取可复用的 instincts 并持久化。
- `$retro`: 触发 Sprint 回顾，统计产出数据、总结决策效果、识别瓶颈、输出改进清单。
- `$commit`: 引导规范化 Git 提交，确保原子提交、描述性消息和提交前检查。
- `$ci`: 搭建或优化 CI/CD 管道，配置质量门禁、自动化测试和部署策略。

### 防护入口

- `$guard`: 同时激活 Careful + Freeze 的组合防护，适用于操作生产环境，提供最高级别的安全保护。
- `$careful`: 激活 Careful 模式，AI 在执行任何危险命令前显示风险警告并要求确认。
- `$freeze`: 锁定指定目录或文件，禁止 AI 编辑，保护关键文件不被意外修改。


## 统一命令语义到插件 skill 的映射

- `zc:api` -> `$api`
- `zc:build` -> `$build`
- `zc:careful` -> `$careful`
- `zc:ci` -> `$ci`
- `zc:commit` -> `$commit`
- `zc:context-init` -> `$context-init`
- `zc:ctx-health` -> `$ctx-health`
- `zc:debug` -> `$debug`
- `zc:doc` -> `$doc`
- `zc:freeze` -> `$freeze`
- `zc:guard` -> `$guard`
- `zc:idea` -> `$idea`
- `zc:learn` -> `$learn`
- `zc:migrate` -> `$migrate`
- `zc:onboard` -> `$onboard`
- `zc:perf` -> `$perf`
- `zc:plan-review` -> `$plan-review`
- `zc:product-analysis` -> `$product-analysis`
- `zc:qa` -> `$qa`
- `zc:quality-review` -> `$quality-review`
- `zc:retro` -> `$retro`
- `zc:sdd-tdd` -> `$sdd-tdd`
- `zc:secure` -> `$secure`
- `zc:ship` -> `$ship`
- `zc:simplify` -> `$simplify`
- `zc:spec` -> `$spec`
- `zc:start` -> `$start`
- `zc:task-plan` -> `$task-plan`
- `zc:ui` -> `$ui`
- `zc:verify` -> `$verify`

## 详细内容在哪里

- 插件 skills：`plugins/zc-toolkit/skills/<command-or-skill>/SKILL.md`
- custom agents：`.codex/agents/zc-<agent>.toml`
- Codex agent role 注册：`.codex/config.toml` 的 `[agents.*]` 配置
- 当前入口文件：`AGENTS.md`

## 已安装能力

此安装当前包含：

- 清单来源：`/home/runner/work/zc-ai-coding-toolkit/zc-ai-coding-toolkit/packages/toolkit/src/content`
- 匹配到的资产：77
- command-alias skills：30 个
- skills：38 个
- custom agents：9 个
