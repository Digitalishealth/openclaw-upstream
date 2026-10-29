# OpenClaw fork engineering guidance

This is Digitalishealth's fork of `openclaw/openclaw`. Preserve upstream compatibility, scoped engineering contracts and source-trust controls. Maintain project entry-point policy here; `CLAUDE.md` remains a symlink to this file. Detailed upstream procedures live in [AGENT_PROCEDURES.md](AGENT_PROCEDURES.md) and are loaded for the affected work.

## Task scope and authority

Inspect the affected implementation/tests and owning scoped `AGENTS.md`. Read enough owners, callers and sibling paths to establish the invariant and expose uncertainty; expand when evidence requires it. Do not default every task to an exhaustive repository search. Complete the requested scope, investigate routine failures, use bounded delegation when useful and preserve unrelated edits. Do not generate idle work, chase unrelated backlog, or repeatedly review unchanged proof. Stage intended files only.

For dependency-backed behaviour inspect current source/docs/types. Codex protocol/runtime claims require the acting reviewer to inspect the exact sibling `../codex` source and cite checked files/lines; summaries and generated schemas do not substitute. Check existing solutions briefly before designing a custom integration. Runtime provider/model/concurrency settings belong in configuration, not this development contract.

Existing authorisation carries forward for the same action. This file does not grant merge, publishing, version bump, spend, external-message or operational authority. Larger product/security/ownership decisions require owner review. Dependency patches/overrides/vendor changes, protocol version bumps and SQLite schema-version bumps retain explicit owner gates; Carbon pin changes remain owner-only as detailed in the security procedure.

## Safety visible in every task

- Never commit credentials, real phone numbers, sensitive captures or live configuration. Never print secrets or edit `node_modules`.
- Classify source trust before executing proof. Never run untrusted checkout scripts, tests, wrappers, config or package hooks locally. Untrusted proof uses secretless fork CI or the sanitised remote procedure; no credentials, IAM role, tailnet/LAN access, ambient Node preload or hydrated lease may reach it. Source trust is independent of proof size.
- Trusted source may use focused local proof with existing dependencies. Acquire remote heavy-proof resources lazily and stop owned resources before handoff. If remote proof is unavailable, trusted source may fall back locally with the reason stated; untrusted source never does. Do not install/reconcile dependencies merely to verify prose.
- Live tests use a session-owned gateway, isolated state directory and free port. Never stop/restart/kickstart another operator's gateway, bind its live port or edit/migrate live `~/.openclaw` state without explicit per-task operator approval. Test copies of state instead.
- Preserve typed auth/approval/session boundaries, exact source provenance and explicit unavailable/unknown states. SecretRef failures remain isolated to the smallest proven owner; unknown ownership fails closed and must not silently fall back to credentials.
- Keep plugin/channel APIs upgrade-compatible; additive gateway protocol changes first. Schema/help/types/metadata/baselines/docs must agree. Preserve deterministic prompt ordering and transcript bytes where possible.

## Layout and commands

Core: `src/`, `ui/`, `packages/`; plugins: `extensions/`; SDK: `src/plugin-sdk/`; protocol: `packages/gateway-protocol/`; native apps: `apps/`. Owning scoped guides apply to their subtrees. User-facing wording is “plugins”; `extensions/` is an internal path.

Use pinned `package.json#engines` and `packageManager`; no runtime/package-manager swaps without approval. Relevant existing commands:

- Focused trusted local tests: `node scripts/run-vitest.mjs <path-or-filter>`; never bare/watch Vitest.
- Worktree change classification: `node scripts/check-changed.mjs -- <files...>`; do not run a repository classifier locally for untrusted source.
- Build: `pnpm build`; source CLI: `pnpm dev` (follow proof routing before heavy commands).
- Normal-checkout lanes: `pnpm check:changed`, `pnpm changed:lanes --json`; SDK surface: `pnpm plugin-sdk:surface:check`.
- Type checks use the existing `tsgo` lanes; formatting uses oxfmt, not Prettier. Use existing binaries/wrappers rather than triggering package reconciliation in linked worktrees.
- Docs-only proof: `git diff --check` and relevant docs sanity. Broader checks/builds activate when scripts, config, generated/package or runtime behaviour changes.

## Read procedures when triggered

| Work | Required sections in AGENT_PROCEDURES.md |
| --- | --- |
| New capability/dependency/API or changed public surface | [Task preflight](AGENT_PROCEDURES.md#task-preflight), plus owning scoped guide |
| Architecture, database, protocol or config | [Architecture](AGENT_PROCEDURES.md#architecture) and relevant scoped contracts |
| Code or test changes | [Code](AGENT_PROCEDURES.md#code), [Tests](AGENT_PROCEDURES.md#tests), [Commands](AGENT_PROCEDURES.md#commands), [Validation](AGENT_PROCEDURES.md#validation) |
| Untrusted or remote/heavy proof | [Commands](AGENT_PROCEDURES.md#commands) and [Validation](AGENT_PROCEDURES.md#validation) before execution |
| PR review/landing | [ClawSweeper review policy](AGENT_PROCEDURES.md#clawsweeper-review-policy), [GitHub / PRs](AGENT_PROCEDURES.md#github--prs), [Git](AGENT_PROCEDURES.md#git) |
| Docs/changelog | [Docs](AGENT_PROCEDURES.md#docs), [Docs / Changelog](AGENT_PROCEDURES.md#docs--changelog), and `docs/AGENTS.md` |
| Security, package or release | [Security / Release](AGENT_PROCEDURES.md#security--release), [Validation](AGENT_PROCEDURES.md#validation) |
| Native devices or gateway operations | [Platform / Ops](AGENT_PROCEDURES.md#platform--ops), [Tests](AGENT_PROCEDURES.md#tests) |

Retain required independent reviews, exact-candidate proof and operator release gates. Distinguish implemented/verified (reviewable diff and relevant checks), ready for release (candidate-specific gates and remaining approval identified), and released (actual artifact/deployment and relevant live checks). Report changed files, exact checks, omissions, blockers and residual risk; fewer startup bytes alone do not establish reduced usage.
