# School Research Integration — Design Spec

**Date**: 2026-03-23
**Status**: Draft
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

1. Triggers automatically during kickoff (background, non-blocking)
2. Runs targeted web searches and page fetches to gather school data
3. Saves structured findings to a **separate school profile file** (reusable across students)
4. Integrates key dates with the existing timeline engine
5. Can be invoked on-demand for refreshes, URL-based updates, or school changes

## Design Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| When to research | During kickoff, in background | Data ready before course load questions without blocking conversation |
| Where to store | Separate file (`school-profile-[slug].md`) | Keeps state lean; reusable if multiple students share a school |
| What to research | All 8 categories (see below) | Comprehensive school context enables multiple commands |
| Date integration | Source in school file, relevant dates copied to student timeline | Single source of truth + grade-filtered surfacing |
| Gap handling | Flag gaps, fall back to conversational, accept student URLs | Graceful degradation with path to fill gaps |

---

## 1. Research Trigger & Flow

### During Kickoff

After the student provides their school name (kickoff step 2):

1. Beacon acknowledges the school and **continues the kickoff conversation** (GPA, courses, etc.) without pausing
2. In parallel, Beacon kicks off the school research workflow
3. When research completes, Beacon weaves findings into subsequent questions naturally — e.g., when asking about current course load (step 5): "I see Carlmont offers AP Physics C — are you taking that?"
4. If kickoff finishes before research completes, Beacon notes school info will be ready for the next command

### On-Demand Usage

The `research-school` command can be invoked directly:

- **No arguments**: Full re-research (useful for new school year)
- **With URL**: Fetch a specific URL and update the profile (e.g., "here's our course catalog page")
- **With category**: Re-research a specific category (e.g., `research-school calendar`)

Use cases:
- Student transfers to a new school
- New school year with updated catalog/calendar
- Student provides a URL for data Beacon couldn't find
- Refreshing stale data (>6 months old)

---

## 2. Research Strategy & Data Collection

### Phase 1 — Discovery (WebSearch)

Run 4-5 targeted searches in parallel:

1. `"[School Name] [State] course catalog [current year]"`
2. `"[School Name] [State] AP honors classes offered"`
3. `"[School Name] [State] clubs extracurriculars athletics"`
4. `"[School Name] [State] academic calendar [current year]"`
5. `"[School Name] [State] staff directory counselors"`

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

---

## 3. School Profile File Format

### Naming Convention

`school-profile-[school-name-slug].md`

- Example: `school-profile-carlmont-high.md`
- Stored in project root alongside `counseling_state.md`
- Reusable — multiple students at the same school reference the same file

### State File Reference

Add to the Profile section of `counseling_state.md`:

```
School Profile: [[school-profile-carlmont-high.md]]
```

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
| 2026-03-15 | Course selection opens | Sophomores, Juniors |
| 2026-04-01 | Course selection deadline | All students |
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
2. **Session start**: Beacon's existing session-start protocol already checks the timeline engine. It now also reads the school profile's Key Dates section.
3. **Grade-filtered surfacing**: Only dates relevant to the student's grade and track get copied into the student's Timeline Status in `counseling_state.md`, tagged with `[school-calendar]`:

```
| Milestone | Expected | Status | Notes |
|-----------|----------|--------|-------|
| PSAT Registration | 2026-09-15 | upcoming | [timeline-engine] |
| Course Selection Opens | 2026-03-15 | ⚠️ 5 days | [school-calendar] |
| AP Exam Sign-up Deadline | 2026-03-20 | ⚠️ 2 days | [school-calendar] |
```

4. **Proactive reminders**: When a school date is within 14 days, Beacon surfaces it at session start alongside regular milestones — e.g., "Heads up — Carlmont's course selection window opens in 5 days. Want to finalize your course plan today?"

5. **Dashboard**: School dates appear on the dashboard timeline alongside universal milestones, visually distinguished (different color or tag)

6. **Staleness**: Dates are school-year specific. At the start of a new school year (or on refresh), Beacon prompts to re-run research for updated calendar.

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
| `references/commands/kickoff.md` | After step 2 (school name), trigger background school research. Add logic to weave school data into subsequent questions, especially step 5 (current course load). |
| `SKILL.md` | Add school profile to managed files list. Add `research-school` to command list. Update session-start protocol to check school profile Key Dates. Add mid-session save for school profile updates from student-provided URLs. |
| `references/timeline-engine.md` | Add guidance for merging school-specific dates into student timeline. Define 14-day proactive reminder window. Add `[school-calendar]` tagging convention. |
| `references/commands/plan.md` | Check school profile for course availability before asking student. Only ask conversationally if data is missing or marked as a gap. |
| `references/commands/activities.md` | Reference school profile's extracurriculars section when advising on activities strategy. |
| `dashboard-template.html` | Add school dates to timeline visualization, visually distinguished from universal milestones. |

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
