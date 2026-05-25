# Using This Template

This template is the **base layer** for new Claude Code projects. It is **not** an application. After deriving a project from it, you choose the stack inside that project and let BMAD-METHOD drive the development loop.

## 1. What this template gives you

| Layer | What it is |
| --- | --- |
| [`CLAUDE.md`](../CLAUDE.md) | Karpathy-style 60-line behavioral baseline + one beginner sentence. Loaded by Claude Code on every session. |
| `.gitignore` / `.gitattributes` / `.editorconfig` / `LICENSE` | Basic hygiene. Cross-OS line endings, common ignore patterns, MIT license. |
| `.claude/hooks/block_dangerous_commands.py` | Blocks Claude from running `rm -rf`, force-pushing, reading `.env` / private keys, printing secret env vars. |
| `.claude/settings.json` | Registers the hook above. |
| **BMAD-METHOD** (installed at template-init time) | Provides the PM / Architect / Developer / QA / UX agents and the SDLC workflow that takes a feature from idea → PRD → architecture → stories → code → QA. |
| `docs/` (this folder) | The two documents you are reading right now. |

That's it. No business code. No CI/CD pipelines. No security workflows. No Tier 1.5 policies. By design.

## 2. How to start a new project from this template

### Step 1 — Create the new repo from the template

On GitHub:

1. Go to https://github.com/lordrance/claude-code-bmad-foundation
2. Click the green **Use this template** → **Create a new repository**
3. Give it a real name (e.g. `my-game-pilot`)
4. **Public or Private** — your call per project
5. **Include all branches**: unchecked (`main` is enough)

If you prefer the terminal:

```bash
gh repo create lordrance/<your-new-project> \
  --template lordrance/claude-code-bmad-foundation \
  --public \
  --clone
cd <your-new-project>
```

### Step 2 — Clone and verify the hook is registered

```bash
cd <your-new-project>
python -m py_compile .claude/hooks/block_dangerous_commands.py   # should be silent
python -m json.tool .claude/settings.json                         # should print the JSON
```

If both succeed, the safety hook is live.

### Step 3 — Update README and LICENSE for the new project

The template ships with a README that describes the template itself. Replace it with one that describes your new project. Update the LICENSE copyright holder if needed.

### Step 4 — Configure GitHub settings for the new repo

`Use this template` copies the **files**, but it does **not** carry over these per-repo settings — you have to set them once in the new repo:

| What to set | Why it doesn't carry over | Where in GitHub UI |
| --- | --- | --- |
| **Description + topics** | GitHub policy; metadata is per-repo. | About panel (right side of repo main page) → ⚙ |
| **Branch protection on `main`** (optional for solo, recommended once collaborators join) | Not template-copied. | Settings → Rules → Rulesets → New ruleset |
| **Secret scanning + Push protection** | Defaults to ON for public repos; verify it is ON if you go private. | Settings → Code security |
| **Dependabot alerts + security updates** | Defaults vary; turn on explicitly. | Settings → Code security |
| **Merge prefs** (squash + rebase, no merge commit, delete branch on merge) | Not template-copied. | Settings → General → Pull Requests |
| **Disable Wiki / Projects** if you do not use them | Defaults to ON. | Settings → General → Features |

For a solo / personal project, you can skip branch protection. For anything you intend to share or take to production, enable at minimum:

- Require a pull request before merging
- Require status checks (after CI is added)
- Block force pushes
- Block deletions

These are the same defaults the **heavyweight sibling repo** (`claude-code-industrial-workbench`) configures automatically — when your project crosses that line, that template is the right move (see [`./decision-bmad-vs-full-workbench.md`](./decision-bmad-vs-full-workbench.md)).

### Step 5 — Now choose a stack — but **inside the new project, not back in the template**

Once you know what you're building (a web app, a CLI tool, a game backend…), add the stack-specific files inside the new project:

- Node project: add `package.json`, pick package manager
- Python project: add `pyproject.toml` + `uv` or `poetry` lockfile
- Go project: `go mod init`
- Etc.

The template intentionally does not pick this for you. Different projects need different stacks.

## 3. How to drive BMAD's PM → Architect → Dev → QA loop

BMAD is the workflow engine. Once installed, it provides specialized agents that take your idea through a structured pipeline. Open Claude Code in the new project's folder and:

### 3.1 — Talk to the PM agent first (NEVER skip this)

```
@pm I want to build <one-sentence description of the project>.
Help me write the PRD.
```

The PM agent will ask clarifying questions (target users, success criteria, scope boundaries). Answer them honestly. The output is a **Product Requirements Document** in `docs/prd.md` (or whichever folder BMAD chose).

### 3.2 — Hand the PRD to the Architect agent

```
@architect Read docs/prd.md and propose an architecture.
```

The Architect agent will produce `docs/architecture.md` covering:
- Tech stack recommendation (this is where the stack choice actually gets made)
- Component boundaries
- Data model sketches
- Major trade-offs and risks

Push back on anything that doesn't fit your context (e.g. "I don't want Postgres for a single-user CLI"). Architecture is iterative.

### 3.3 — Break architecture into stories

```
@scrum-master Break the architecture into developer-ready stories.
```

The result is a set of files under `docs/stories/` (or BMAD's chosen path). Each story is a small, mergeable unit of work with acceptance criteria.

### 3.4 — Let the Developer agent implement one story at a time

```
@dev Implement story 1.1.
```

Critical rule: **one story per session**. Don't tell the Dev agent to "implement everything." That defeats the loop.

### 3.5 — QA verifies each story before the next

```
@qa Verify story 1.1.
```

QA writes tests, checks acceptance criteria, and either approves the story or sends it back to dev. Only after QA approves do you move on to story 1.2.

### 3.6 — Commit each completed story as its own commit

Use Conventional Commits:

```bash
git add .
git commit -m "feat(story-1.1): implement <thing>"
```

This is the cycle. PRD → architecture → stories → dev → QA → commit → next story. Slower than vibe-coding, dramatically higher hit rate on the first try.

## 4. When to add wshobson/agents

Don't add it on day 1. Add it when **all three** are true:

1. You've done at least one full BMAD cycle on the project.
2. You've identified a specific gap BMAD doesn't cover well (e.g. "I need a deep refactor specialist", "I need an SEO-focused content writer", "I need a SQL query optimizer").
3. You've checked that no built-in BMAD agent handles it.

Then:

```bash
/plugin marketplace add wshobson/agents
/plugin install <specific-plugin>
```

Install **one plugin at a time**. Claude's tool list gets noisy fast, and noise reduces decision quality.

## 5. Why SuperClaude is intentionally NOT installed

SuperClaude is a popular alternative framework that injects its own `CLAUDE.md` and instruction files into `~/.claude/`. Two problems with installing it on top of this template:

1. It would overwrite or compete with the carefully preserved Karpathy `CLAUDE.md`, which is the behavioral baseline this template is built around.
2. Its persona-switching model overlaps with BMAD's agent-handoff model — running both produces confused behavior.

Choose one or the other, not both. This template's choice is BMAD.

## 6. Things this template will not solve for you

To be honest about scope:

- **Security review at scale** — for multi-person or regulated projects, use the heavyweight sibling [`claude-code-industrial-workbench`](https://github.com/lordrance/claude-code-industrial-workbench) instead. See [`./decision-bmad-vs-full-workbench.md`](./decision-bmad-vs-full-workbench.md).
- **Cost guardrails / token budgeting** — not implemented yet anywhere in the Claude Code ecosystem as of 2026-05. Watch your usage manually.
- **Eval harness / regression testing of AI behavior** — same as above.
- **Audit log of every AI action** — same as above.

These four are the Tier 3 gaps. They will land in a future revision of the heavyweight workbench, not here.

## 7. One-line summary

**This template ships you to the starting line; BMAD coaches you through the race; you finish projects faster than building from zero each time.**
