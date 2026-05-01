# BUMM — AI-Powered Solana Smart Contract Builder

> **Colosseum hackathon submission.** Bumm turns a plain-English prompt into an audited, deployed Anchor program on Solana — generation, build, security audit, auto-fix, and deploy run end-to-end behind a chat UI.

This repository is the umbrella entry point. The active code lives in two public component repositories linked below.

---

## What we're building

Bumm is a "prompt-to-program" pipeline for Solana. You describe a contract in chat — *"escrow with 7-day timelock"*, *"staking with linear rewards"*, *"AMM with a wSOL leg"* — and a multi-stage agent does the rest:

1. **Enrich** — turns the loose prompt into a structured spec.
2. **Generate** — produces an Anchor 0.32 crate (Rust) using a curated knowledge base of Solana patterns and pitfalls.
3. **Build** — compiles in an isolated cargo runner with a warm-target image (cold compile **900s → 176s**).
4. **Audit** — runs static checks (clippy + cargo-audit + 14 categories of Solana-specific regex rules: `UncheckedAccount` safety, native-SOL/SPL mixing, vault rent-exemption, dead accounts, phantom fields, …) **in parallel** with an LLM auditor backed by a 117-vector Qdrant knowledge base.
5. **Fix** — applies LLM-driven and rule-driven fixes, re-builds, re-audits.
6. **Deploy** — uploads to devnet/testnet/mainnet via `solana program deploy`, with on-chain SOL → credits accounting.

Live progress streams to the frontend via WebSocket (with REST polling fallback) so the user watches every stage.

---

## Component repositories

| Component | Repo | Stack |
|-----------|------|-------|
| **Frontend** | [`bumm-ai/frontend_v3`](https://github.com/bumm-ai/frontend_v3) | Next.js 14, React 18, TypeScript, Tailwind, `@solana/web3.js`, wallet-adapter |
| **Backend** | [`bumm-ai/backend_v3`](https://github.com/bumm-ai/backend_v3) *(private — access shared with judges)* | Python 3.12, FastAPI, LangGraph, PostgreSQL, Redis, Qdrant, Docker |

> The backend repository is **private** because it ships with prod-tuned prompts, the curated Qdrant knowledge base, and audit-rule sources we don't want scraped by competitors. Per Colosseum's submission rules, access has been granted to **hackathon@colosseum.org** so judges can review the full source. If you're a judge and can't see the repo, ping that email — the invite is already sent.

---

## Why now

Solana's developer funnel is brutal: Anchor's learning curve, BPF toolchain quirks, and the gap between *"compiles"* and *"actually safe"* filter out most builders before they ship. We've watched real contracts burn weeks on `UncheckedAccount` footguns, seed-binding lifetime errors, and AMM designs that mix native lamports with SPL CPIs — bugs that are mechanical to detect once you know the pattern.

The pieces finally align: Claude Sonnet 4.5 / GPT-5 can generate non-trivial Rust when guided by a structured KB; prompt caching makes multi-turn agents economical; and Solana's tooling (Anchor 0.32, surfpool, `solana-program-test`) is mature enough to close the loop end-to-end. Eighteen months ago the LLMs hallucinated lifetimes; six months from now every L1 will have this. The window to set the dev-tooling pattern on Solana is open right now.

---

## Highlights of recent engineering work

A condensed view of the last two phases of pipeline hardening (full week-by-week dev logs ship in the frontend repo under `docs/DEV_LOG_BUMM_WEEK_*.md`):

- **Phase F closeout** (F0–F6) — pipeline hardening complete, 673 backend tests green.
- **Phase G STEPs 1–8** —
  - warm cargo target image (cold 900s → **176s**),
  - per-stage durations persisted (Alembic 0009),
  - `audit_static` parallelised via `asyncio.gather`,
  - Anthropic prompt caching wrapper (`cache_control: ephemeral`),
  - host target pre-warm (cycle-2 audit_static **95s → 3.4s**).
- **Audit depth** — `generate.md` rules 17/20/21/22 (`/// CHECK:` doc, seed binding, `checked_pow`, wSOL pattern); `audit.md` categories 13/14 (native-SOL/SPL mixing CRITICAL, vault rent-exemption HIGH); `_check_native_sol_token_mixing` regex detector; KB re-seeded to **117 vectors**.
- **Deploy correctness** — discovered and fixed a CRITICAL bug where `find_so` was selecting the warm-seed Hello-World binary over the user crate, pinning every deploy to a single shared program ID. Resolver now requires an exact `bumm_<uid>.so` match; warm-seed leftovers are scrubbed from `target/deploy/` after each seed; Alembic migration 0010 backfilled affected contracts with `requires_redeploy=true` and a UI re-deploy banner.
- **Reliability** — deploy idempotency (`is_step_in_flight` + 409); WS+REST polling fallback in the frontend `useContractStream` hook; `builds_gc` apscheduler tick (5 min, 7-day retention).

---

## Tech stack

**Solana** — Anchor 0.32.1, solana-cli, BPFLoaderUpgradeable, surfpool (local validator), SPL Token, wSOL, PDA + CPI patterns.

**Backend** — Python 3.12 · FastAPI · LangGraph (state-machine pipeline) · PostgreSQL 16 + SQLAlchemy 2.0 async + Alembic · Redis (pub/sub) · Qdrant (vector KB) · Docker · isolated cargo build runner with warm-target seeding · apscheduler.

**AI / LLM** — Anthropic Claude (Sonnet 4.5) primary generator + auditor with prompt caching · OpenAI GPT-5 fallback · custom static analyzers (regex + AST) for Solana-specific anti-patterns · RAG over Qdrant for audit rules and generation pitfalls.

**Frontend** — Next.js 14 · React 18 · TypeScript · Tailwind · shadcn/ui · `@solana/web3.js` + wallet-adapter (Phantom, Solflare) · custom WebSocket hub with REST polling fallback · Sign-in-with-Solana (nonce → ed25519 → JWT).

**Auth & payments** — wallet-based JWT (access + refresh) · on-chain SOL → credits ledger, idempotent by tx signature.

**Dev tooling** — pytest + pytest-asyncio (673 backend tests) · Playwright (frontend) · ruff · mypy · eslint · GitHub Actions CI · **Claude Code** (used heavily for pipeline iteration, migrations, audit-rule authoring).

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

[MIT](./LICENSE).

---

*Built for the Solana ecosystem.*
