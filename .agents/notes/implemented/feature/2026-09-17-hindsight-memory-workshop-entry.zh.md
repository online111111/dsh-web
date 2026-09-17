# Agent Note: Hindsight 记忆创意工坊条目

Status: implemented

## Problem

创意工坊的记忆分类已经收录本地与项目型记忆插件，但没有展示现有的 `dsh-hindsight-memory` 包。因此，运行共享 Hindsight 服务的用户无法从 DSH 设置与创意工坊流程中发现这套管线级集成，尽管该包已经提供独立的一级设置分区。

## Decision

社区插件索引在 `knowledge / memory` 分类下收录 `dsh-hindsight-memory`，并指向作者仓库与 npm 包。目录文案说明可观察的集成契约：每轮第一次模型调用前召回并注入记忆，轮次结束后异步留存；插件拥有独立设置分区，可配置启用状态、API URL、Bearer API Key、Bank ID 与 Recall Budget。

本仓库继续只做索引，不搬运第三方实现。安装仍使用创意工坊的标准 npm 流程，配置仍由安装后插件自己的 `settings.section` 设置面负责。

## Alternatives considered

- 在 `dsh-web` 内重新实现 Hindsight：否决；已有受维护的 DSH 插件实现管线钩子与设置 UI，重复实现会分裂所有权与安全修复。
- 不安装运行时插件，只在通用 Web 插件分区添加静态 Hindsight 字段：否决；单独设置表单无法提供 recall 或 retain 行为，会形成具有误导性的惰性配置。
- 仅使用通用 Hindsight MCP 端点：否决；MCP 依赖模型主动调用工具，不具备每轮自动前置召回与结束后留存语义。

## Consequences

- 创意工坊用户可以在记忆分类中发现并安装 Hindsight 集成。
- 安装并重启 DSH 后，插件显示独立 Hindsight 设置分区，并通过 DSH settings 服务持久化配置。
- API Key 仍属于第三方插件设置；本仓库不保存用户凭据或部署专用 URL。
- 社区索引、生成的创意工坊清单与测试必须保持同步。
