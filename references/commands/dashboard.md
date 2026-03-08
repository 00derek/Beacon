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

~~~javascript
window.BEACON_DATA = {
  profile: {
    name: "",
    grade: "",
    school: "",
    gpaUnweighted: "",
    gpaWeighted: "",
    counselorStyle: "",
    academicTrack: "",
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
    strength: ""
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
~~~

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
