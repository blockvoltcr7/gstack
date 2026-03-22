# gstack Power User Guide

A practical guide to using gstack on existing (brownfield) projects and new projects alike.

---

## What is gstack?

gstack is an AI engineering toolkit that gives Claude Code (and Codex, Gemini CLI, Cursor) structured workflows for the entire development lifecycle. It's 25+ slash commands that turn Claude Code from a coding assistant into a full development pipeline — brainstorming, planning, coding, reviewing, shipping, deploying, and monitoring.

**Core philosophy (from ETHOS.md):**

- **Boil the Lake** — With AI, completeness costs near-zero. Don't skip tests, docs, or reviews. Do the complete thing.
- **Search Before Building** — Check if the runtime/framework already has a built-in before designing from scratch.
- **Build for Yourself** — The best tools solve your own real problems.

---

## Installation (One-Time)

```bash
# 1. Clone gstack
git clone https://github.com/garrytan/gstack.git ~/.claude/skills/gstack

# 2. Run setup (builds browse binary, links skills globally)
cd ~/.claude/skills/gstack && ./setup
```

If you already have gstack cloned elsewhere (e.g. `~/dev/gstack/gstack`):

```bash
# Symlink into Claude's global skills directory
mkdir -p ~/.claude/skills
ln -snf ~/dev/gstack/gstack ~/.claude/skills/gstack

# Run setup from the skills directory
cd ~/.claude/skills/gstack && ./setup
```

After setup, all 24 skills are available globally — use them from any directory.

---

## Day Zero: First Time in an Existing Project

```bash
cd ~/dev/my-existing-app
claude
```

The first time you use a gstack skill (like `/ship` or `/qa`) in a new project, it reads your `CLAUDE.md` for project config. If config is missing, it asks:

- What's your test command? (`npm test`, `pytest`, `bun test`, etc.)
- What's your deploy target? (Vercel, GCP, AWS, etc.)

Answers are saved to your project's `CLAUDE.md` so you're never asked again.

---

## The Full Workflow Pipeline

```
/office-hours → /plan-eng-review → [code] → /review → /ship → /land-and-deploy → /canary
```

You don't have to use every step. The minimum viable workflow is:

```
[code] → /ship
```

`/ship` will tell you if you skipped reviews and let you override.

---

## Daily Workflow (Power User)

### 1. Debugging

```
/investigate
```

Structured debugging with an iron law: **no fixes without root-cause investigation first.** It:

- Locks your edit scope to the affected module (via `/freeze`)
- Runs 4 phases: Investigation → Pattern analysis → Hypothesis testing → Implementation
- Generates regression tests automatically

### 2. Planning (Optional, Recommended for Non-Trivial Work)

```
/office-hours              # Brainstorm the idea, get a design doc
/plan-ceo-review           # Strategic review — push scope up or cut scope down
/plan-eng-review           # Architecture review — lock in the plan before coding
/plan-design-review        # Design decisions review — before implementation
```

**`/office-hours`** has two modes:

- **Startup mode**: Six forcing questions that expose product-market validation gaps
- **Builder mode**: Design thinking for side projects, hackathons, learning

### 3. Building

Just normal Claude Code — no special skill needed. Write your code.

### 4. Reviewing

```
/review
```

Pre-landing code review with two passes:

- **Pass 1 (Critical):** SQL injection, race conditions, LLM trust boundaries, enum completeness
- **Pass 2 (Informational):** Dead code, magic numbers, test gaps, performance

Also checks for **scope drift** — did you build what you intended, or did extra stuff sneak in?

Findings are classified as:
- **AUTO-FIX**: Dead code, N+1 queries, stale comments → fixed automatically
- **ASK**: Complex fixes, ambiguous intent → asks for your decision

### 5. Shipping

```
/ship
```

Fully automated, non-interactive workflow:

1. Checks you're on a feature branch (not main)
2. Merges base branch
3. Runs your tests
4. Checks review readiness (did you run `/review`? `/plan-eng-review`?)
5. Bumps VERSION, updates CHANGELOG
6. Bisects your work into clean, logical commits
7. Creates the PR

**Only stops for:** merge conflicts, test failures, review gate decisions, major/minor version bumps.

### 6. Deploying

```
/land-and-deploy
```

Merge PR → wait for deploy → verify production health. Mostly automated with one pre-merge readiness gate.

### 7. Monitoring

```
/canary                    # 10-minute post-deploy visual monitoring
/benchmark                 # Performance regression detection (TTFB, LCP, bundle size)
```

### 8. End of Week

```
/retro                     # Weekly retrospective
/retro 24h                 # Last 24 hours
/retro 14d                 # Last 2 weeks
/retro compare             # Compare this week vs last week
```

Shows: commits to main, contributors, PRs merged, LOC added/deleted, test health, hotspot analysis.

---

## Frontend-Specific Skills

### Browser Setup

```
/setup-browser-cookies     # Import your logged-in browser session (Chrome, Arc, Brave, Edge)
```

Required for QA testing authenticated pages. Supports interactive picker or direct domain import.

### QA Testing

```
/qa                        # Find bugs → fix them → verify (3 tiers: Quick, Standard, Exhaustive)
/qa-only                   # Report bugs only, no fixes
```

Uses a headless Chromium browser to actually click through your app, fill forms, check console errors, take screenshots.

### Visual Design

```
/design-review             # Visual polish on live sites — spacing, typography, hierarchy, responsive
/design-consultation       # Create a DESIGN.md with your design system (colors, typography, spacing)
```

`/design-review` makes atomic commits with before/after screenshots for each fix.

---

## Safety Skills

Use when touching production, shared systems, or during focused debugging:

| Command | What it does |
|---------|-------------|
| `/guard` | Full safety: destructive command warnings + edit scope locking |
| `/careful` | Warns before `rm -rf`, `DROP TABLE`, `git push -f`, `kubectl delete`, etc. |
| `/freeze src/auth/` | Locks all edits to a specific directory |
| `/unfreeze` | Removes the edit lock |

`/guard` combines `/careful` + `/freeze` for maximum safety.

---

## Skill Reference (All 24 Skills)

### Brainstorming & Planning

| Skill | Purpose |
|-------|---------|
| `/office-hours` | YC-style idea validation or builder brainstorming |
| `/design-consultation` | Create DESIGN.md with complete design system |
| `/plan-ceo-review` | Strategic plan review (4 modes: expand, selective, hold, reduce scope) |
| `/plan-eng-review` | Architecture & engineering plan review |
| `/plan-design-review` | Designer's eye plan review (7 passes) |

### Implementation & Debugging

| Skill | Purpose |
|-------|---------|
| `/investigate` | Systematic root-cause debugging (4 phases) |
| `/browse` | Direct headless browser interaction |

### QA & Design

| Skill | Purpose |
|-------|---------|
| `/qa` | QA test → fix → verify loop |
| `/qa-only` | QA report only (no fixes) |
| `/design-review` | Live site visual audit + fixes |

### Code Review

| Skill | Purpose |
|-------|---------|
| `/review` | Pre-landing PR review (2-pass: critical + informational) |
| `/codex` | Second opinion via OpenAI Codex CLI |

### Shipping & Deployment

| Skill | Purpose |
|-------|---------|
| `/ship` | Automated: tests → review → version bump → CHANGELOG → PR |
| `/land-and-deploy` | Merge PR → deploy → verify production |
| `/document-release` | Post-ship documentation updates |
| `/setup-deploy` | Auto-detect deploy platform, save config |

### Monitoring

| Skill | Purpose |
|-------|---------|
| `/canary` | Post-deploy visual monitoring (10 min default) |
| `/benchmark` | Performance regression detection |

### Retrospectives

| Skill | Purpose |
|-------|---------|
| `/retro` | Weekly engineering analytics with per-author breakdown |

### Safety

| Skill | Purpose |
|-------|---------|
| `/careful` | Destructive command warnings |
| `/freeze` | Lock edits to a directory |
| `/unfreeze` | Remove edit lock |
| `/guard` | Full safety mode (careful + freeze) |

### Utilities

| Skill | Purpose |
|-------|---------|
| `/gstack-upgrade` | Upgrade to latest version |
| `/setup-browser-cookies` | Import authenticated browser sessions |

---

## How gstack Differs From Plain Claude Code

| Aspect | Plain Claude Code | Claude Code + gstack |
|--------|------------------|---------------------|
| **Code review** | Ad-hoc, whatever you ask for | Structured 2-pass review with scope drift detection |
| **Shipping** | Manual: commit, push, create PR | Automated: tests → review gate → version bump → CHANGELOG → bisected commits → PR |
| **QA** | Read code and guess | Headless browser: clicks, screenshots, console errors, form testing |
| **Review gates** | None | `/ship` checks if you ran `/review` and `/plan-eng-review` |
| **Project memory** | Per-session only | Persistent: design docs, review history, test plans in `~/.gstack/projects/` |
| **Scope control** | None | `/freeze` locks edits to one directory; `/review` detects scope creep |
| **Deploy verification** | Manual | `/canary` monitors post-deploy with screenshots and error tracking |
| **Retrospectives** | None | `/retro` with commit stats, LOC, test health, contributor breakdown |

---

## AI Effort Compression

The fundamental insight: with AI assistance, completeness is near-zero marginal cost.

| Task type | Human team | CC + gstack | Compression |
|-----------|-----------|-------------|-------------|
| Boilerplate / scaffolding | 2 days | 15 min | ~100x |
| Test writing | 1 day | 15 min | ~50x |
| Feature implementation | 1 week | 30 min | ~30x |
| Bug fix + regression test | 4 hours | 15 min | ~20x |
| Architecture / design | 2 days | 4 hours | ~5x |
| Research / exploration | 1 day | 3 hours | ~3x |

---

## Tips & Best Practices

1. **Start with `/ship`** — It's the gateway skill. Even if you skip everything else, `/ship` gives you automated tests, version bumps, changelogs, and clean commits.

2. **Use `/review` before `/ship`** — `/ship` checks for it. Running `/review` first means fewer surprises at the review gate.

3. **`/investigate` before fixing bugs** — Don't jump to fixes. Root-cause first, then fix. The regression test it generates prevents the bug from coming back.

4. **`/guard` when touching prod** — One command activates both destructive command warnings and edit scope locking. Peace of mind.

5. **`/retro` every Friday** — Even solo, it's valuable to see what you shipped, which files were hotspots, and whether your test coverage kept up.

6. **`/qa` catches what code review can't** — Code review finds structural issues. `/qa` finds the button that doesn't work, the form that overflows, the console error on page load.

7. **Let skills ask you questions** — First time in a project, skills ask for config (test command, deploy target). Answer honestly and they save it to `CLAUDE.md`. Don't pre-fill — let the skill discover what it needs.

8. **Commit style: bisect everything** — gstack's `/ship` auto-bisects commits. Each commit is one logical change, independently revertable. This is intentional — it makes `git bisect` actually useful.
