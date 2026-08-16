# AI-Ready Repo — Agent Guide

This is an **Agent Skill** — not a traditional application. It contains no source code to build or test. The deliverable is a skill definition (`SKILL.md`) that teaches AI coding agents (Claude Code, GitHub Copilot CLI, and others that support the Agent Skills format) how to make any repository AI-ready.

## Repository Structure

```
ai-ready/
├── .github/
│   ├── copilot-instructions.md     # Conventions for contributing to THIS repo (Copilot)
│   ├── plugin/
│   │   ├── plugin.json             # Plugin manifest for `copilot plugin install`
│   │   └── marketplace.json        # Marketplace manifest for Awesome Copilot install button
│   ├── workflows/copilot-setup-steps.yml  # Cloud agent setup (checkout only — no build)
│   ├── dependabot.yml              # GitHub Actions dependency updates
│   ├── workflows/ci.yml            # PR validation (skill integrity checks)
│   ├── ISSUE_TEMPLATE/             # Bug reports, feature requests, new skill ideas
│   ├── PULL_REQUEST_TEMPLATE.md    # PR checklist (integrity checks, test evidence)
│   └── CODEOWNERS                  # @johnpapa owns all paths
├── .claude/
│   └── skills/
│       └── ai-ready/
│           ├── SKILL.md               # The 12-step skill procedure (<500 lines)
│           └── references/           # Detailed reference material (loaded on demand)
│               ├── github-discovery.md   # GitHub API tables, PR mining, health gaps
│               ├── detection-tables.md   # Manifest detection, course/monorepo heuristics
│               ├── report-template.md    # Report format, HTML spec, badge, PR flow
│               └── training-repos.md     # Repos used to validate skill heuristics
├── docs/
│   └── how-it-works.md             # Detailed explanation of the 3 mechanisms + 12 assets
├── examples/
│   ├── sample-report-peacock.html  # Sample HTML report (GitHub Pages)
│   └── sample-report-peacock.md    # Sample markdown report
├── images/                         # Screenshots and visual assets
├── .vscode/
│   └── settings.json               # Editor settings
├── AGENTS.md                       # This file
├── CHANGELOG.md                    # Version history
├── README.md                       # Project overview, quick start, what gets generated
├── SECURITY.md                     # Vulnerability reporting policy
└── LICENSE                         # MIT
```

## Tech Stack

- **Content format:** Markdown, YAML, JSON
- **No runtime, build system, or test framework** — this is a documentation-driven project

## Build & Run

There is no build step. This repo ships markdown and JSON files that Claude Code, Copilot CLI, and other Agent-Skills-compatible agents read directly.

**To test the skill locally (Claude Code):**

```bash
cp -r .claude/skills/ai-ready ~/.claude/skills/ai-ready
```

Then start Claude Code and invoke the skill by name or trigger phrase.

**To test the skill locally (Copilot CLI):**

```bash
copilot plugin install johnpapa/ai-ready
```

Then start Copilot and invoke the skill:

```bash
copilot
```

```
make this repo ai-ready
```

## Testing

There is no automated test suite. Validation is:

1. **Skill integrity** — SKILL.md exists and frontmatter is valid
2. **Smoke test** — install the skill, invoke it on a sample repo, verify the analysis is correct and files are generated properly
3. **CI** — the workflow validates YAML syntax and skill frontmatter on every PR

## Key Patterns and Conventions

- **The skill lives at `.claude/skills/<name>/SKILL.md`** — a spec-recognized location loaded automatically by Claude Code, and also copyable into `.github/skills/` or `.agents/skills/` for other agents. Each skill is a markdown file with YAML frontmatter (`name`, `description`) and step-by-step instructions
- **The skill is self-sufficient** — it uses agent-native file search, read, and write/edit capabilities to analyze repos and generate files. No custom extensions, code, or vendor-specific tool names required
- **Never overwrite existing files** — the skill checks for existing assets before generating
- **Issue/PR provenance is required** — issue and PR communication produced by this skill must explicitly mention AI Ready (for example: `Assisted by [ai-ready](https://github.com/johnpapa/ai-ready)`)
- **Docs must stay in sync** — when skill behavior changes, update `README.md`, `docs/how-it-works.md`, and `CHANGELOG.md` to match repo standards
- **PR conflicts must be addressed** — when opening PRs, attempt conflict resolution first; if unresolved, ask for user direction

## Adding a New Skill

1. Create `.claude/skills/<skill-name>/SKILL.md` with YAML frontmatter:
   ```yaml
   ---
   name: skill-name
   description: What this skill does and when to invoke it
   ---
   ```
2. Write step-by-step instructions in the markdown body
3. Update `README.md` to mention the new skill
4. Update this file (`AGENTS.md`) to reflect the new structure

## Common Pitfalls

- **Don't add build/test/runtime dependencies** — this is a markdown-only project. Agents should not invent `npm install`, `pip install`, or any setup commands for this repo
- **SKILL.md frontmatter is required** — the `name` and `description` fields in the YAML frontmatter are how Claude Code, Copilot, and other agents discover and match the skill to user requests
- **Test on real repos** — the only meaningful test is invoking the skill on different repo types (Node.js, Python, Go, Rust, etc.) and verifying the output
