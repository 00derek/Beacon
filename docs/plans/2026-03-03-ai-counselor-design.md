# AI Counselor — Design Document

**Date**: 2026-03-03
**Status**: Approved
**Architecture**: Approach A (Mirror Interview Coach Skill)

## Overview

A Claude Code skill that guides high school students (freshman through senior year) and their parents through the entire college application journey. Built on the same architecture as the interview-coach-skill: command-based workflows, persistent state, reference modules loaded on demand, and file-based persistence with no backend.

## Key Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| V1 Scope | Full 4-year journey | Cover the complete pipeline from interest discovery to college commitment |
| Primary User | Student-led, parent access | Student drives conversation; parent can review state/progress |
| Writing Mode | Socratic with examples | Never draft student content; show anonymized examples; guide through questions |
| Safety Approach | Light emotional support | Acknowledge stress, normalize anxiety, redirect to professionals for anything beyond normal |
| Proactivity | Timeline tracking | Grade-aware milestones, session greeting with status, flag overdue items |

---

## Project Structure

```
ai-counselor/
├── CLAUDE.md                          # Core system prompt
├── SKILL.md                           # Copy of CLAUDE.md (for distribution)
├── README.md                          # User guide
├── VERSIONS.md                        # Version roadmap
├── LICENSE
├── .claude/settings.json              # Permissions (Read, Edit, Write, WebFetch, WebSearch)
│
└── references/
    ├── commands/                       # One file per command
    │   ├── kickoff.md                 # Onboarding + profile creation
    │   ├── discover.md                # Interest elicitation
    │   ├── plan.md                    # Academic track mapping
    │   ├── activities.md              # Extracurricular strategy
    │   ├── testing.md                 # SAT/ACT strategy
    │   ├── schools.md                 # College list building
    │   ├── essays.md                  # Socratic essay coaching
    │   ├── apply.md                   # Application strategy
    │   ├── financial.md               # Financial aid + scholarships
    │   ├── summer.md                  # Summer program strategy
    │   ├── review.md                  # Progress + timeline check
    │   └── help.md                    # Command menu
    │
    ├── cross-cutting.md               # Shared modules (spike, narrative, equity, MI)
    ├── safety-protocol.md             # Emotional support + referral logic
    ├── elicitation-frameworks.md      # Holland, Ikigai, Flow, Gardner, VIA
    ├── academic-tracks.md             # 7 tracks + course sequences
    ├── admissions-knowledge.md        # Holistic review, demonstrated interest, timelines
    ├── essay-examples.md              # Anonymized strong essay examples
    └── timeline-engine.md             # Grade-aware milestone tracking
```

**Persistent state**: `counseling_state.md` (auto-created on `kickoff`)

---

## Counseling State Schema

`counseling_state.md` is auto-created on `kickoff` and persists across sessions.

### Profile
- Name, Grade, School
- Target graduation year
- GPA (weighted/unweighted)
- Counselor directness level (1-5)
- Parent involvement preference
- Key concerns

### Interest Discovery
- Holland Code results (RIASEC)
- Flow activities (lose track of time)
- Strengths identified
- Values clarified
- Interest evolution log (interests change — track over time)

### Academic Track
- Primary track (e.g., Engineering, Pre-Med, Business)
- Secondary interests
- Current course load
- Planned course sequence (by year)
- GPA trajectory

### Activities & Spike
- Spike area(s)
- Activity list (name, role, hours/week, years, tier level)
- Leadership positions
- Impact/achievements
- Activity gaps identified

### Testing
- Practice scores (SAT/ACT)
- Official scores
- Target score range
- Test preference (SAT vs ACT)
- Prep plan status

### College List
- Reach (2-3)
- Match (4-5)
- Safety (2-3)
- Per-school: name, type, ED/EA/RD strategy, demonstrated interest actions, fit notes

### Essays
- Personal statement topic/theme
- Brainstorming notes (Socratic Q&A log)
- Supplemental essay tracker (school, prompt, status)
- Narrative thread (how essays connect to spike/story)

### Financial
- FAFSA status
- CSS Profile status
- Net price calculator results
- Scholarship list (name, deadline, amount, status)
- Family financial context (optional)

### Summer Experiences
- Per year: activity, description, connection to spike

### Timeline Status
- Current milestone statuses (ahead/on-track/coming-up/behind)
- Last timeline check date

### Session Log
- Date, topics covered, key decisions, next steps

### Coaching Notes
- Active coaching strategy (current focus area)
- Previous approaches
- Observations about student engagement/motivation

---

## Command Registry

| Command | Purpose | Reference Files Loaded |
|---------|---------|----------------------|
| `kickoff` | Onboarding, profile creation, initial interest exploration | elicitation-frameworks, timeline-engine |
| `discover` | Deep interest elicitation (Holland, Flow, Ikigai) | elicitation-frameworks, cross-cutting |
| `plan` | Map interests to academic track, course sequence | academic-tracks, timeline-engine |
| `activities` | Extracurricular strategy, spike development | cross-cutting |
| `testing` | SAT/ACT strategy, prep planning, score analysis | admissions-knowledge, timeline-engine |
| `schools` | College list building (reach/match/safety) | admissions-knowledge |
| `essays` | Socratic essay coaching with anonymized examples | essay-examples, safety-protocol |
| `apply` | Application strategy (ED/EA/RD, demonstrated interest) | admissions-knowledge, timeline-engine |
| `financial` | FAFSA, CSS Profile, scholarships, net price | admissions-knowledge |
| `summer` | Summer program strategy by tier | admissions-knowledge, timeline-engine |
| `review` | Full progress check against timeline | timeline-engine, cross-cutting |
| `help` | Command menu with context-aware recommendations | — |

---

## Operating Rules

1. **Always identify as AI** — never pretend to be a human counselor
2. **One question at a time** — respect teen attention, don't overwhelm
3. **Motivational Interviewing voice** — open questions, affirmations, reflective listening, no lecturing
4. **Socratic for all student writing** — never draft content, show anonymized examples, guide through questions
5. **Strengths-first** — lead with what's working, then gaps as opportunities
6. **Evidence-tagged claims** — use confidence labels (High/Medium/Low) for admissions advice
7. **Timeline-aware greeting** — every session starts with grade-aware status + recommendation
8. **Anti-bias enforcement** — never gatekeep based on demographics, validate trades/CTE equally, offer reach schools to all
9. **Light emotional support** — acknowledge stress, normalize anxiety, redirect to professional resources for anything beyond normal college stress
10. **Session state discipline** — load state at start, save after major workflows, save at end
11. **No diagnosis, no therapy** — stay in the college counseling lane
12. **Proactive help surfacing** — naturally mention relevant commands

---

## Timeline Engine

Grade-aware milestone tracker. Used by `review` command and session greetings.

### Freshman Year

| Semester | Milestone | Category |
|----------|-----------|----------|
| Fall | Complete interest discovery (`discover`) | Exploration |
| Fall | Join 2-3 clubs/activities aligned with interests | Activities |
| Fall | Establish strong study habits, target high GPA | Academics |
| Spring | Identify 1-2 "spike" activities to go deep on | Activities |
| Spring | Map preliminary academic track (`plan`) | Academics |
| Summer | First meaningful summer experience | Summer |

### Sophomore Year

| Semester | Milestone | Category |
|----------|-----------|----------|
| Fall | Take leadership role in spike activity | Activities |
| Fall | Begin honors/AP coursework aligned with track | Academics |
| Spring | Take practice SAT and ACT, pick one (`testing`) | Testing |
| Spring | Research summer programs (Tier 1-3) | Summer |
| Spring | Start building preliminary college list | Schools |
| Summer | Tier 1/2 program application or self-directed project | Summer |

### Junior Year

| Semester | Milestone | Category |
|----------|-----------|----------|
| Fall | Finalize course rigor (AP/IB alignment with track) | Academics |
| Fall | SAT/ACT first official attempt | Testing |
| Fall | Begin college research in earnest (`schools`) | Schools |
| Winter | Start brainstorming personal statement topics | Essays |
| Spring | SAT/ACT retake if needed | Testing |
| Spring | Visit colleges (or virtual tours) | Schools |
| Spring | Ask for recommendation letters | Applications |
| Spring | Run net price calculators (`financial`) | Financial |
| Summer | Draft personal statement (`essays`) | Essays |
| Summer | Finalize college list (2-3 reach, 4-5 match, 2-3 safety) | Schools |

### Senior Year

| Semester | Milestone | Category |
|----------|-----------|----------|
| Sep | Finalize ED/EA strategy (`apply`) | Applications |
| Oct | FAFSA opens — file early (`financial`) | Financial |
| Nov 1 | ED/EA deadlines | Applications |
| Nov | CSS Profile submitted (if needed) | Financial |
| Jan 1 | RD deadlines for most schools | Applications |
| Feb-Mar | Scholarship applications | Financial |
| Mar-Apr | Decisions arrive, compare financial aid | Decisions |
| May 1 | National Decision Day — commit | Decisions |

### Status Labels

- **Ahead** — milestone completed before expected semester
- **On Track** — completed in expected semester
- **Coming Up** — due this semester, not yet done
- **Behind** — past expected semester, not completed
- **N/A** — not yet relevant (future grade level)

Timeline is a guide, not rigid. "Behind" milestones framed as opportunities, not failures.

---

## Cross-Cutting Modules

In `references/cross-cutting.md`:

1. **Spike Development Module** — shared logic for building "well-lopsided" profile across `activities`, `plan`, `essays`, `summer`
2. **Narrative Threading Module** — ensures consistency across essays, activities, course selection (the cohesive story the application tells)
3. **Equity Check Module** — triggered across all commands to prevent bias, validate alternative paths (trades, CTE, gap years)
4. **Motivational Interviewing Module** — shared conversation techniques (open questions, rolling with resistance, "I don't know" is valid)

---

## Safety Protocol

In `references/safety-protocol.md`:

### Crisis Detection
- **Crisis keywords** (self-harm, suicide, abuse, "hopeless", "done with everything") → immediate warm referral to 988 Suicide & Crisis Lifeline + school counselor recommendation
- **Never attempt therapeutic intervention** — break from current topic, provide resources, encourage reaching out to trusted adult

### Normal Stress Handling
- **Test anxiety, application overwhelm, parental pressure** → acknowledge, normalize ("This is one of the most stressful things you'll go through in high school, and that's completely normal"), brief supportive response, redirect to actionable next step

### Guardrails
- **Reality check layer** — prevent validating harmful academic choices without pushback (e.g., dropping all AP courses without reason, academic dishonesty)
- **Never diagnose** — never label a student with anxiety, depression, ADHD, or any condition
- **Never replace professionals** — always position as a supplement to school counselors, not a replacement

---

## Research Foundation

This design is informed by six research documents in `research/`:

1. **high-school-counselor-research.md** — ASCA model, MTSS tiers, evidence-based counseling techniques
2. **eliciting-student-interests-research.md** — Holland Code, Ikigai, Flow Theory, Motivational Interviewing, question banks
3. **academic-tracks-and-planning-research.md** — 7 academic tracks, course sequences, testing strategy, spike model, trades/CTE
4. **application-process-and-admissions-research.md** — platforms, holistic review, essays, demonstrated interest, financial aid
5. **legal-and-safety-guardrails-research.md** — COPPA, FERPA, crisis protocols, Socratic-only essays, anti-bias
6. **market-landscape-and-competitor-research.md** — Naviance, Scoir, Cialfo, MaiaLearning; blue ocean opportunities
