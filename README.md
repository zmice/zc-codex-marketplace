# zc Codex Marketplace

[![source repo](https://img.shields.io/badge/source-zc--ai--coding--toolkit-24292f)](https://github.com/zmice/zc-ai-coding-toolkit)
[![npm version](https://img.shields.io/npm/v/@zmice/zc)](https://www.npmjs.com/package/@zmice/zc)
[![license](https://img.shields.io/github/license/zmice/zc-ai-coding-toolkit)](LICENSE)
[![codex marketplace](https://img.shields.io/badge/Codex-marketplace-111827)](https://developers.openai.com/codex/plugins/build)

这是由 [zc AI Coding Toolkit](https://github.com/zmice/zc-ai-coding-toolkit) 自动导出的 Codex marketplace 仓库。

这个仓库面向 Codex 官方插件分发模型，提供：

- `.agents/plugins/marketplace.json`
- `plugins/zc-toolkit/.codex-plugin/plugin.json`
- `plugins/zc-toolkit/skills/`
- `AGENTS.md` 薄入口
- `.codex/agents/` 和 `.codex/config.toml` 的 zc-managed custom agent 配置

## 安装 marketplace

```bash
codex plugin marketplace add zmice/zc-codex-marketplace
```

注册 marketplace 后，打开 Codex 的 **Plugins** 页面，选择 `zc-toolkit` 安装或启用插件。

## 更新

```bash
codex plugin marketplace upgrade zc-toolkit
```

更新后重新打开 Codex 或开始新线程，让新的 skills 列表进入会话。

## 常用入口

安装插件后，优先从这些入口开始：

- `$start`
- `$product-analysis`
- `$sdd-tdd`
- `$debug`
- `$quality-review`
- `$context-init`

也可以在 Codex 中使用 `@` 显式选择 `zc-toolkit` 或其中的 skill。

## 目录说明

- `.agents/plugins/marketplace.json`
  - Codex marketplace 清单，插件路径指向 `./plugins/zc-toolkit`
- `plugins/zc-toolkit/`
  - Codex plugin 根目录，包含 `.codex-plugin/plugin.json` 和 skills
- `AGENTS.md`
  - repo-local 薄入口，保留 zc 规则、命令语义映射和能力索引
- `.codex/agents/`
  - zc-managed custom agent role 配置，供项目级 Codex 配置使用

## 维护方式

这个仓库不是手工维护。

- 主仓库：<https://github.com/zmice/zc-ai-coding-toolkit>
- npm CLI：<https://www.npmjs.com/package/@zmice/zc>
- 同步方式：主仓库 GitHub Actions 自动导出并同步

如果你希望调整内容，请回到主仓库修改 `packages/toolkit/src/content/`，再由同步流程重新发布。

## 反馈与问题

- Issues：<https://github.com/zmice/zc-ai-coding-toolkit/issues>
- CLI README：<https://github.com/zmice/zc-ai-coding-toolkit/blob/main/apps/cli/README.md>
- 使用说明：<https://github.com/zmice/zc-ai-coding-toolkit/blob/main/docs/usage-guide.md>
- Codex 插件构建说明：<https://developers.openai.com/codex/plugins/build>
