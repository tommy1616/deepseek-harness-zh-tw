# Agent Note: OpenCode Go session affinity

Status: implemented

English | [中文](2026-09-10-opencode-session-affinity.zh.md)

## Problem

OpenCode Go requires a stable `x-opencode-session` header for each conversation. The pi-ai adapter already receives the Harness `GenerateOptions.sessionId`, but its generic request-header merge did not project that id into the OpenCode Go header, so Console Go rejected affected requests with `MissingSessionID`.

## Decision

The `llm-pi-ai` adapter identifies OpenCode routes by a route key beginning with `opencode` or by an endpoint hostname equal to `opencode.ai` or one of its subdomains. When a request carries a session id, the adapter sends that id as `x-opencode-session` through pi-ai's common stream options. A case-insensitive static header with the same name is removed before the per-conversation value is added; Harness attribution headers retain their existing precedence. The common stream call covers the supported pi-ai protocols and auxiliary requests that carry the same session id.

## Alternatives considered

**A static `headers` entry.** Rejected as the product fix: one value would make every conversation share an affinity key and would reduce routing and prompt-cache locality. The dynamic value also needs to survive session switches, resumption, compaction, and retries.

**Adding the header to every provider.** Rejected: `x-opencode-session` is an OpenCode Go requirement, and unrelated gateways should receive only their configured headers and Harness attribution.

**Relying on pi-ai's generic session-affinity compatibility switches.** Rejected for this route: the installed formats do not emit `x-opencode-session`, and the adapter must cover the provider's public gateway requirement directly.

## Consequences

OpenCode Go requests with a Harness session id pass the gateway's required routing check without user configuration. Requests made without a session id cannot be given a stable conversation header and retain their existing behavior. The adapter's focused tests cover dynamic injection, replacement of a stale static value, and isolation of unrelated gateways.
