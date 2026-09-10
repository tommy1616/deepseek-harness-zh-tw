# Agent Note: OpenCode Go 会话亲和性

Status: implemented

[English](2026-09-10-opencode-session-affinity.md) | 中文

## 问题

OpenCode Go 要求每个对话携带稳定的 `x-opencode-session` 标头。pi-ai 适配器已经收到 Harness 的 `GenerateOptions.sessionId`，但原本的通用请求标头合并没有把该 ID 投影到 OpenCode Go 标头，因此 Console Go 会以 `MissingSessionID` 拒绝受影响的请求。

## 决定

`llm-pi-ai` 适配器通过以 `opencode` 开头的路由键，或主机名等于 `opencode.ai` 及其子域名的端点识别 OpenCode 路由。请求携带会话 ID 时，适配器通过 pi-ai 的通用流选项把该 ID 作为 `x-opencode-session` 发送。加入每个对话的值之前，会移除名称不区分大小写且同名的静态标头；Harness 归属标头仍保持原有优先级。通用流式调用覆盖支持的 pi-ai 协议，以及携带同一会话 ID 的辅助请求。

## 考虑过的替代方案

**静态 `headers` 项。** 不作为产品修复：单一值会让所有对话共享一个亲和性键，并降低路由与提示词缓存的局部性。动态值还需要在切换、恢复、压缩与重试时跟随对话。

**对每个提供方都加入该标头。** 不采用：`x-opencode-session` 是 OpenCode Go 的要求，无关网关应只收到自身配置的标头与 Harness 归属标头。

**依赖 pi-ai 的通用会话亲和兼容开关。** 不采用该路由：已安装版本的格式不会发送 `x-opencode-session`，适配器必须直接覆盖提供方的公开网关要求。

## 影响

带有 Harness 会话 ID 的 OpenCode Go 请求无需用户配置即可通过网关要求的路由检查。没有会话 ID 的请求无法生成稳定的对话标头，因此保持原有行为。适配器的聚焦测试覆盖动态注入、替换过期静态值，以及与无关网关的隔离。
