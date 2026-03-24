# School Research Integration — Design Spec

**Date**: 2026-03-23
**Status**: Draft (Rev 2 — post spec review)
**Summary**: Automatically research a student's school during kickoff to populate a reusable school profile with course catalog, staff, calendar, extracurriculars, and more — enabling school-aware recommendations and proactive date reminders.

---

## Problem

Beacon currently stores the student's school as a plain text string and relies on asking students "does your school offer AP X?" during planning. This means:

- Course recommendations are uninformed until the student manually confirms availability
- School-specific context (pathways, prerequisites, restrictions like "no AP for 9th graders") is discovered late or missed entirely
- Key school dates (course selection deadlines, AP sign-ups) are invisible to the timeline engine
- Every session that needs school data requires redundant conversation

## Solution Overview

Add a `research-school` command backed by a reference module that:

1. Triggers automatically during kickoff as a blocking research step after the student provides their school name
2. Runs targeted web searches and page fetches to gather school data
3. Saves structured findings to a **separate school profile file**
4. Integrates key dates with the existing timeline engine
5. Can be invoked on-demand for refreshes, URL-based updates, or school changes

## Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| When to research | During kickoff, blocking step after school name | Data ready before course load questions; ensures availability |
| Execution model | Blocking research step with progress message | Beacon is a single-threaded LLM skill; true background execution is unreliable. A brief "Let me look up your school" pause is acceptable UX since it only happens once during kickoff. |
| Where to store | Separate file (`school-profile-[slug].md`) in project root, gitignored | Keeps state lean; generated data shouldn't be version-controlled |
| What to research | All 8 categories (see below) | Comprehensive school context enables multiple commands |
| Date integration | Source in school file, relevant dates copied to student timeline | Single source of truth + grade-filtered surfacing |
| Gap handling | Flag gaps, fall back to conversational, accept student URLs | Graceful degradation with path to fill gaps |

---

## 1. Research Trigger & Flow

### During Kickoff

After the student answers "What school do you go to?" (Step 2: Profile Collection, question 2 in kickoff.md):

1. Beacon says: "Great — let me look up [School Name] so I can give you school-specific recommendations. This'll take a moment."
2. Beacon runs the research workflow (Phase 1-4, see Section 2). Target: under 60 seconds.
3. Beacon reports a brief summary: "Found [School]'s course catalog, activities, and calendar. A few things I couldn't find — I'll ask you about those as we go."
4. Beacon resumes kickoff at the next profile question (GPA — question 3 within Step 2), now informed by school data
5. When asking about current course load (question 5 within Step 2), Beacon can reference actual offerings: "I see Carlmont offers AP Physics C — are you taking that?"

**If the school profile file already exists** (e.g., from a prior student or a previous kickoff attempt), Beacon reads the existing file instead of re-researching. If the file is >6 months old, Beacon asks: "I have school data from [date] — want me to refresh it?"

### On-Demand Usage

The `research-school` command can be invoked directly. Like other Beacon commands, it uses natural language detection (not formal subcommand syntax):

- **"research-school"** or **"refresh my school info"**: Full re-research (useful for new school year)
- **"research-school [URL]"** or **"here's our course catalog: [URL]"**: Fetch a specific URL and update the profile
- **"research-school calendar"** or **"can you look up my school's calendar?"**: Re-research a specific category

Use cases:
- Student transfers to a new school
- New school year with updated catalog/calendar
- Student provides a URL for data Beacon couldn't find
- Refreshing stale data (>6 months old)

---

## 2. Research Strategy & Data Collection

### Phase 1 — Discovery (WebSearch)

Run 4-5 targeted searches (use the current calendar year from system date for `[current year]`):

1. `"[School Name] [State] course catalog [current year]"`
2. `"[School Name] [State] AP honors classes offered"`
3. `"[School Name] [State] clubs extracurriculars athletics"`
4. `"[School Name] [State] academic calendar [current year]"`
5. `"[School Name] [State] staff directory counselors"`

**Search budget**: Limit to 5 WebSearch calls and 8 WebFetch calls total. If a fetch times out or returns unhelpful content, skip it and mark that category as a gap rather than retrying.

### Phase 2 — Deep Fetch (WebFetch)

From search results, identify the school's official website and fetch key pages:

- Academics / course catalog page
- Activities / clubs page
- Calendar page
- Staff directory page
- Graduation requirements or pathway pages

### Phase 3 — Structured Extraction

Parse fetched content into the 8 data categories:

1. **Course catalog** — AP, Honors, regular courses organized by department
2. **Course pathways & prerequisites** — e.g., "Pre-Calc Honors requires Algebra II/Trig"
3. **Graduation requirements** — minimum credits, required courses by category
4. **Staff directory** — counselors (with caseload assignments if found), key department heads
5. **Extracurriculars** — clubs, sports, arts, competitions
6. **Special programs** — dual enrollment, CTE pathways, IB, magnet programs
7. **Key dates** — registration deadlines, course selection windows, AP sign-ups
8. **School profile** — size, demographics, college-going rate, location

### Phase 4 — Gap Assessment

For each category, mark as:

- **Found** — data captured with source URL
- **Partial** — some data found, gaps noted
- **Not found** — flag with suggestion to ask school counselor or provide URL

### Complete Failure Handling

If all searches return zero useful results (e.g., misspelled school name, no web presence, international school):

1. Create a minimal school profile with all 8 categories marked "Not found"
2. Inform the student: "I wasn't able to find information about [School Name] online. No worries — I'll ask you about courses and activities as we go. If you have a link to your school's website, you can share it anytime and I'll pull the details."
3. Proceed with kickoff using the current conversational approach for all school-related questions
4. The minimal profile still serves as a container for student-provided data added later

**Tool unavailability**: If WebSearch or WebFetch tools are not available in the current environment, skip the research step entirely and proceed as if all categories returned "Not found" (same as complete failure path above).

---

## 3. School Profile File Format

### Naming Convention

`school-profile-[slug].md`

**Slug generation rules** (deterministic):
1. Take the school name as provided by the student
2. Lowercase all characters
3. Strip trailing suffixes in this order (longest first, trailing only): "preparatory school", "high school", "middle school", "academy", "preparatory", "school"
4. Trim whitespace, replace spaces with hyphens
5. Strip any remaining non-alphanumeric characters (except hyphens)
6. Append the state abbreviation (lowercase) for disambiguation

Examples:
- "Carlmont High School" (CA) → `school-profile-carlmont-ca.md`
- "Thomas Jefferson Academy" (VA) → `school-profile-thomas-jefferson-va.md`
- "Palo Alto High School" (CA) → `school-profile-palo-alto-ca.md`

**Storage location**: Project root alongside `counseling_state.md`. Add `school-profile-*.md` to `.gitignore` since these are generated files (like `dashboard.html`).

### State File Reference

Add to the Profile section of `counseling_state.md`:

```
School Profile: school-profile-carlmont-ca.md
```

This is a plain filename reference (not a wiki-link). When Beacon encounters this field during session start, it reads the referenced file to load school context. If the file doesn't exist, Beacon proceeds without school data and suggests running `research-school`.

### File Structure

```markdown
# School Profile: Carlmont High School (CA)

Last Updated: 2026-03-23
Sources: [list of URLs fetched]

## School Overview
- Type: Public
- Location: Belmont, CA
- Enrollment: ~2,100 students
- College-going rate: 94%

## Course Catalog
### Math
- Algebra I, Geometry, Algebra II, Pre-Calculus, AP Calculus AB, AP Calculus BC...
### Science
- Biology, Chemistry, AP Physics C...
### English
[...by department]

## Course Pathways & Prerequisites
- Pre-Calc Honors → requires Algebra II/Trig or placement test (MIRA score)
- AP Physics C → requires concurrent Calc AB
- AP Chemistry → requires Chemistry + Algebra II
[...key prerequisite chains]

## Graduation Requirements
- 220 credits total
- 4 years English, 3 years Math, 2 years Science...
[...by category]

## AP & Honors Offerings
[Summary list with restrictions]
- No AP courses available to 9th graders
- 15 AP courses offered total
[...specific offerings]

## Special Programs
- Dual enrollment: Canada College partnership
- CTE pathways: Engineering, Digital Media
[...details]

## Extracurriculars
### Clubs
- Robotics, Debate, Model UN...
### Athletics
- Football, Soccer, Track...
### Arts
- Orchestra, Theater, Visual Arts...
### Competitions
- Science Olympiad, Math League...

## Staff Directory
### Counselors
- [Name] — Last names A-G
- [Name] — Last names H-N
[...if found]
### Department Heads
- Math: [Name]
- Science: [Name]

## Key Dates
| Date | Event | Relevant To |
|------|-------|-------------|
| 2026-04-15 | Course selection opens | Sophomores, Juniors |
| 2026-05-01 | Course selection deadline | All students |
| 2026-05-06 | AP Exams begin | AP students |
| 2026-08-19 | First day of school | All students |

## Research Gaps
- [ ] CTE pathway details — ask school counselor
- [ ] Club meeting schedules — not found online
- [ ] Counselor caseload assignments — not available
```

---

## 4. Key Dates Integration with Timeline Engine

### How It Works

1. **Source of truth**: School-specific dates live in the school profile's Key Dates section

2. **Session start protocol integration**: The existing session start protocol (SKILL.md) follows this sequence: read state → check grade + date → surface milestones → recommend action. The school profile read happens **immediately after reading `counseling_state.md`** (step 1), so school dates are available before the milestone check:
   - Read `counseling_state.md`
   - **Read school profile file** (referenced in Profile section)
   - Check grade + current date against timeline engine
   - Merge school dates into milestone surfacing (see step 3)
   - Surface 2-3 upcoming/overdue milestones (now includes school dates)
   - Recommend next action

3. **Grade-filtered surfacing**: Only dates relevant to the student's grade and track get copied into the student's Timeline Status in `counseling_state.md`. A new `Source` column is **added alongside** the existing `Notes` column (not replacing it):

```
| Milestone | Expected | Status | Source | Notes |
|-----------|----------|--------|--------|-------|
| PSAT Registration | 2026-09-15 | upcoming | timeline-engine | |
| Course Selection Opens | 2026-04-15 | !! 5 days | school-calendar | Register via student portal |
| AP Exam Sign-up Deadline | 2026-04-20 | !! 2 days | school-calendar | See counselor for fee waivers |
```

   The `Source` column is a new addition to the existing schema (`Milestone | Expected | Status | Notes` becomes `Milestone | Expected | Status | Source | Notes`). Existing milestones from the timeline engine are tagged `timeline-engine`; school-specific dates are tagged `school-calendar`. The status column uses the existing timeline engine icons (`>>` ahead, `--` on-track, `..` coming-up, `!!` urgent).

4. **Proactive reminders**: During the session start milestone check, Beacon compares the current date against school Key Dates. Any date within 14 days gets `!!` status and is surfaced in the opening message — e.g., "Heads up — Carlmont's course selection window opens in 5 days. Want to finalize your course plan today?" This calculation happens in the session start protocol, not in the timeline engine itself.

5. **Dashboard**: School dates appear on the dashboard timeline alongside universal milestones, visually distinguished (different color or tag)

6. **Staleness**: Two staleness triggers, checked independently:
   - **Age-based**: If the school profile's "Last Updated" date is >6 months old, suggest a refresh at session start.
   - **School-year boundary**: If the profile was last updated before August 1 of the current school year (i.e., data is from a prior school year), suggest a refresh — even if it's less than 6 months old. This catches spring-semester calendar data going stale when a new school year begins.

---

## 5. Gap Handling & Student-Provided URLs

### Three Fallback Layers

1. **Flag in school profile** — the Research Gaps section tracks what wasn't found, so Beacon knows not to assume completeness

2. **Fall back to conversational approach** — for missing categories, Beacon uses the current pattern of asking the student directly. E.g., if course catalog wasn't found: "I couldn't find your school's full course catalog online. What AP/Honors courses does your school offer?"

3. **Accept student-provided URLs** — at any point, the student can say "here's our course catalog: [URL]" and Beacon will:
   - Fetch the URL via WebFetch
   - Extract relevant data
   - Update the school profile file
   - Clear the corresponding gap from Research Gaps

### Staleness & Refresh

- **Last Updated** date at the top of the school profile indicates freshness
- If data is >6 months old, Beacon suggests a refresh at session start
- Student or counselor can trigger `research-school` at any time for a full refresh

---

## 6. Files Modified & Created

### Modified Files

| File | Change |
|------|--------|
| `references/commands/kickoff.md` | After step 2 (school name), run blocking school research. Add logic to weave school data into subsequent questions, especially step 5 (current course load). |
| `SKILL.md` | Add school profile to managed files list. Add `research-school` to command list. Update session-start protocol to read school profile after state (see Section 4). Add `School Profile:` field to the Profile section of the `counseling_state.md` schema. Add `Source` column to Timeline Status schema. Add `school-profile-*.md` to gitignore instructions. Add mid-session save for school profile updates from student-provided URLs. |
| `references/timeline-engine.md` | Add `Source` column convention (`timeline-engine` / `school-calendar`) for distinguishing milestone origins. Add guidance for merging school-specific dates during session start. Define 14-day `!!` urgency threshold for school dates. |
| `references/commands/plan.md` | Check school profile for course availability before asking student. Only ask conversationally if data is missing or marked as a gap. |
| `references/commands/activities.md` | Reference school profile's extracurriculars section when advising on activities strategy. |
| `references/commands/testing.md` | Reference school profile's AP offerings when recommending AP exam prep targets. If school offers limited APs, adjust testing strategy accordingly. |
| `references/commands/summer.md` | Reference school profile's Special Programs section for dual enrollment options and school partnerships when recommending summer activities. |
| `dashboard-template.html` | Add school dates to timeline visualization, visually distinguished from universal milestones (different color or tag). |
| `.gitignore` | Add `school-profile-*.md` pattern. |

### New Files

| File | Purpose |
|------|---------|
| `references/commands/research-school.md` | Research workflow command — search strategy, fetch logic, extraction, gap assessment, file creation/update |
| `school-profile-[slug].md` | Generated per school — not a template, created by the research process |

---

## 7. How Existing Commands Benefit

| Command | Current Behavior | With School Profile |
|---------|-----------------|-------------------|
| `kickoff` | Asks school name, stores string | Triggers research, weaves findings into course load questions |
| `plan` | Asks "does your school offer AP X?" | Checks profile first, only asks for gaps |
| `activities` | Generic extracurricular advice | References school's actual clubs, sports, competitions |
| `testing` | Generic AP exam guidance | Knows which APs the school offers, can target prep |
| `summer` | Generic summer recommendations | Knows about dual enrollment options, school partnerships |
| `review` | Progress check against milestones | Includes school-specific date awareness |
| `dashboard` | Universal timeline only | School dates on timeline, school context in profile section |
| Session start | Checks timeline engine | Also checks school Key Dates, surfaces 14-day reminders |
