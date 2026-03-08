# Beacon

An AI college counselor that guides high school students through the complete 4-year college application journey — from freshman-year interest discovery through senior-year decisions.

12 commands covering interest elicitation, academic planning, extracurricular spike strategy, standardized testing, college list building, Socratic essay coaching, application strategy, financial aid, and summer planning. Beacon uses Motivational Interviewing techniques, tracks milestones against a grade-aware timeline engine, and maintains persistent state across sessions so nothing falls through the cracks. Not a generic advice bot. An adaptive counselor that meets students where they are and gets sharper the longer you use it.

Say `kickoff`, share your grade level, and you're being counseled in under 2 minutes.

---

## What It Does

**Interest discovery** -- Deep elicitation using Holland Code, flow activities, Ikigai, Gardner's Multiple Intelligences, and VIA Strengths frameworks. Helps students figure out what they're genuinely passionate about, not what they think they should be passionate about. Results feed directly into academic and activity planning.

**Academic planning** -- Maps discovered interests to one of nine academic tracks (Engineering, CS, Pre-Med, Business, Law-Humanities, Psychology-Education, Arts-Design, Trades-CTE, Undecided) with a complete 4-year course sequence. Aligns course rigor with target schools and what the student's school actually offers.

**Spike strategy** -- Builds "well-lopsided" extracurricular profiles instead of well-rounded ones. Identifies 1-2 spike areas and guides students from participation to leadership to impact to recognition using a four-tier activity ranking system (National, State, School, Community).

**Timeline tracking** -- A grade-aware milestone engine that checks the student's current position against where they should be every session. Surfaces upcoming deadlines, flags overdue milestones with opportunity framing, and adapts recommendations to the academic calendar and season.

**Socratic essay coaching** -- Guides students to find their own story and voice through questions, not templates. Uses anonymized strong essay examples for illustration. Never writes content for the student. Covers personal statements, supplemental essays, and activity descriptions.

**College list building** -- Constructs balanced reach/match/safety lists with fit analysis across academics, activities, culture, and cost. Includes demonstrated interest strategy, application type guidance (ED/EA/RD), and school-specific research using live web lookups.

**Financial aid guidance** -- FAFSA and CSS Profile walkthroughs, net price calculator direction for each school on the list, scholarship strategy (merit, local, national, essay-based), and financial aid comparison and negotiation coaching when offers arrive.

**Progress monitoring** -- Full milestone-by-milestone timeline checks, section-by-section assessment of the student's profile, and top priority actions based on grade level, season, and where the student is in the process. Coaching strategy adapts when something isn't working.

**Gamification** -- Progress pulses show visual progress bars every few questions so students see they're getting somewhere, not just answering an endless Q&A. Long workflows break into named chapters with natural stopping points -- students opt in to more rather than enduring a marathon. Micro-summaries reflect insights back in real-time ("Interesting -- you keep coming back to building things and leading teams. That's a pattern."). An achievement system awards 19 badges across journey milestones, streaks, depth, and engagement categories. Completing milestones unlocks new capabilities and makes the next step feel like a reward, not a chore.

**Visual dashboard** -- The `dashboard` command generates a self-contained HTML page and opens it in the browser. No server, no dependencies -- just a single file with your progress baked in. Shows your journey map (which commands you've completed), stats at a glance (track, GPA, spike strength, next milestone), achievement shelf (earned and locked badges), timeline status (color-coded milestones), and college list (reach/match/safety with strategy labels). Run `dashboard` anytime to refresh.

---

## Quick Start

1. Clone the repo:

```bash
git clone https://github.com/00derek/Beacon.git
cd beacon
```

Or download it as a ZIP and unzip.

2. Activate Beacon for your AI tool:

```bash
# Claude Code
cp SKILL.md CLAUDE.md

# Gemini CLI
cp SKILL.md GEMINI.md

# OpenAI Codex CLI
cp SKILL.md AGENTS.md
```

3. Open the folder in your AI tool and say `kickoff`.

Requires a paid plan for your chosen AI tool. Works with Claude Code, Gemini CLI, OpenAI Codex CLI, Cursor, or any AI coding environment with file system access.

---

Beacon will ask for your grade level, school, interests, and goals -- then build your profile, check your timeline, and give you a prioritized action plan. Everything saves automatically to `counseling_state.md` so you pick up where you left off next session.

---

## Commands

### Getting Started

| Command | What It Does |
|---------|-------------|
| `kickoff` | Set up your profile -- grade, school, interests, goals, and family context. Everything starts here. |

### Exploration & Planning

| Command | What It Does |
|---------|-------------|
| `discover` | Deep interest exploration using proven frameworks (Holland Code, flow activities, strengths, values). Helps you figure out what you're genuinely passionate about. |
| `plan` | Map your interests to an academic track and build a 4-year course sequence. Aligns course rigor with your goals and what your school offers. |
| `activities` | Build your extracurricular strategy. Identify your "spike" -- the 1-2 areas where you go deep. Depth beats breadth in modern admissions. |

### Testing & College Research

| Command | What It Does |
|---------|-------------|
| `testing` | SAT vs. ACT decision, prep planning, score analysis, AP exam strategy, and test-optional guidance. Builds a testing timeline matched to your college list. |
| `schools` | Build your college list -- reach, match, and safety schools with fit analysis across academics, activities, culture, and cost. Includes demonstrated interest strategy. |
| `summer` | Summer program strategy -- from prestigious research programs to meaningful self-directed projects. Grade-appropriate recommendations connected to your spike. |

### Applications & Essays

| Command | What It Does |
|---------|-------------|
| `essays` | Socratic essay coaching -- questions to help you find your story and your voice. Covers personal statement, supplements, and activity descriptions. Never writes for you. |
| `apply` | Application strategy -- ED/EA/RD decisions, recommendation letter strategy, activity descriptions, application platform guidance, and deadline management. |

### Financial

| Command | What It Does |
|---------|-------------|
| `financial` | FAFSA and CSS Profile guidance, net price calculators for your schools, scholarship strategy (merit, local, national, essay-based), and financial aid comparison and negotiation when offers arrive. |

### Progress

| Command | What It Does |
|---------|-------------|
| `review` | Full progress check -- milestone-by-milestone timeline status, section-by-section assessment, and top 3 priority actions based on your grade and where you are in the process. |
| `dashboard` | See your progress visually -- opens a dashboard in your browser with your journey map, achievements, timeline, and stats. |
| `help` | Command guide with context-aware recommendations based on where you are. |

---

## Workflow Examples

### 1) "I'm a freshman with no idea what I want to do"

```text
kickoff
```

Set up your profile, then:

```text
discover
```

Explore your interests through structured frameworks -- Holland Code, flow activities, strengths, and values. Then:

```text
plan
```

Map those interests to an academic track and build your course sequence. Then:

```text
activities
```

Find your spike area and start building extracurricular depth. Beacon walks you through this sequence naturally and recommends the next step after each command.

### 2) "I need to start my college applications"

```text
kickoff
```

Set up your profile (or Beacon picks up from your existing state). Then:

```text
schools
```

Build your reach/match/safety list with fit analysis and demonstrated interest strategy. Then:

```text
apply
```

Map out your application strategy -- ED/EA/RD decisions, recommendation letter asks, and deadline management. Then:

```text
essays
```

Socratic essay coaching for your personal statement and supplementals. Beacon helps you find your story through questions, not templates.

### 3) "Am I on track?"

```text
review
```

Full progress check against the timeline engine. You get milestone-by-milestone status (ahead, on-track, coming-up, behind), a section-by-section profile assessment, and the top 3 actions to focus on right now. Beacon frames overdue milestones as opportunities, not failures.

### 4) "Help me with my essay"

```text
essays
```

Beacon checks whether you have a narrative thread (from `discover` and `activities`). If you do, it uses that foundation to guide essay brainstorming. If you don't, it walks you through interest discovery first so your essay connects to a real story. All coaching is Socratic -- questions and anonymized examples, never drafting content for you.

---

## Gamification & Dashboard

Beacon keeps students engaged over a multi-year journey with lightweight gamification — no leaderboards, no competition, just personal progress tracking that makes the process feel rewarding.

### Progress Pulses

Long Q&A sessions break into chapters with visual progress:

```
━━━━━━━━━━░░░░░░░░░░ 50% through Discovery
🔓 Phase 3 complete: Strengths Mapping

"Halfway through! Want to continue with Values Clarification, or save this for next time?"
```

### Achievements

19 badges earned by completing milestones, maintaining streaks, and going deep:

```
🚀 Launched        — Complete kickoff
🧬 Interest DNA    — Complete all 6 phases of discovery
📐 Track Set       — Choose your academic track
🎯 Spike Found     — Identify your focus area
📊 Score Check     — Testing plan in place
🏫 List Built      — College list complete (reach/match/safety)
📝 Story Seed      — Personal statement theme found
🔥 Momentum x3    — 3 sessions in a row
⬆️ Level Up        — Activity reaches state-level recognition
🎓 Full Picture    — Application tells a cohesive story
...and 9 more
```

### Visual Dashboard

Type `dashboard` to open a browser-based progress page:

```
┌─────────────────────────────────────────────────────────┐
│  Maya Chen                              12 Sessions  4🔥│
│  Grade 11 · Westfield High School                       │
│                                                         │
│  Journey Map                                            │
│  🟢 Kickoff → 🟢 Discover → 🟢 Plan → 🟢 Activities   │
│  → 🔵 Testing → 🟢 Schools → 🔵 Essays → ⚪ Apply     │
│  → 🔵 Financial → 🔵 Summer                            │
│                                                         │
│  At a Glance                                            │
│  Track: Engineering    GPA: 3.82 / 4.3 W                │
│  Spike: [Robotics] [Human-Centered Design]              │
│  ████████████████░░░░ State-level                       │
│                                                         │
│  Achievements (10/19)                                   │
│  🚀 🧬 📐 🎯 📊 🏫 📝 🔥 ⬆️ 📋  🔒🔒🔒🔒🔒🔒🔒🔒🔒  │
│                                                         │
│  College List                                           │
│  Reach: MIT (EA) · Olin (ED)                            │
│  Match: Purdue (EA) · UMich (EA) · RPI (EA)            │
│  Safety: UConn (RD) · WPI (EA)                         │
└─────────────────────────────────────────────────────────┘
```

The dashboard is a self-contained HTML file -- no server, no dependencies. It reads your `counseling_state.md` data, renders it with clean CSS (dark mode supported, mobile-friendly), and opens in your default browser. Run `dashboard` again anytime to refresh with your latest progress.

---

## How It Works

**Session state** -- Beacon maintains a persistent `counseling_state.md` file that tracks your profile, interest discovery results, academic track, activities, testing, college list, essay progress, financial aid status, summer plans, recommendations, timeline milestones, coaching notes, and active counseling strategy. At the start of each session, it reads this file and picks up where you left off. Saves happen automatically after every major workflow -- not just at session end.

**Timeline engine** -- A milestone database organized by grade level and season (freshman fall through senior spring). Every session, Beacon checks your current position against the timeline and surfaces what's coming up, what's on track, and what needs attention. Recommendations are always grounded in where you actually are, not a generic checklist.

**Socratic approach** -- Beacon never writes application content for the student. Essays, activity descriptions, and personal statements are guided through questions. Anonymized strong examples are available for illustration, but the student's voice stays their own. This is enforced as a non-negotiable operating rule.

**Motivational Interviewing voice** -- Open-ended questions (70%+ of all questions), affirmations, reflective listening, and rolling with resistance. "I don't know" is always a valid answer -- it's information, not failure. Beacon adapts its directness level (1-5) based on the student's preference, but the underlying assessment never changes.

**Grade-aware adaptation** -- Beacon's tone, urgency, and command recommendations shift based on grade level. Freshmen get exploration mode with low pressure. Sophomores get building mode with emerging direction. Juniors get action mode with clear timeline urgency. Seniors get execution mode with specific deadlines.

**Counselor style selection** -- Students choose their preferred counseling style during kickoff: Guide (reflective and exploratory), Coach (action-oriented and direct), Supporter (patient and encouraging), Friend (casual and authentic), or Strategist (efficient and systematic). Each style changes how questions are asked and ordered, not what information is collected. Beacon monitors engagement and proposes style shifts when the current approach isn't landing -- the student always decides.

---

## Important Notes

**AI, not a human counselor.** Beacon supplements school counselors, parents, and professional advisors. It never replaces them. Beacon is transparent about being an AI in every interaction.

**Socratic only for all student writing.** Beacon will never draft essays, activity descriptions, or any application content. It guides through questions and shows anonymized examples. The student's voice must be their own.

**Safety protocol.** If any input contains crisis signals (self-harm, abuse, suicidal ideation), Beacon immediately surfaces professional resources (988 Suicide & Crisis Lifeline, school counselor, therapist) and pauses the current workflow. It never diagnoses, never attempts therapy, and never labels a student with any condition.

**Equity check always active.** Trades, CTE, community college, gap years, and military paths are presented with equal respect. Every student gets reach school suggestions regardless of GPA. First-generation students get proactive guidance on the hidden curriculum of college applications. Financial alternatives are always included alongside expensive options.

---

## Repository Structure

```text
beacon/
├── SKILL.md                            # Core skill -- copy to CLAUDE.md, GEMINI.md, or AGENTS.md to activate
├── README.md                           # This file
├── VERSIONS.md                         # Version roadmap
├── LICENSE                             # MIT License
├── dashboard-template.html             # Dashboard HTML template (used by the dashboard command)
├── counseling_state.md                 # Created on first kickoff (persistent memory, auto-saved)
├── dashboard.html                      # Generated by dashboard command (gitignored, student-specific)
└── references/
    ├── commands/                       # Per-command workflows (loaded on demand)
    │   ├── kickoff.md
    │   ├── discover.md
    │   ├── plan.md
    │   ├── activities.md
    │   ├── testing.md
    │   ├── schools.md
    │   ├── essays.md
    │   ├── apply.md
    │   ├── financial.md
    │   ├── summer.md
    │   ├── review.md
    │   ├── dashboard.md
    │   └── help.md
    ├── cross-cutting.md                # Shared modules: spike development, narrative threading, equity check, MI techniques
    ├── academic-tracks.md              # Track definitions, course sequences, activity recommendations
    ├── admissions-knowledge.md         # Holistic review, testing landscape, financial aid, application types
    ├── elicitation-frameworks.md       # Holland Code, Flow, Ikigai, Gardner, VIA frameworks and question banks
    ├── essay-examples.md              # Anonymized strong essay examples for Socratic illustration
    ├── timeline-engine.md              # Grade-aware milestone database (freshman through senior)
    └── safety-protocol.md              # Crisis detection, resource routing, reality check layer
```

---

## Disclaimer

Beacon is an AI tool, not a professional counselor, financial advisor, or legal advisor. The information and guidance it provides are for educational purposes only and should not be relied upon as professional advice.

- **Not a substitute for professional guidance.** Always consult your school counselor, a licensed college admissions consultant, or a qualified financial advisor for important decisions.
- **No guarantee of accuracy.** Beacon may produce outdated, incomplete, or incorrect information. Always verify deadlines, admission requirements, financial aid details, and school policies through official sources.
- **No liability.** The creators of Beacon are not responsible for any decisions made or outcomes resulting from its use.
- **Intended for supplemental use.** Beacon is designed to support — not replace — the guidance of parents, teachers, school counselors, and other trusted adults.

By using Beacon, you acknowledge that it is an AI tool with inherent limitations and that all important decisions should be independently verified.

## License

MIT
