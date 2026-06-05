---
source_url: https://github.com/earendil-works/pi
ingested: 2026-06-05
sha256: f5d9c7d76176268b71f50af704525162fc5487af06d26f73669f1c22285a5178
---

# Pi Agent Harness Mono Repo

This is the home of the pi agent harness project including our self extensible coding agent.

* **[@earendil-works/pi-coding-agent](packages/coding-agent)**: Interactive coding agent CLI
* **[@earendil-works/pi-agent-core](packages/agent)**: Agent runtime with tool calling and state management
* **[@earendil-works/pi-ai](packages/ai)**: Unified multi-provider LLM API (OpenAI, Anthropic, Google, …)

## Share your OSS coding agent sessions

Public OSS session data helps improve coding agents with real-world tasks, tool use, failures, and fixes instead of toy benchmarks.

## All Packages

| Package | Description |
|---------|-------------|
| **[@earendil-works/pi-ai](packages/ai)** | Unified multi-provider LLM API (OpenAI, Anthropic, Google, etc.) |
| **[@earendil-works/pi-agent-core](packages/agent)** | Agent runtime with tool calling and state management |
| **[@earendil-works/pi-coding-agent](packages/coding-agent)** | Interactive coding agent CLI |
| **[@earendil-works/pi-tui](packages/tui)** | Terminal UI library with differential rendering |

## Permissions & Containerization

Pi does not include a built-in permission system for restricting filesystem, process, network, or credential access. By default, it runs with the permissions of the user and process that launched it.

Containerization patterns:
- **OpenShell**: run the whole pi process in a policy-controlled sandbox.
- **Gondolin extension**: keep pi and provider auth on the host while routing built-in tools and commands into a local Linux micro-VM.
- **Plain Docker**: run the whole pi process in a local container for simple isolation.

## Supply-chain hardening

- Direct external dependencies are pinned to exact versions. Internal workspace packages remain version-ranged.
- `.npmrc` sets `save-exact=true` and `min-release-age=2` to avoid same-day dependency releases during npm resolution.
- `package-lock.json` is the dependency ground truth.
- The published CLI package includes `npm-shrinkwrap.json`, generated from the root lockfile, to pin transitive deps for npm users.
- CI installs with `npm ci --ignore-scripts`, and a scheduled GitHub workflow runs `npm audit --omit=dev` plus `npm audit signatures --omit=dev`.
