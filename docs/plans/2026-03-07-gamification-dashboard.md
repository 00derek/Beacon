# Gamification & Dashboard Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add conversational gamification mechanics (progress pulses, micro-summaries, session chunking, achievements) and a `dashboard` command that generates a self-contained HTML progress page.

**Architecture:** Gamification is layered into existing command workflows via prompt instructions in SKILL.md and individual command references. The dashboard is a new command that reads `counseling_state.md`, generates a self-contained `dashboard.html` with embedded data, and opens it in the browser. No server, no dependencies.

**Tech Stack:** Markdown (prompt engineering), HTML/CSS/JS (dashboard template)

---

### Task 1: Add Achievements and Session Stats to counseling_state.md Template

**Files:**
- Modify: `SKILL.md:85-223` (counseling_state.md format section)

**Step 1: Add two new sections to the counseling_state.md template**

In `SKILL.md`, find the `counseling_state.md` format section. After the `## Style Adaptation Log` section (line ~222) and before the closing triple backticks, add:

```markdown
## Session Stats
- Total sessions: 0
- Current streak: 0
- Longest streak: 0
- Last session date: [date]

## Achievements
[Achievements are earned by completing milestones, maintaining streaks, and progressing through the college journey. Format: emoji + name + description + date earned.]
```

**Step 2: Add achievement and stats update triggers**

In the "State Update Triggers" section of `SKILL.md` (after line ~239), add a new trigger entry:

```markdown
- **All commands**: After every command execution, update Session Stats (increment total sessions, update streak based on whether last session was within 14 days, update last session date). Check achievement conditions and award any newly earned achievements to the Achievements section. Never remove earned achievements.
```

**Step 3: Commit**

```bash
git add SKILL.md
git commit -m "feat: add achievements and session stats sections to counseling state template"
```

---

### Task 2: Define the Achievement Catalog in SKILL.md

**Files:**
- Modify: `SKILL.md` (add after Non-Negotiable Operating Rules section, before Equity Check)

**Step 1: Add the achievement catalog section**

After the Non-Negotiable Operating Rules (rule 13, around line ~259), add a new section:

```markdown
## Achievement System

Achievements reward progress and engagement. Award them silently during state saves — don't interrupt the conversation flow. When an achievement is newly earned, mention it briefly at the next natural pause point: "By the way — you just earned [achievement name]. [one-line description]."

### Achievement Catalog

#### Journey Milestones
| Achievement | Trigger | Description |
|-------------|---------|-------------|
| 🚀 Launched | Complete `kickoff` | You've started your college journey |
| 🧬 Interest DNA | Complete all 6 phases of `discover` | Your interest profile is mapped |
| 📐 Track Set | Choose an academic track in `plan` | Your academic direction is locked in |
| 🎯 Spike Found | Identify a spike area in `activities` | You've found your focus |
| 📊 Score Check | Complete first testing strategy in `testing` | Your testing plan is in place |
| 🏫 List Built | Build a complete college list in `schools` | Reach, match, and safety — you're covered |
| 📝 Story Seed | Identify a personal statement theme in `essays` | Your essay story is taking shape |
| 📬 App Ready | Complete application strategy in `apply` | Your application plan is set |
| 💰 Money Smart | Complete financial aid strategy in `financial` | You know your financial options |
| ☀️ Summer Planned | Complete summer planning in `summer` | Your summer has a purpose |

#### Streaks
| Achievement | Trigger | Description |
|-------------|---------|-------------|
| 🔥 Momentum x3 | 3 sessions within 14-day windows | You're building momentum |
| 🔥 Momentum x5 | 5 sessions within 14-day windows | Consistent effort pays off |
| 🔥 Momentum x10 | 10 sessions within 14-day windows | You're in the zone |

#### Depth
| Achievement | Trigger | Description |
|-------------|---------|-------------|
| ⬆️ Level Up | Activity reaches State tier (Tier 2) | Your spike is gaining recognition |
| 🏆 Top Tier | Activity reaches National tier (Tier 1) | National-level achievement unlocked |
| ✍️ Draft Done | Personal statement reaches "drafting" status | Words on the page — that's the hardest part |
| 🎓 Full Picture | Narrative thread connects spike + courses + activities + essays | Your application tells a cohesive story |

#### Engagement
| Achievement | Trigger | Description |
|-------------|---------|-------------|
| 🔄 Fresh Eyes | Return to `discover` after 3+ months to reassess | Growth means your interests evolve |
| 📋 Check-In Pro | Run `review` 3 times across different semesters | You're staying on top of your timeline |
```

**Step 2: Commit**

```bash
git add SKILL.md
git commit -m "feat: add achievement catalog with journey, streak, depth, and engagement categories"
```

---

### Task 3: Add Gamification Mechanics to SKILL.md

**Files:**
- Modify: `SKILL.md` (add after Achievement System section)

**Step 1: Add the gamification mechanics section**

After the Achievement System section, add:

```markdown
## Gamification Mechanics

Four mechanics keep students engaged across sessions. These layer on top of existing workflows — they don't replace them.

### Progress Pulses

Every 3-4 questions within a multi-question workflow (especially `discover`, `essays`, `kickoff`), pause and show a visual progress indicator:

```
━━━━━━━━━━░░░░░░░░░░ 50% through Discovery
🔓 Phase 2 complete: Flow Detection
```

Progress pulses break Q&A monotony and show the student they're making progress. Keep them to 2 lines max — don't derail the conversation.

### Micro-Summaries

After every few questions, reflect back a small insight instead of saving all synthesis for the end:

- "Interesting — you keep coming back to building things and leading teams. That's not random, that's a pattern."
- "I'm noticing you light up when you talk about design but go quiet around pure math. That's useful information."

Micro-summaries make questions feel productive. The student sees that their answers are going somewhere, not just being extracted.

### Session Chunking

Long workflows (`discover` has 6 phases, `essays` has 5 steps) break into named chapters. At the end of each chapter, offer a natural stopping point:

- "That's a good stopping point — you've finished Phase 2: Flow Detection. Next up is Strengths Mapping. Keep going or pick it up next time?"

If the student stops mid-workflow, save progress to `counseling_state.md` so they can resume. When they return, pick up where they left off: "Last time we finished [chapter]. Ready to continue with [next chapter]?"

### Unlockable Content

Completing certain milestones opens new capabilities. Mention these naturally:

- Completing `discover` → "Now that I know your interests, we can do something cool — let's build your spike strategy in `activities`."
- Building a college list → "With your schools identified, I can give you much sharper essay guidance — the 'Why This School' supplementals will actually have teeth now."
- Earning streak achievements → "Your consistency is paying off. Want to try a deep-dive session where we go further than usual on [topic]?"

Unlockables create forward momentum. They make the next command feel like a reward, not a chore.
```

**Step 2: Commit**

```bash
git add SKILL.md
git commit -m "feat: add gamification mechanics — progress pulses, micro-summaries, session chunking, unlockables"
```

---

### Task 4: Add Chapter Breakpoints to discover.md

**Files:**
- Modify: `references/commands/discover.md`

**Step 1: Add progress pulse and chapter markers to each phase**

At the beginning of `discover.md`, after the first paragraph and before `### Phase 1`, add:

```markdown
### Session Chunking

The discover workflow has 6 phases. Each phase is a chapter. At the end of each phase, show a progress pulse and offer a stopping point. If the student stops mid-workflow, save completed phases to `counseling_state.md` (Interest Discovery section — note which phases are complete) so they can resume next session.

When resuming: "Last time we finished [phase name]. Ready to continue with [next phase name]?"
```

At the end of Phase 1 (after the `**Transition:**` line), add:

```markdown
**Progress pulse:**
```
━━━░░░░░░░░░░░░░░░░░ 15% through Discovery
🔓 Phase 1 complete: Warm-Up
```
```

At the end of Phase 2, add:

```markdown
**Progress pulse:**
```
━━━━━━░░░░░░░░░░░░░░ 33% through Discovery
🔓 Phase 2 complete: Flow Detection
```
"That was great — we've identified what genuinely energizes you. Want to keep going into Strengths Mapping, or pick it up next time?"
```

At the end of Phase 3, add:

```markdown
**Progress pulse:**
```
━━━━━━━━━━░░░░░░░░░░ 50% through Discovery
🔓 Phase 3 complete: Strengths Mapping
```
**Micro-summary:** Reflect back 1-2 strengths the student didn't recognize: "You might not think of [X] as a strength, but it absolutely is — and it connects to [flow activity]."

"Halfway through! Want to continue with Values Clarification, or save this for next time?"
```

At the end of Phase 4, add:

```markdown
**Progress pulse:**
```
━━━━━━━━━━━━━░░░░░░░ 66% through Discovery
🔓 Phase 4 complete: Values Clarification
```
"We've got your interests, strengths, and values mapped. Two more phases to go — Possibility Expansion and Synthesis. Keep going?"
```

At the end of Phase 5, add:

```markdown
**Progress pulse:**
```
━━━━━━━━━━━━━━━━░░░░ 83% through Discovery
🔓 Phase 5 complete: Possibility Expansion
```
"Almost there — one more phase to pull it all together."
```

At the end of Phase 6 (before the Output Schema), add:

```markdown
**Progress pulse:**
```
━━━━━━━━━━━━━━━━━━━━ 100% Discovery Complete!
🏆 Achievement unlocked: 🧬 Interest DNA
```
```

**Step 2: Commit**

```bash
git add references/commands/discover.md
git commit -m "feat: add chapter breakpoints and progress pulses to discover workflow"
```

---

### Task 5: Add Chapter Breakpoints to essays.md

**Files:**
- Modify: `references/commands/essays.md`

**Step 1: Add session chunking note and progress pulses**

At the beginning of `essays.md`, after the Socratic-Only Enforcement section (after the "When the student asks you to write for them" block, before `### Step 1`), add:

```markdown
### Session Chunking

Essay coaching sessions can be long and emotionally demanding. Each step is a chapter. At the end of Steps 1-3, offer a stopping point. Save progress to `counseling_state.md` (Essays section — note which step the student is on for this essay) so they can resume.

When resuming: "Last time we were working on your [essay type] and got through [step]. Ready to pick up with [next step]?"
```

At the end of Step 1 (after the student picks an essay type), add:

```markdown
**Progress pulse:**
```
━━━━░░░░░░░░░░░░░░░░ 20% — Essay type selected
```
```

At the end of Step 2 (after topic discovery), add:

```markdown
**Progress pulse:**
```
━━━━━━━━░░░░░░░░░░░░ 40% — Topic identified
🔓 Step 2 complete: Topic Discovery
```
**Micro-summary:** "Here's what I'm hearing as your story: [1-2 sentence reflection of the topic and why it matters]."

"Finding your topic is the hardest part — and you just did it. Want to keep going into development, or let it marinate and come back?"
```

At the end of Step 3 (after development/outlining), add:

```markdown
**Progress pulse:**
```
━━━━━━━━━━━━░░░░░░░░ 60% — Outline taking shape
🔓 Step 3 complete: Development
```
"You've got a topic and a structure. The next step is writing your draft on your own, then bringing it back for feedback. Take your time with the draft — there's no rush."
```

At the end of Step 5 (after draft review), add:

```markdown
**Progress pulse:**
```
━━━━━━━━━━━━━━━━━━━━ 100% — Essay coaching session complete
```
If the personal statement theme was identified for the first time:
```
🏆 Achievement unlocked: 📝 Story Seed
```
```

**Step 2: Commit**

```bash
git add references/commands/essays.md
git commit -m "feat: add chapter breakpoints and progress pulses to essays workflow"
```

---

### Task 6: Create the dashboard Command Reference

**Files:**
- Create: `references/commands/dashboard.md`

**Step 1: Write the dashboard command reference**

```markdown
# dashboard — Visual Progress Dashboard

### Purpose

Generate a self-contained HTML dashboard that visualizes the student's college journey progress. Opens in the default browser.

### Prerequisites

- `counseling_state.md` must exist (run `kickoff` first)

### Workflow

1. **Read `counseling_state.md`** — parse all sections
2. **Read `dashboard-template.html`** — load the HTML template
3. **Embed state data** — inject the parsed state as a JavaScript object into the template
4. **Write `dashboard.html`** — save the generated file to the project root
5. **Open in browser** — run `open dashboard.html` (macOS) or equivalent

### Data Extraction

Parse the following from `counseling_state.md` and embed as a JSON object assigned to `window.BEACON_DATA`:

```javascript
window.BEACON_DATA = {
  profile: {
    name: "",
    grade: "",
    school: "",
    gpaUnweighted: "",
    gpaWeighted: "",
    counselorStyle: "",
    currentSemester: "",
    targetGradYear: ""
  },
  sessionStats: {
    totalSessions: 0,
    currentStreak: 0,
    longestStreak: 0,
    lastSessionDate: ""
  },
  journeyProgress: {
    kickoff: "complete" | "not-started",
    discover: "complete" | "in-progress" | "not-started",
    plan: "complete" | "in-progress" | "not-started",
    activities: "complete" | "in-progress" | "not-started",
    testing: "complete" | "in-progress" | "not-started",
    schools: "complete" | "in-progress" | "not-started",
    essays: "complete" | "in-progress" | "not-started",
    apply: "complete" | "in-progress" | "not-started",
    financial: "complete" | "in-progress" | "not-started",
    summer: "complete" | "in-progress" | "not-started"
  },
  achievements: [
    { emoji: "", name: "", description: "", date: "" }
  ],
  timeline: [
    { milestone: "", status: "ahead|on-track|coming-up|behind", notes: "" }
  ],
  spike: {
    areas: [],
    strength: "" // description like "School-level, working toward State"
  },
  collegeList: {
    reach: [{ name: "", strategy: "" }],
    match: [{ name: "", strategy: "" }],
    safety: [{ name: "", strategy: "" }]
  },
  essays: {
    personalStatement: { status: "", theme: "" },
    supplementals: [{ school: "", status: "" }]
  }
};
```

### Determining Journey Progress

Use these rules to determine command completion status:

| Command | Complete When | In-Progress When |
|---------|--------------|------------------|
| `kickoff` | Profile section is populated | — |
| `discover` | Holland Code is not "not yet assessed" AND flow activities listed AND strengths listed AND values listed | Any of these exist but not all |
| `plan` | Primary track is set AND planned course sequence has entries | Primary track set but no courses |
| `activities` | Spike area(s) identified AND activity list has entries | Either exists but not both |
| `testing` | Test preference set AND (official scores exist OR prep plan is set) | Any testing data exists |
| `schools` | College list has entries in all 3 categories (reach, match, safety) | Any schools listed |
| `essays` | Personal statement status is "drafting" or beyond | Any essay work started |
| `apply` | Application strategies set for schools in college list | Any strategy data exists |
| `financial` | FAFSA status is not "not started" | Any financial data exists |
| `summer` | Summer experiences table has entries | — |

### Output

After generating and opening the dashboard:

"Your dashboard is open in the browser. It shows your current progress, achievements, and timeline status. Run `dashboard` anytime to refresh it with your latest data."

No state update needed — this command is read-only.
```

**Step 2: Commit**

```bash
git add references/commands/dashboard.md
git commit -m "feat: add dashboard command reference with data extraction spec"
```

---

### Task 7: Create the Dashboard HTML Template

**Files:**
- Create: `dashboard-template.html`

**Step 1: Write the HTML template**

Create `dashboard-template.html` in the project root. This is a single self-contained HTML file with embedded CSS and JS. The JS reads from `window.BEACON_DATA` (which will be injected by the dashboard command).

The template should include:

**Header:**
- Student name, grade, school
- Session stats (total sessions, current streak)
- Last updated date

**Journey Map:**
- Horizontal progress bar showing all 10 commands
- Each command shown as a node: green (complete), blue (in-progress), gray (not started)
- Connected by a line/arrow

**Stats Block:**
- Academic track (if set)
- GPA
- Spike areas + strength indicator (visual bar)
- Timeline: X% on-track (calculated from timeline milestones)
- Next milestone + status

**Achievement Shelf:**
- Grid of earned achievements
- Each achievement: emoji + name + date
- Unearned achievements shown as locked (grayed out with 🔒)

**Timeline View:**
- List of milestones grouped by status
- Color-coded: green (ahead), blue (on-track), yellow (coming-up), red (behind)

**College List (if populated):**
- Three columns: Reach, Match, Safety
- School names with strategy labels

**Design requirements:**
- Mobile-first responsive CSS
- Dark mode support via `prefers-color-scheme`
- Clean, modern design — think Linear or Notion aesthetic
- No external dependencies (fonts use system font stack)
- Accessible: proper heading hierarchy, ARIA labels, color contrast

The file should be a complete, working HTML page. The `<!-- BEACON_DATA -->` comment marker will be replaced with the actual `<script>window.BEACON_DATA = {...}</script>` block by the dashboard command.

**Step 2: Verify the template renders correctly**

Open `dashboard-template.html` in a browser. It should render with placeholder/empty state (no data). The layout, colors, and responsiveness should all work.

**Step 3: Commit**

```bash
git add dashboard-template.html
git commit -m "feat: add self-contained dashboard HTML template"
```

---

### Task 8: Register the dashboard Command in SKILL.md

**Files:**
- Modify: `SKILL.md:289-322` (Command Registry and File Routing sections)

**Step 1: Add dashboard to the command registry table**

In the Command Registry table (around line 290), add a new row:

```markdown
| `dashboard` | Generate visual progress dashboard and open in browser |
```

**Step 2: Add file routing entry**

In the File Routing section (around line 310), add:

```markdown
- **`dashboard`**: Also read `references/commands/dashboard.md` (for data extraction spec and generation workflow), `dashboard-template.html` (for the HTML template to populate).
```

**Step 3: Add dashboard to the help command output**

In `references/commands/help.md`, add `dashboard` to the Progress section of the command table:

```markdown
| `dashboard` | See your progress visually — opens a dashboard in your browser with your journey map, achievements, timeline, and stats. |
```

**Step 4: Commit**

```bash
git add SKILL.md references/commands/help.md
git commit -m "feat: register dashboard command in skill definition and help"
```

---

### Task 9: Add Gamification to kickoff.md

**Files:**
- Modify: `references/commands/kickoff.md`

**Step 1: Add progress pulses to kickoff workflow**

After Step 2 (Profile Collection), before Step 3, add:

```markdown
**Progress pulse:**
```
━━━━━━━━━━░░░░░░░░░░ 50% through Kickoff
🔓 Profile complete
```
```

After Step 4 (Grade-Appropriate Context), before Step 5, add:

```markdown
**Progress pulse:**
```
━━━━━━━━━━━━━━━━━━━━ 100% Kickoff Complete!
🏆 Achievement unlocked: 🚀 Launched
```
```

**Step 2: Add dashboard mention to kickoff summary**

At the end of the Kickoff Summary output schema (after the `**Recommended next**` line), add:

```markdown
💡 **Tip**: Type `dashboard` anytime to see your progress visually in a browser.
```

**Step 3: Commit**

```bash
git add references/commands/kickoff.md
git commit -m "feat: add progress pulses and achievement announcement to kickoff"
```

---

### Task 10: Update State Update Triggers for Achievements

**Files:**
- Modify: `SKILL.md` (State Update Triggers section)

**Step 1: Add achievement-aware triggers to existing command triggers**

Update the following existing trigger entries to include achievement checks. For each command trigger, append the achievement that should be checked:

After `kickoff` trigger: add `Check achievement: 🚀 Launched.`
After `discover` trigger: add `Check achievement: 🧬 Interest DNA (all 6 phases complete).`
After `plan` trigger: add `Check achievement: 📐 Track Set (primary track chosen).`
After `activities` trigger: add `Check achievement: 🎯 Spike Found (spike area identified). Check ⬆️ Level Up and 🏆 Top Tier based on activity tiers.`
After `testing` trigger: add `Check achievement: 📊 Score Check (testing strategy complete).`
After `schools` trigger: add `Check achievement: 🏫 List Built (all 3 categories populated).`
After `essays` trigger: add `Check achievement: 📝 Story Seed (personal statement theme identified). Check ✍️ Draft Done (personal statement "drafting" or beyond).`
After `apply` trigger: add `Check achievement: 📬 App Ready (application strategy complete).`
After `financial` trigger: add `Check achievement: 💰 Money Smart (financial aid strategy complete).`
After `summer` trigger: add `Check achievement: ☀️ Summer Planned.`
After `review` trigger: add `Check achievement: 📋 Check-In Pro (3 reviews across different semesters).`

Also add: `After discover, if last discover was 3+ months ago: Check achievement: 🔄 Fresh Eyes.`

**Step 2: Add streak tracking logic**

After the achievement trigger updates, add to the general "All commands" trigger:

```markdown
- **All commands (session stats)**: Increment total sessions by 1. If last session date is within 14 days, increment current streak by 1; otherwise reset current streak to 1. Update longest streak if current streak exceeds it. Update last session date to today. Check streak achievements (🔥 Momentum x3, x5, x10).
```

**Step 3: Add narrative thread achievement check**

Add: `When updating the narrative thread in the Essays section, check if spike + courses + activities + essays are all connected. If so, award 🎓 Full Picture.`

**Step 4: Commit**

```bash
git add SKILL.md
git commit -m "feat: add achievement and streak tracking to state update triggers"
```

---

### Task 11: Update VERSIONS.md

**Files:**
- Modify: `VERSIONS.md`

**Step 1: Add gamification and dashboard to v1 feature list**

At the end of the v1 bullet list, add:

```markdown
- **Gamification**: Progress pulses every 3-4 questions, micro-summaries for real-time insight, session chunking with named chapters and natural stopping points, achievement system with journey milestones, streaks, depth, and engagement badges.
- **Dashboard command**: `dashboard` generates a self-contained HTML page showing journey progress, achievements, timeline status, stats, and college list. Opens in the browser — no server needed.
```

**Step 2: Add future live dashboard note to v4**

In the v4 section, add:

```markdown
- **Live dashboard**: Real-time browser dashboard that watches `counseling_state.md` for changes and auto-updates. Replaces the snapshot-based `dashboard` command with a persistent, always-current view using file watching and WebSocket or polling.
```

**Step 3: Commit**

```bash
git add VERSIONS.md
git commit -m "docs: update version roadmap with gamification and dashboard features"
```

---

### Task 12: Add dashboard to .gitignore

**Files:**
- Modify: `.gitignore` (or create if it doesn't exist)

**Step 1: Add generated dashboard file to gitignore**

The `dashboard.html` file is generated per-student and should not be committed. Add:

```
# Generated dashboard (student-specific)
dashboard.html
```

Note: `dashboard-template.html` should NOT be gitignored — it's the template that ships with the project.

**Step 2: Commit**

```bash
git add .gitignore
git commit -m "chore: gitignore generated dashboard.html"
```
