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

## 推荐开始方式

- 不确定从哪开始：`$start`
- 需求还模糊：`$product-analysis`
- 完整交付主流程：`$sdd-tdd`
- 修 bug：`$debug`
- 代码审查和反馈收敛：`$quality-review`
- 文档或发布说明：`$doc`
- 陌生项目先摸底：`$onboard` 或 `$ctx-health`

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
