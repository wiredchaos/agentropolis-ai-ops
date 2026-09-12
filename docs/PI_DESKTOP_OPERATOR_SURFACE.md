# PI-Desktop Operator Surface

Status: optional adapter profile
Owner: AGENTROPOLIS AI-OPS
Role: desktop operator surface, not runtime authority

## Decision

PI-Desktop is treated as an optional visual engineering workstation for local projects, model switching, session visibility, diffs, shell/file workflows, MCP, skills, plugins, and local-model endpoints.

It is **not** an AGENTROPOLIS control plane, policy authority, audit authority, or autonomous runtime brain.

AGENTROPOLIS remains above the workstation:

```text
Human / Agent
  -> Identity
  -> Mandate
  -> ATG risk classification
  -> Policy
  -> Execution Envelope
  -> Operator Surface (PI-Desktop, Hermes Desktop, Codex, Claude Code, Devin, etc.)
  -> filesystem / shell / MCP / preview / model endpoint
  -> Receipt
  -> BUZZ event
  -> Audit Ledger
  -> Drift / entropy telemetry
```

## Why this adapter exists

PI-Desktop overlaps with several AGENTROPOLIS primitives without replacing them:

| PI-Desktop capability | AGENTROPOLIS owner |
| --- | --- |
| Project/session workspace | AI-OPS / Continuity Plane |
| Local session import | Context Capsule adapters |
| Model switching | Model Router |
| MCP access | MCP policy + registry |
| Skills/plugins | Skill Registry + install assurance |
| File/shell actions | Execution Envelope |
| Diffs / approval UI | Human governance checkpoint |
| Local persistence | Sovereign/local-first infrastructure |
| Subagents | Collective within an Execution Envelope |

## Continuity mapping

Session imports from coding workspaces are normalized into a Context Capsule before they cross an AGENTROPOLIS boundary.

Required capsule fields:

- `capsule_id`
- `source_surface`
- `source_session_id`
- `actor_identity`
- `repo`
- `objective`
- `decisions`
- `files_touched`
- `commands_observed`
- `open_blockers`
- `models_used`
- `tools_used`
- `checkpoint_at`
- `receipt_refs`

The adapter must preserve provenance. Imported state is context, not authorization.

## Execution rules

1. Every mutating action must be attributable to an identity and mandate.
2. File writes, shell execution, network actions, MCP calls, and credential use must remain inside the active Execution Envelope.
3. Changing models does not reset risk tier, permissions, or mandate.
4. Subagents inherit the parent envelope with equal or narrower permissions.
5. Hosted-model use is never described as fully local. Required context may leave the device for provider inference.
6. Local Ollama/LM Studio-style endpoints are treated as compute endpoints, not trusted policy authorities.
7. Plugins and skills are capabilities, not trust grants. Installation and activation are separately governed.
8. Session import never imports authority, secrets, or stale approvals.

## Local-first boundary

`local-first` means workspace/session data may be stored locally. It does **not** imply that prompts, files, tool results, or context remain local when a hosted model is selected.

AI-OPS should surface the active data path:

```text
LOCAL WORKSPACE -> LOCAL MODEL
LOCAL WORKSPACE -> HOSTED MODEL
LOCAL WORKSPACE -> MCP SERVER
LOCAL WORKSPACE -> REMOTE TOOL
```

The active path must be visible in telemetry and receipts.

## Telemetry contract

AI-OPS should collect at minimum:

- operator surface and version
- model/provider/endpoint class
- context size and checkpoint count
- session age
- token/cost data when available
- filesystem operations count
- shell command count
- MCP/tool calls
- plugin/skill identifiers
- subagent count
- approval events
- denied actions
- recovery events
- receipt IDs
- envelope ID and risk tier

Telemetry is evidence. It does not grant execution authority.

## Adapter strategy

Default strategy: **adapter/plugin/MCP integration, not a hard fork**.

Recommended package boundary:

```text
@agentropolis/pi-adapter
  continuity/
  execution-envelope/
  receipts/
  buzz/
  atg/
  aegis/
  skill-registry/
  model-router/
  mcp-policy/
```

This keeps AGENTROPOLIS governance independent of any single desktop client and allows the same contracts to apply to future operator surfaces.

## Failure posture

If the adapter cannot resolve identity, mandate, risk tier, or Execution Envelope, the safe state is read-only observation. Mutating tools must fail closed.

If receipt emission fails after a mutating action, the session is marked `audit_degraded` and further consequential actions require re-authorization.

## Non-goals

This integration does not:

- replace Hermes
- replace NemoClaw/Nemotron
- turn PI-Desktop into Mission Control
- bypass branch protection or repository policy
- trust imported sessions by default
- let MCP servers self-authorize
- let plugins widen their own permissions

## Canonical positioning

Hermes is the persistent agent/operator society surface.
PI-Desktop is an optional visual engineering workstation.
NemoClaw/Nemotron are sovereign runtime paths.
Codex, Claude Code, Devin and similar tools are specialist workers.
AGENTROPOLIS governs identity, mandate, policy, execution, receipts, audit and continuity across all of them.
