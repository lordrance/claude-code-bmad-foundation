# claude-code-bmad-foundation

A **lightweight** Claude Code template for solo developers who want to ship code fast. The heavy lifting of "how do features get planned, designed, built, and tested" is handed to [**BMAD-METHOD**](https://github.com/bmad-code-org/BMAD-METHOD); this repo only provides the minimum scaffolding above it.

This is a **base template**, not an application. It has no business code, no tech stack, no app features.

## 1. What this is

- A Karpathy-style [`CLAUDE.md`](./CLAUDE.md) — the same 60-line behavioral baseline used in the upstream project, plus one beginner-friendly sentence.
- Basic project hygiene: [`.gitignore`](./.gitignore), [`.gitattributes`](./.gitattributes), [`.editorconfig`](./.editorconfig), [`LICENSE`](./LICENSE) (MIT).
- A single safety hook: `.claude/hooks/block_dangerous_commands.py` — prevents Claude from running `rm -rf`, reading `.env`, force-pushing, etc.
- **BMAD-METHOD installed on top** — provides the PM / Architect / Developer / QA workflow and 12+ specialized agents.
- Two short docs in `docs/` explaining how to use the template and how it relates to the heavyweight sibling repo.

## 2. What is NOT in here (by design)

- No `package.json`, `pyproject.toml`, `requirements.txt`, `go.mod`, `Cargo.toml`, `pom.xml`, or any other concrete-stack manifest.
- No `Dockerfile`, Terraform, Kubernetes, or framework configs (vite/next/jest/pytest/etc.).
- No `src/`, `app/`, `server/`, `client/` — no source code skeleton.
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

---

License: [MIT](./LICENSE). Not affiliated with, endorsed by, or created by Anthropic, GitHub, Cursor, or Andrej Karpathy.
