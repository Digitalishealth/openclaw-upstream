# openclaw-upstream — Strategic Direction and Sprint Plan

**Date:** 12 September 2026  
**Status:** Mirror-management proposal — keep `main` clean

## Decision

**Treat this repository as a clean upstream mirror/dependency reference, not a Digitalis product repository.** Digitalis-specific runtime code, policy, patches and planning belong in `Digitalishealth/Openclaw`. The mirror exists to provide reproducible upstream source, compatibility testing and a controlled upgrade path.

This document is intentionally proposed on a branch/draft PR. If strict mirror purity is adopted, do not merge it into `main`; retain the policy externally or in the Digitalis OpenClaw repository and delete the strategy branch after the decision is recorded.

## Strategic rules

- `main` should track the authoritative upstream history as closely as the chosen mirror model permits.
- Do not place Digitalis secrets, configuration, proprietary product logic or long-lived patches directly in the mirror.
- Pin the exact upstream commit/tag consumed by Digitalis OpenClaw.
- Every upgrade is compatibility-tested before changing the production pin.
- Security fixes may accelerate the upgrade cadence but still pass the minimum compatibility/security gate.
- Local patches that cannot be upstreamed live as explicit, reviewable patches/adapters in `Digitalishealth/Openclaw`, with a retirement condition.

## Sprint UP-01 — Reproducible Upstream Upgrade Path

**Cadence:** 2 weeks  
**Objective:** make upstream provenance, pinning and Digitalis compatibility explicit and repeatable without contaminating the mirror.

| ID | Work item | Acceptance evidence |
|---|---|---|
| UP01-01 | Identify authoritative upstream and mirror method | Upstream URL/repository, sync command/process and provenance recorded |
| UP01-02 | Reconcile mirror freshness | Current mirror head compared with authoritative upstream; stale delta quantified |
| UP01-03 | Record Digitalis production pin | Exact commit/tag consumed by `Openclaw`; no floating dependency |
| UP01-04 | Build compatibility smoke suite in Digitalis `Openclaw` | Gateway/startup, agent routing, approvals, scheduler, sessions, plugin/extension and security-watch checks |
| UP01-05 | Inventory local compatibility patches | Patch purpose, affected upstream versions, owner and removal condition |
| UP01-06 | Define upgrade gate | Mirror sync → candidate pin → tests/security scan → operator smoke → production pin update |
| UP01-07 | Define rollback | Previous known-good pin and config compatibility retained; rollback drill documented |
| UP01-08 | Define security-update handling | Advisory triage, exposure check, emergency candidate path and evidence requirements |
| UP01-09 | Automate drift/freshness reporting | Upstream/mirror/production-pin divergence visible without auto-upgrading production |
| UP01-10 | Decide mirror purity | If strict mirror: do not merge Digitalis docs/patches to main; if maintained fork: rename/reframe repository accordingly |

### Exit gate

The team can state which upstream revision is mirrored, which revision Digitalis runs, what local patches exist, whether a candidate upgrade is compatible, and how to roll back — without embedding Digitalis runtime policy in the upstream source tree.
