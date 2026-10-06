---
framework_version: 1.0.0
---

# Agent Guidelines: AI Job Search

This workspace is structured to manage job search activities, scraper tools, CVs, cover letters, and interview preparation.

## Thin-Pointer Design (Single Source of Truth)

To prevent duplication and configuration drift across different AI agent frameworks (Claude Code, Google Antigravity, Codex, Cursor, Gemini CLI, OpenCode, etc.), this workspace uses a unified thin-pointer design. All agent runtimes should load the canonical specifications and candidate profiles from the files and directories below:

`.claude/` is the canonical source of workflows.
`.agents/skills/` contains portable CLI tools.
`.opencode/` contains only the OpenCode integration layer.

1. **Personal Candidate Profile:**
   - The candidate profile, contact details, education, and target preferences are defined in [CLAUDE.md](CLAUDE.md) and the individual profile methodology files under [.claude/skills/job-application-assistant/](.claude/skills/job-application-assistant/) (specifically `01-*.md` etc.).
2. **Canonical Workflow Specifications:**
   - The step-by-step instructions and triggers for tasks (setup, scrape, rank, apply, upskill, interview) are defined in the [.claude/](.claude/) directory (specifically under `.claude/skills/` and `.claude/commands/`).
   - Do not duplicate these rules or specifications. Treat `.claude/` files as the single source of truth.
3. **Portal Search Skills:**
   - Job-portal search CLIs live under [.agents/skills/](.agents/skills/) in the portable Agent Skills format (with a `SKILL.md` per portal). Codex and Antigravity discover these automatically; the `/scrape` workflow in [.claude/skills/job-scraper/](.claude/skills/job-scraper/) orchestrates them.

## OpenCode Compatibility

This repository has been adapted to work with [OpenCode](https://opencode.ai). See [docs/opencode-setup.md](docs/opencode-setup.md) for the full setup guide.

### Tool Name Mapping

Commands reference Claude Code tool names. When running in OpenCode, the model maps them to OpenCode equivalents:

| Claude Code   | OpenCode      | Notes |
|---------------|---------------|-------|
| `Read`        | `read`        | File reading |
| `Write`       | `write`       | File creation |
| `Edit`        | `edit`        | File editing |
| `Glob`        | `glob`        | File pattern search |
| `Grep`        | `grep`        | Content search |
| `WebFetch`    | `webfetch`    | Fetch URL content |
| `WebSearch`   | `websearch`   | Web search |
| `Bash`        | `bash`        | Shell commands |
| `Agent`       | `task`        | Subagent dispatch (use `general` subagent type) |
| `AskUserQuestion` | `question` | Ask user for input |
| `Skill(...)`  | `skill`       | Load a skill |

### Subagent Dispatch

Where commands say "Use the **Agent tool** to spawn a `general-purpose` agent", in OpenCode use the **Task tool** with `subagent_type: "general"`.

The reviewer agent for `/apply` is pre-configured in `.opencode/agents/reviewer.md`. The scoring agent for `/rank` is pre-configured in `.opencode/agents/scoring-agent.md`.

### Command Discovery

Commands in `.opencode/commands/` are symlinks to `.claude/commands/`. OpenCode discovers them automatically as custom commands (e.g., `/apply`, `/setup`).

## Learned User Preferences

- Prefer responses in Portuguese
- Primary job search: remote Product Designer roles that accept Brazil-based candidates and pay in USD (prefer remote-anywhere / worldwide; treat US-only remote as weak unless Brazil eligibility is explicit); CV and cover letter in English
- Secondary job search: remote Product Designer roles in Brazil; CV and cover letter in Portuguese
- When searching jobs, prefer portals beyond LinkedIn (multi-portal scrape) for worldwide-eligible remote roles
- When building or expanding profile docs, prefer completeness over brevity
- Exclude older short LinkedIn roles from active profile materials (Awari, ILA 2018, Treee, old freelancing, and similar)
- Ground profile content and application materials in real experience from the candidate’s documents and notes; do not invent roles or achievements
- For interview and application answers, prioritize concepts grounded in real experience over mirroring the job posting’s wording

## Learned Workspace Facts

- `notion-job-searching-notes/` is a Notion export archive of past applications, cover letters, and experience writeups (reference only; canonical profile lives in CLAUDE.md and `.claude/skills/job-application-assistant/`)
- Source onboarding documents (CV PT/EN, LinkedIn PDF) live under `documents/`

## Cursor Cloud specific instructions

This repo has no dev server. Cloud Agents get Poppler, Bun, and TinyTeX from the environment install (nothing to start on boot). `lualatex`, `xelatex`, and `tlmgr` resolve from `/usr/local/bin` (TinyTeX tree: `~/.TinyTeX`; the installer also links engines into `~/bin`). Bun is `/usr/local/bin/bun`.

- CV: `cd cv && lualatex -interaction=nonstopmode -halt-on-error main_example.tex` (2 pages). Cover letter: `cd cover_letters && xelatex -interaction=nonstopmode -halt-on-error cover_example.tex` (1 page). Then `python3 tools/verify_pdf.py` on both PDFs.
- Python: `python3 tools/lint_skills.py`, `python3 tools/security_guards.py`, and `python3 -m unittest discover -s tests -t . -v`.
- Each portal CLI under `.agents/skills/*/cli` uses `bun install`, `bun run typecheck`, and `bun test`. Do not send live LinkedIn requests from automation. A public smoke search is `bun run .agents/skills/freehire-search/cli/src/cli.ts search -q "product designer" --remote remote --limit 5 --format table`.
