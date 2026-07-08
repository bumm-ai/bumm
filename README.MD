## Open Source & Public Goods Model

Bumm follows a deliberate three-tier architecture designed for long-term sustainability and maximum ecosystem impact on Solana:

| Level | What it is                              | Who pays                          | License / Status          |
|-------|-----------------------------------------|-----------------------------------|---------------------------|
| **A** | Open Source (CLI, SDKs, MCP, tools)    | Nothing (MIT / Apache-2.0)       | Publishing to npm/PyPI   |
| **B** | Free hosted public good                 | User pays own LLM tokens + SOL   | Grant-subsidized infra   |
| **C** | Proprietary (hosted generation + KB)    | Credits / subscriptions          | Closed (funds A + B)     |

### Level A — Open Source (going public)
We are open-sourcing the entire client and tooling layer:
- Python CLI + SDK (`bumm`)
- TypeScript SDK (`@bumm/sdk-ts`)
- MCP Server (~30 tools) — connects Claude / Cursor / Cline / any MCP agent directly to Solana contract building
- Agent tools (function calling for OpenAI / Anthropic / Gemini)
- Claude Code plugin
- **AnchorBench** — public eval harness and leaderboard for Anchor program quality
- Static security rule-pack (deterministic Solana checks on top of clippy + cargo-audit)
- On-chain attestation program (reproducible builds)
- OpenAPI spec + generated clients

### Level B — Free hosted public good (core of this grant application)
Deterministic parts of the pipeline run on our infrastructure **at $0 marginal cost** to the user:
- Isolated Docker build (warm cache)
- Static audit + rules-based auto-fix
- Public `POST /api/v1/validate` (no auth, no LLM, no credits)
- Public `GET /api/v1/kb/search`
- Non-custodial self-deploy (user signs & pays SOL themselves, ownership transfers immediately)

**User pays only**: their own LLM tokens (BYO-key) + SOL for deployment.  
**Grant covers**: fixed infrastructure (builder CPU, RPC, hosting). This makes Level B sustainably free.

### Level C — Proprietary (funds the public goods)
Hosted generation on our LLM keys + curated knowledge base (the real moat), Debate Mode, Pro Mode (multi-file), priority queues. Level C subsidizes Levels A + B long-term, so the public goods remain free even after the grant ends.

**Access policy**  
Frontend and client repositories will be fully public under MIT/Apache-2.0.  
Backend (orchestration + curated KB) remains private to protect our variable costs and moat, but **read access is granted on request** to contributors, security researchers, and OSS program reviewers (including this application). We have already used this process successfully with Colosseum hackathon judges.

On-chain attribution: every self-deploy through Bumm emits a verifiable `bumm:v1` memo on-chain (Dune dashboard available). This gives measurable, on-chain proof of ecosystem usage.
