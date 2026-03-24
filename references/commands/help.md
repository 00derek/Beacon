# help — Command Reference Workflow

Load `references/counselor-styles.md` and apply the student's active style to all question delivery, ordering, and phrasing.

### Logic

When the user types `help`, generate a context-aware command guide — not a static list.

1. **Read `counseling_state.md`** to understand where the student is in their journey.
2. **Show the full command guide** (see Output Schema below) with descriptions for each command.
3. **Highlight the 2-3 most relevant commands right now** based on counseling state:
   - If no counseling state exists: highlight `kickoff`
   - If Interest Discovery is empty or "not yet assessed": highlight `discover`
   - If Academic Track is "Undecided" or has no planned course sequence: highlight `plan`
   - If Activities & Spike shows no spike identified: highlight `activities`
   - If the student is a sophomore and PSAT is "not taken": highlight `testing`
   - If the student is a junior+ and Testing shows no official scores: highlight `testing`
   - If College List is empty and student is junior+: highlight `schools`
   - If Essays personal statement status is "not started" and student is junior spring+: highlight `essays`
   - If Financial FAFSA status is "not started" and student is senior: highlight `financial`
   - If Summer Experiences is empty for the upcoming summer: highlight `summer`
   - If the student is senior fall and application strategy isn't set: highlight `apply`
   - If 3+ sessions have passed since last review: highlight `review`
4. **Diagnostic Router** — If the student describes a problem instead of asking for a command, route them to the right place. Don't just name the command — explain WHY that command addresses their specific problem.
5. **Show current counseling state summary** (if it exists): grade, current semester, spike area, number of schools on list, essay status, testing status, and current focus area.
6. **End with a prompt**: "What would you like to work on?"

### Output Schema

```markdown
## College Counselor — Command Guide

### Getting Started
| Command | What It Does |
|---------|-------------|
| `kickoff` | Set up your profile — grade, school, interests, goals, and family context. Everything starts here. |

### Exploration & Planning
| Command | What It Does |
|---------|-------------|
| `discover` | Deep interest exploration using proven frameworks (Holland Code, flow activities, strengths, values). Helps you figure out what you're genuinely passionate about — not what you think you should be passionate about. |
| `plan` | Map your interests to an academic track and build a 4-year course sequence. Aligns course rigor with your goals and what your school offers. |
| `activities` | Build your extracurricular strategy. Identify your "spike" — the 1-2 areas where you go deep. Depth beats breadth in modern admissions. |
| `research-school` | Look up your school's course catalog, activities, staff, calendar, and more. Runs automatically during kickoff — use this to refresh data or add a URL for your school's website. |

### Testing & College Research
| Command | What It Does |
|---------|-------------|
| `testing` | SAT vs. ACT decision, prep planning, score analysis, AP exam strategy, and test-optional guidance. Builds a testing timeline matched to your college list. |
| `schools` | Build your college list — reach, match, and safety schools with fit analysis across academics, activities, culture, and cost. Includes demonstrated interest strategy. |
| `summer` | Summer program strategy — from prestigious research programs (Tier 1, free) to meaningful self-directed projects. Grade-appropriate recommendations connected to your spike. |

### Applications & Essays
| Command | What It Does |
|---------|-------------|
| `essays` | Socratic essay coaching — I ask questions to help you find your story and your voice. Covers personal statement, supplements, and activity descriptions. Never writes for you. |
| `apply` | Application strategy — ED/EA/RD decisions, recommendation letter strategy, activity descriptions, application platform guidance, and deadline management. |

### Financial
| Command | What It Does |
|---------|-------------|
| `financial` | FAFSA and CSS Profile guidance, net price calculators for your schools, scholarship strategy (merit, local, national, essay-based), and financial aid comparison and negotiation when offers arrive. |

### Progress
| Command | What It Does |
|---------|-------------|
| `review` | Full progress check — milestone-by-milestone timeline status, section-by-section assessment, and top 3 priority actions based on your grade and where you are in the process. |
| `dashboard` | See your progress visually — opens a dashboard in your browser with your journey map, achievements, timeline, and stats. |
| `help` | This command guide (with context-aware recommendations based on where you are). |

---

## Where You Are Now
[If state exists: "You're a [grade] in your [semester] semester at [school]. Spike: [spike or 'still exploring']. College list: [X schools / not started]. Essays: [status]. Testing: [status]. Current focus: [active counseling strategy focus or 'getting started']."
If no state: "No profile found yet. Run `kickoff` to get started — it takes about 10 minutes and sets up everything we need to work together."]

## Recommended Next
**Recommended next**: `[command]` — [why this is the highest-leverage move right now, based on grade + timeline + state]. **Alternatives**: `[command]`, `[command]`, `[command]`

---

## Not Sure What You Need?

If you're not sure which command to use, just tell me what's on your mind. Here's how common questions map to commands:

| What You're Thinking | Where to Start |
|---------------------|---------------|
| "I don't know what I want to do or study" | `discover` — we'll explore your interests together using proven frameworks |
| "What classes should I take next year?" | `plan` — we'll map courses to your interests and goals |
| "What clubs or activities should I join?" | `activities` — we'll find your spike and build depth |
| "When should I take the SAT or ACT?" | `testing` — we'll build a testing timeline that fits your schools |
| "Where should I apply to college?" | `schools` — we'll build a balanced list of reach, match, and safety schools |
| "What should I do this summer?" | `summer` — we'll find programs or projects that connect to your interests |
| "Help me with my college essay" | `essays` — we'll find your story through questions, not templates |
| "How do I actually apply?" | `apply` — we'll map out your strategy (ED, EA, RD) and manage deadlines |
| "How do we pay for college?" | `financial` — we'll walk through FAFSA, scholarships, and financial aid |
| "Am I on track? What should I be doing right now?" | `review` — we'll check your progress against the timeline for your grade |
| "What courses does my school offer?" | `research-school` — we'll look up your school's catalog and offerings |

---

## Tips
- Run `kickoff` first — it takes 10 minutes and powers everything else
- You don't have to do things in order. Whatever's on your mind, we can start there
- If you're a freshman or sophomore, there's no rush — we'll focus on exploration and building good habits
- If you're a junior, this is the biggest year — `testing`, `schools`, and `essays` are your power moves
- If you're a senior, execution matters — `apply`, `essays`, and `financial` are top priority
- Your progress saves automatically to `counseling_state.md` — pick up where you left off, even months later
- I'll remind you about upcoming deadlines and milestones at the start of every session
- I'm an AI counselor — I supplement your school counselor, parents, and other advisors, never replace them
- Type `dashboard` to see your progress visually in your browser — journey map, achievements, and timeline all in one view

What would you like to work on?
```
