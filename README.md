# claude-code-bmad-foundation

A **lightweight** Claude Code template for solo developers who want to ship code fast. The heavy lifting of "how do features get planned, designed, built, and tested" is handed to [**BMAD-METHOD**](https://github.com/bmad-code-org/BMAD-METHOD); this repo only provides the minimum scaffolding above it.

This is a **base template**, not an application. It has no business code, no tech stack, no app features.

## 1. What this is

- A Karpathy-style [`CLAUDE.md`](./CLAUDE.md) — the upstream behavioral baseline plus a short section telling Claude to use the template's installed tools (Context7, /loop, different-model code review) before training memory.
- Basic project hygiene: [`.gitignore`](./.gitignore), [`.gitattributes`](./.gitattributes), [`.editorconfig`](./.editorconfig), [`LICENSE`](./LICENSE) (MIT).
- A single safety hook: `.claude/hooks/block_dangerous_commands.py` — prevents Claude from running `rm -rf`, reading `.env`, force-pushing, etc.
- One pre-wired MCP server in [`.mcp.json`](./.mcp.json): **Context7** — fetches live, version-specific docs for hundreds of libraries so Claude doesn't hallucinate stale APIs. Free tier, no API key needed.
- **Playwright pre-staged** for frontend end-to-end tests: [`package.json`](./package.json), [`playwright.config.ts`](./playwright.config.ts), and [`e2e/example.spec.ts`](./e2e/example.spec.ts) (`test.skip`'d placeholder). Activate per derived project via `pnpm install && pnpm e2e:install`. If your project is backend-only or CLI-only, delete these three files.
- GitHub PR + Issue templates in [`.github/`](./.github/) — language-agnostic, keeps contributions structured.
- **BMAD-METHOD installed on top** — provides the PM / Architect / Developer / QA workflow and 12+ specialized agents.
- Two short docs in `docs/` explaining how to use the template and how it relates to the heavyweight sibling repo.

**Prerequisites:** [Node 18+](https://nodejs.org/) and [pnpm](https://pnpm.io/installation) (`npm install -g pnpm`) are required to activate Playwright. If you'll never need a frontend, you can skip both and delete the Playwright files (see above).

## 2. What is NOT in here (by design)

- No application-stack manifest (`pyproject.toml`, `requirements.txt`, `go.mod`, `Cargo.toml`, `pom.xml`, etc.) — the only `package.json` is the minimal one that pulls in Playwright as a devDependency.
- No `Dockerfile`, Terraform, Kubernetes, or app-framework configs (vite/next/jest/pytest/etc.) — `playwright.config.ts` is the only framework config and is opt-out (delete it for non-frontend projects).
- No `src/`, `app/`, `server/`, `client/` — no source code skeleton. (`e2e/` is the Playwright test directory, not application code.)
- No CI / security / supply-chain workflows. Those live in the heavyweight sibling repo: [`claude-code-industrial-workbench`](https://github.com/lordrance/claude-code-industrial-workbench).
- No SuperClaude, no wshobson/agents — not installed by default to keep Claude's tool surface focused.
- No homemade SDLC workflow (specs/, .specify/, custom skills, custom subagents) — BMAD owns this layer.

## 3. How to use this template

Short version: see [`docs/usage.md`](./docs/usage.md) for the full walkthrough.

In short:

1. Click **Use this template** on GitHub → create a new repo for your real project.
2. Clone the new repo locally.
3. **Choose your stack inside that project**, not here.
4. Start talking to BMAD's PM agent about what you want to build.
5. Let BMAD drive the PM → Architect → Dev → QA loop.

## 4. Related: when to use the full workbench instead

If you're working on a multi-person project, dealing with regulated data, or already shipping to customers — use the heavyweight sibling: [`claude-code-industrial-workbench`](https://github.com/lordrance/claude-code-industrial-workbench). It adds CODEOWNERS, branch protection, OpenSSF Scorecard, SBOM / container / IaC scanning, AI governance policies, and a Tier 1.5 universal engineering skeleton.

See [`docs/decision-bmad-vs-full-workbench.md`](./docs/decision-bmad-vs-full-workbench.md) for the full trade-off.

## 5. Attribution

This template includes BMAD-METHOD-generated files from [`bmad-code-org/BMAD-METHOD`](https://github.com/bmad-code-org/BMAD-METHOD). BMAD-METHOD is licensed under MIT. BMad-related names and trademarks (BMad™, BMad Method™, BMAD-METHOD™, BMad Core™) belong to BMad Code, LLC. **This repository is not an official BMAD project** and is not affiliated with, sponsored by, or endorsed by BMad Code, LLC.

The full list of third-party components and the license/trademark scope live in [`THIRD_PARTY_NOTICES.md`](./THIRD_PARTY_NOTICES.md).

---

License: [MIT](./LICENSE). Not affiliated with, endorsed by, or created by Anthropic, GitHub, Cursor, or Andrej Karpathy.
