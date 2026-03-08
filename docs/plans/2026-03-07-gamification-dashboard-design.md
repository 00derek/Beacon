# Beacon Gamification & Dashboard — Design

## Problem

Students (14-18) lose attention during long Q&A sessions and lack visual feedback on their multi-year college journey. The Socratic approach is pedagogically sound but feels like an interrogation without reward loops. Students try Beacon once but don't come back regularly, and they procrastinate because the process feels overwhelming.

## Solution

Two features that work together:

1. **Conversational Gamification** — make the chat itself more engaging with progress loops
2. **Visual Dashboard** — a `dashboard` command that generates a browser-viewable progress page

## Design Constraints

- `counseling_state.md` stays optimized for the AI agent — no visual formatting clutter
- Socratic approach, MI voice, safety protocol, equity check all unchanged
- Gamification layers on top of existing workflows, doesn't replace them
- Dashboard is read-only, no server, no dependencies

---

## Feature 1: Conversational Gamification

Four mechanics woven into existing command workflows:

### A. Progress Pulses

Every 3-4 questions, Beacon pauses and shows visual progress:

```
━━━━━━━━━━░░░░░ 60% through Discovery
🔓 Phase 2 complete: Flow Detection
```

Breaks Q&A monotony. Student sees they're getting somewhere.

### B. Micro-Summaries as Payoff

After every few questions, Beacon reflects back a small insight instead of saving synthesis for the end:

```
"Interesting — you keep coming back to building things
and leading teams. That's not random, that's a pattern."
```

Makes questions feel productive, not extractive.

### C. Session Chunking

Long workflows (especially `discover` and `essays`) break into named chapters:

```
"That's a good stopping point — you've finished Phase 2:
Flow Detection. Next up is Strengths Mapping.
Keep going or pick it up next time?"
```

Students opt in to more rather than enduring a marathon. Progress saves between chunks.

### D. Achievements

Completing milestones earns named achievements tracked in `counseling_state.md`:

```
## Achievements
- 🧬 Interest DNA — Completed full discovery (Mar 2026)
- 📐 Track Set — Chose Engineering path (Mar 2026)
- 🔥 Momentum x3 — 3 sessions in a row
- 🎯 Spike Identified — Found your focus area
- 📝 Story Seed — Personal statement theme found
```

Unlockable content tied to achievements:

- Completing `discover` unlocks `activities` spike workshop
- Building college list unlocks essay strategy insights
- Earning streak badges unlocks "deep dive" bonus questions

Achievement categories:

| Category | Examples |
|----------|----------|
| Journey milestones | Completed discovery, chose track, built college list |
| Streaks | 3, 5, 10 consecutive sessions |
| Depth | Reached state-level spike, completed all essay phases |
| Engagement | Asked a great question, revisited and updated interests |

---

## Feature 2: Dashboard Command

### How It Works

1. Student says `dashboard` in conversation
2. Beacon reads `counseling_state.md`
3. Generates a self-contained `dashboard.html` with all data embedded as a JS variable
4. Opens it in the default browser

### What the Dashboard Shows

**Journey Map** — horizontal progress visualization showing command completion:
```
🟢 Kickoff → 🟢 Discover → 🔵 Plan → ⚪ Activities → ⚪ Testing → ⚪ Schools → ⚪ Essays → ⚪ Apply
```

**Stats Block** — quick-glance profile:
- Sessions completed, current streak
- Academic track, GPA
- Timeline status (% on-track)
- Spike strength (visual bar)
- Next milestone and deadline

**Achievement Shelf** — all earned achievements with dates

**Timeline View** — upcoming milestones with status indicators (ahead / on-track / behind)

**College List** — reach / match / safety schools if populated

### Design Constraints

- Single HTML file, no external dependencies, no server
- Clean, modern CSS — mobile-friendly (teens will open on phones)
- Read-only — no editing state from the dashboard
- Re-run `dashboard` to refresh with current data
- Template file (`dashboard-template.html`) lives in the repo

---

## Changes to Existing Files

| File | Change |
|------|--------|
| `SKILL.md` | Add `dashboard` as 13th command. Add gamification instructions (progress pulses, micro-summaries, session chunking, achievements) to command workflows |
| `references/commands/` | New `dashboard.md` command reference |
| `references/commands/discover.md` | Add chapter breakpoints and progress pulse triggers |
| `references/commands/essays.md` | Add chapter breakpoints |
| `counseling_state.md` template | Add `## Achievements` section and `## Session Stats` (streak count, total sessions) |
| New: `dashboard-template.html` | HTML/CSS/JS template populated with state data |
| `VERSIONS.md` | Note gamification and dashboard in v2 |

## What Does NOT Change

- `counseling_state.md` stays agent-optimized markdown — no visual formatting
- Existing command workflows stay intact, gamification layers on top
- Socratic approach unchanged — just broken into rewarding chunks
- Safety protocol, equity check, MI voice all unchanged

---

## Future: Live Web Dashboard (Not Built Now)

A lightweight local server that watches `counseling_state.md` for changes and updates the browser in real-time. Could use file watching + WebSocket or simple polling. Worth exploring when the v4 web interface is on the roadmap. This would replace the snapshot-based `dashboard` command with a persistent, always-current view.
