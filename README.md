# BUMM — AI-Powered Solana Smart Contract Builder

Bumm turns a plain-English prompt into an audited, deployed Anchor program on Solana. The full pipeline — generation, build, security audit, auto-fix, and deploy — runs end-to-end and is available through a chat UI, CLI, SDK, and MCP server for AI agents.

This repository serves as the main project entry point. The active code is split across component repositories listed below.

---

## Open Source & Public Goods Model

Bumm uses a deliberate three-tier architecture designed for maximum ecosystem impact and long-term sustainability:

| Level | Description                                      | Who pays                              | Status                          |
|-------|--------------------------------------------------|---------------------------------------|---------------------------------|
| **A** | **Open Source** (CLI, SDKs, MCP, tools)         | Nothing (MIT / Apache-2.0)           | Publishing to npm / PyPI       |
| **B** | **Free hosted public good**                      | User pays own LLM tokens + SOL       | Grant-subsidized infrastructure |
| **C** | **Proprietary** (hosted generation + KB moat)   | Credits / subscriptions              | Funds Level A + B long-term    |

### Level A — Open Source (being released)
We are open-sourcing the complete client and tooling layer:
- Python CLI + SDK
- TypeScript SDK (`@bumm/sdk-ts`)
- MCP Server (~30 tools) — allows Claude, Cursor, Cline and other MCP-compatible agents to build directly on Solana
- Agent tools (function calling definitions)
- Claude Code plugin
- **AnchorBench** — public evaluation harness and leaderboard for Anchor program quality
- Static security rule-pack (deterministic Solana checks)
- On-chain attestation program for reproducible builds
- OpenAPI specification + generated clients

### Level B — Free hosted public good
The deterministic parts of the pipeline run on our infrastructure at near-zero marginal cost:
- Isolated Docker build with warm target cache
- Static audit + rules-based auto-fix
- Public `POST /api/v1/validate` (no authentication, no LLM, no credits)
- Public `GET /api/v1/kb/search`
- Non-custodial self-deploy (user signs the transaction and pays SOL themselves)

Users only bring their own LLM key (BYO-key) and pay SOL for deployment. Fixed infrastructure costs (builder, RPC, hosting) are covered by grants / sponsorship so Level B can remain sustainably free.

### Level C — Proprietary (funds the public goods)
Hosted generation using our LLM keys + curated knowledge base, Debate Mode, Pro Mode (multi-file projects), and priority features. This layer generates revenue that subsidizes Levels A and B long-term, ensuring the public goods remain free even after initial grant funding ends.

**Backend access policy**  
The core backend (orchestration, prompts, and curated knowledge base) remains private to protect our variable costs and competitive advantage. However, **read access is granted upon request** to contributors, security researchers, and OSS program reviewers. We have a transparent process for providing access when needed.

---

## What we're building

Bumm is a "prompt-to-program" pipeline for Solana. Describe a contract in natural language and a multi-stage agent handles the rest:

1. **Enrich** — converts the prompt into a structured specification.
2. **Generate** — produces an Anchor 0.32 Rust crate using a curated knowledge base of Solana patterns and common pitfalls.
3. **Build** — compiles in an isolated Cargo runner with a warm-target image (cold compile reduced from ~900s to 176s).
4. **Audit** — runs static checks (clippy + cargo-audit + 14+ categories of Solana-specific rules) in parallel with an LLM auditor backed by a Qdrant vector knowledge base.
5. **Fix** — applies rule-based and LLM-driven fixes, then re-builds and re-audits.
6. **Deploy** — deploys to devnet/testnet/mainnet with on-chain SOL → credits accounting.

Live progress is streamed to the frontend via WebSocket with REST polling fallback.

---

## Component repositories

| Component   | Repository                                      | Stack                                                                 | Status          |
|-------------|-------------------------------------------------|-----------------------------------------------------------------------|-----------------|
| **Frontend**    | [bumm-ai/frontend_v3](https://github.com/bumm-ai/frontend_v3)     | Next.js 15, React 19, TypeScript, Tailwind, shadcn/ui, `@solana/web3.js` | Public (MIT)   |
| **Backend**     | `bumm-ai/backend_v3` (private)                  | Python 3.12, FastAPI, LangGraph, PostgreSQL, Redis, Qdrant, Docker   | Access on request |
| **Clients**     | (in progress)                                   | Python CLI + SDK, TypeScript SDK, MCP Server                         | Publishing soon |

The backend is kept private because it contains production-tuned prompts, the curated Qdrant knowledge base, and audit rules. Read access is available upon request for legitimate purposes (contributors, security reviews, OSS grants).

---

## Why now

Solana smart contract development still has a high barrier to entry. The combination of Anchor, BPF toolchain nuances, and the gap between "it compiles" and "it is actually safe" stops many developers.

Recent advances in LLMs (especially Claude Sonnet with strong reasoning and prompt caching) finally make it practical to close this loop reliably when guided by a high-quality, Solana-specific knowledge base. The window to establish the leading developer tooling pattern on Solana is open right now — before every L1 has similar capabilities.

---

## Engineering highlights

- Full pipeline with **1,754 backend tests** passing
- Warm Cargo target image (build time reduced from ~900s to 176s)
- Parallel static + LLM audit with prompt caching
- Robust deploy system with idempotency, exact binary matching, and automatic cleanup
- Non-custodial self-deploy with on-chain memo attribution
- Public no-token endpoints: `/api/v1/validate` and `/api/v1/kb/search`
- AnchorBench evaluation harness for measuring generation quality

Detailed week-by-week engineering logs are available in the frontend repository under `docs/DEV_LOG_BUMM_WEEK_*.md`.

---

## Tech stack

**Solana** — Anchor 0.32, `solana-program`, SPL Token, wSOL, PDA + CPI patterns  
**Backend** — Python 3.12, FastAPI, LangGraph, PostgreSQL + SQLAlchemy + Alembic, Redis, Qdrant, Docker  
**AI/LLM** — Anthropic Claude (primary) with prompt caching, OpenAI fallback, custom static analyzers, RAG over Qdrant  
**Frontend** — Next.js, React, TypeScript, Tailwind, shadcn/ui, `@solana/web3.js` + wallet-adapter  
**Dev tools** — pytest, ruff, mypy, GitHub Actions, **Claude Code** (heavily used for development)

---

## Live resources

- Website: [bumm.io](https://bumm.io)
- API docs: [api.bumm.io/docs](https://api.bumm.io/docs)
- Public validate endpoint: `POST /api/v1/validate`
- Public KB search: `GET /api/v1/kb/search`

---

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md).

## License

[MIT](./LICENSE)

---

*Building better developer infrastructure for the Solana ecosystem.*
