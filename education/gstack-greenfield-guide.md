# gstack Greenfield Project Guide

How to use gstack when starting a brand new project from zero.

---

## The Difference: Brownfield vs Greenfield

With a **brownfield** project (existing codebase), you typically jump straight to `/review` and `/ship` — the code already exists, you're iterating.

With a **greenfield** project, the most valuable skills are the ones that run **before any code is written**. gstack's upstream skills prevent the most expensive mistake in software: building the wrong thing fast.

---

## The Greenfield Pipeline

```
/office-hours → /design-consultation → /plan-ceo-review → /plan-eng-review → [code] → /qa → /review → /ship
```

Each step produces an artifact that feeds the next. Here's what each does and why it matters at project birth.

---

## Phase 1: Idea Validation — `/office-hours`

```
claude
> /office-hours
```

This is your starting point. It asks: **what are you building, and should you build it?**

### Two modes, automatically selected:

**Startup Mode** (if you're building a company or internal product):

Six forcing questions asked one at a time. These are YC-style questions designed to expose weak spots before you write a single line of code:

1. **Demand Reality** — "What's the strongest evidence someone actually wants this?" Not waitlist signups. Not "people say it's interesting." Real behavioral evidence.

2. **Status Quo** — "What are your users doing right now to solve this — even badly?" If the answer is "nothing," that's a red flag. No workaround often means no real pain.

3. **Desperate Specificity** — "Name the actual human who needs this most. What's their title? What gets them fired?" Category-level answers ("SMBs", "marketing teams") get pushed back.

4. **Narrowest Wedge** — "What's the smallest version someone would pay real money for this week?" Not after you build the platform. This week.

5. **Observation & Surprise** — "Have you watched someone use this without helping them? What surprised you?" Surveys lie. Demos are theater.

6. **Future-Fit** — "If the world looks different in 3 years, does your product become more essential or less?"

The skill pushes on each question until your answer is specific, evidence-based, and slightly uncomfortable. Comfort means you haven't gone deep enough.

**Builder Mode** (side project, hackathon, learning, open source):

Same structure but generative instead of interrogative:

- What's the coolest version of this?
- Who would you show this to? What would make them say "whoa"?
- What's the fastest path to something you can actually use?
- What existing thing is closest, and how is yours different?
- What would you add with unlimited time?

The vibe is enthusiastic collaborator, not interrogator.

### What it produces:

A **design document** saved to `~/.gstack/projects/{your-project}/`. This document persists across sessions and is automatically picked up by downstream skills (`/plan-ceo-review`, `/plan-eng-review`, `/design-consultation`).

### Why this matters for greenfield:

The biggest risk at project birth isn't bad code — it's building the wrong thing. `/office-hours` forces you to articulate the problem before jumping to solutions. With AI making coding 30-100x faster, the bottleneck has shifted from "can we build it?" to "should we build it?"

---

## Phase 2: Design System — `/design-consultation`

```
> /design-consultation
```

Creates your project's visual identity from scratch. This runs like a conversation with a senior product designer, not a form wizard.

### What happens:

1. **Product context** — Reads your `/office-hours` design doc (if it exists) to understand the product. If not, asks you.

2. **Competitive research** (optional) — Searches for products in your space. If the browse binary is available, it actually visits competitor sites, takes screenshots, and analyzes their typography, color palettes, and layout patterns.

3. **Three-layer design synthesis:**
   - **Layer 1 (tried-and-true):** What design patterns does every product in this category share? These are table stakes.
   - **Layer 2 (new-and-popular):** What's trending in current design discourse?
   - **Layer 3 (first-principles):** Given THIS product's users — is there a reason the conventional design approach is wrong?

4. **Proposes a complete system:** Aesthetic direction, typography (with specific font choices), color palette, layout approach, spacing scale, motion guidelines.

5. **Generates preview pages** — Font specimens and color swatches you can actually see.

### What it produces:

**`DESIGN.md`** — Your project's design source of truth. Every frontend skill (`/qa`, `/design-review`) reads this file to evaluate your UI against your stated design intentions.

### Why this matters for greenfield:

Starting without a design system means every UI decision is ad-hoc. You end up with 4 different font sizes, 7 shades of blue, and inconsistent spacing. `DESIGN.md` prevents this by making design decisions explicit before the first component is built.

---

## Phase 3: Strategic Review — `/plan-ceo-review`

```
> /plan-ceo-review
```

CEO/founder-mode review of your plan. Four selectable modes:

| Mode | Posture | When to use |
|------|---------|-------------|
| **SCOPE EXPANSION** | Dream big. "What would make this 10x better for 2x effort?" | Early stage, exploring what's possible |
| **SELECTIVE EXPANSION** | Hold scope as baseline, cherry-pick expansions individually | Have a solid plan, want to find opportunities |
| **HOLD SCOPE** | Make the plan bulletproof. Catch every failure mode | Plan is locked, need rigor |
| **SCOPE REDUCTION** | Find minimum viable version. Cut everything else | Need to ship fast, too much scope |

### What it does:

- Challenges your premises ("why do you assume X?")
- Maps failure modes, edge cases, shadow paths (nil input, empty input, upstream error)
- Requires ASCII diagrams for every non-trivial flow
- Evaluates decisions through Bezos's one-way/two-way door framework
- Applies "completeness is cheap" — if the full version costs minutes more with AI, do the full version

### Key cognitive patterns it applies:

- **Inversion reflex** — For every "how do we win?" also asks "what would make us fail?"
- **Focus as subtraction** — Primary value is what NOT to do
- **Speed calibration** — Fast is default, only slow down for irreversible + high-magnitude decisions
- **Proxy skepticism** — Are our metrics serving users or becoming self-referential?

### Why this matters for greenfield:

At project birth, scope decisions are the highest-leverage decisions you'll make. Building 3 features well beats building 10 features poorly. `/plan-ceo-review` helps you find the right scope before you've invested engineering time.

---

## Phase 4: Engineering Review — `/plan-eng-review`

```
> /plan-eng-review
```

Engineering manager mode. Locks in the architecture before you start coding.

### What it reviews:

- **Scope challenge** — Is the plan trying to do too much?
- **Architecture** — Are the right abstractions in place? Is the data model sound?
- **Code quality** — DRY, explicit over clever, minimal diff
- **Tests** — Test strategy, coverage plan, test diagram
- **Performance** — Are there obvious bottlenecks?

### Engineering preferences it enforces:

- Well-tested code is non-negotiable
- "Engineered enough" — not under-engineered (fragile) and not over-engineered (premature abstraction)
- Handle more edge cases, not fewer
- Observability is not optional — new codepaths need logs, metrics, or traces
- Security is not optional — new codepaths need threat modeling

### Why this matters for greenfield:

Architecture decisions made in week 1 are the hardest to change in month 6. `/plan-eng-review` catches structural issues (wrong database choice, missing auth model, N+1 query patterns) before they're baked into your codebase.

---

## Phase 5: Design Plan Review — `/plan-design-review`

```
> /plan-design-review
```

A designer's eye on your plan (not your live site — that's `/design-review`).

### Seven review passes:

1. **Information architecture** — Is the content organized logically?
2. **Visual hierarchy** — Will users see the most important things first?
3. **Specificity** — Are design decisions concrete or vague?
4. **Edge cases** — Empty states, error states, loading states, first-run experience
5. **AI slop detection** — Generic, committee-designed, could-be-anything-for-anyone patterns
6. **Responsive design** — Does the plan account for different screen sizes?
7. **Accessibility** — Color contrast, keyboard navigation, screen reader support

### Philosophy:

- Empty states are features, not afterthoughts
- Specificity over vibes ("a clean, modern look" is not a design decision)
- AI slop is the enemy — if it looks like every other AI-generated UI, it's wrong
- Subtraction is the default

---

## Phase 6: Build It

Now you code. Just normal Claude Code — no special gstack skill needed.

But you have context that most greenfield projects lack:
- A design doc from `/office-hours` telling you WHAT to build
- A `DESIGN.md` from `/design-consultation` telling you HOW it should look
- An architecture plan from `/plan-eng-review` telling you HOW to structure it
- Edge cases and failure modes mapped by `/plan-ceo-review`

---

## Phase 7: QA, Review, Ship

Same as the brownfield workflow:

```
/qa                  # Headless browser QA (if you have a UI)
/review              # Pre-landing code review
/ship                # Automated: tests → version bump → CHANGELOG → PR
/land-and-deploy     # Merge → deploy → verify
/canary              # Post-deploy monitoring
```

---

## Greenfield-Specific Tips

### 1. Don't skip `/office-hours`

It's tempting to jump straight to coding when you have an idea. Resist. The 15 minutes you spend in `/office-hours` saves you from building the wrong thing — which is the most expensive mistake when AI makes building the right thing so fast.

### 2. Run `/design-consultation` before your first UI component

Creating `DESIGN.md` before writing CSS means every component starts consistent. Retrofitting a design system onto an existing UI is 10x harder than starting with one.

### 3. Use `/plan-eng-review` even for small projects

"It's just a side project" is how you end up with no tests, no error handling, and a database schema that can't handle your second feature. With AI, doing it right costs almost nothing extra.

### 4. The artifacts carry forward

Everything produced by upstream skills is automatically discovered by downstream skills:
- `/office-hours` design doc → picked up by `/design-consultation`, `/plan-ceo-review`
- `DESIGN.md` → picked up by `/design-review`, `/qa`
- Review history → picked up by `/ship` (review gates)

You don't need to manually pass context between skills. They find each other's output.

### 5. You can enter at any point

The full pipeline is:
```
/office-hours → /design-consultation → /plan-ceo-review → /plan-eng-review → [code] → /qa → /review → /ship
```

But if you already know what you're building and just need to code, start at `[code]`. If you have the idea but need to validate it, start at `/office-hours`. If you have a plan and need an architecture review, start at `/plan-eng-review`.

The pipeline is a recommendation, not a requirement.

---

## Example: Starting a New SaaS App

```bash
mkdir ~/dev/my-saas && cd ~/dev/my-saas
git init
claude
```

**Session 1: Idea + Design**
```
> /office-hours
# Answer the six questions. Get a design doc.

> /design-consultation
# Get DESIGN.md with typography, colors, spacing, layout system.
```

**Session 2: Plan**
```
> /plan-ceo-review
# Strategic review. Pick SELECTIVE EXPANSION mode.
# Cherry-pick scope expansions that are worth the effort.

> /plan-eng-review
# Lock in architecture. Get test strategy. Map data model.
```

**Session 3+: Build**
```
# Write code with Claude Code. Build features.
# Use /investigate if you hit bugs.
# Use /freeze if you want to lock edits to one module.
```

**When ready to ship:**
```
> /qa                    # Browser QA (if you have UI)
> /review                # Code review
> /ship                  # Tests → PR → done
> /land-and-deploy       # Merge and verify
```

**Weekly:**
```
> /retro                 # What did I ship? How's test coverage? Hotspots?
```

---

## The Completeness Principle

gstack's core thesis: **with AI, the marginal cost of completeness is near-zero.**

In the old world, you'd skip tests, docs, design systems, and code reviews because they took too long. In the new world:

| What you'd skip | Human cost | AI cost | Skip it? |
|-----------------|-----------|---------|----------|
| Test coverage | 1 day | 15 min | No — boil the lake |
| Design system | 2 days | 30 min | No — prevents 10x rework |
| Architecture review | 2 days | 4 hours | No — week 1 decisions haunt month 6 |
| Code review | 4 hours | 15 min | No — `/review` is automated |
| CHANGELOG | 30 min | 0 min | No — `/ship` does it for you |
| Bisected commits | 1 hour | 0 min | No — `/ship` does it for you |

The full pipeline isn't overhead — it's cheaper than the shortcuts used to be.
