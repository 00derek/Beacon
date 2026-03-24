# School Research Integration — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add automated school research during kickoff that populates a reusable school profile file, enabling school-aware course planning, activity recommendations, and proactive date reminders.

**Architecture:** New `research-school` command (reference module at `references/commands/research-school.md`) triggers during kickoff after school name collection. Research runs as a blocking step, writes results to a separate `school-profile-[slug].md` file, and integrates with the timeline engine via a new `Source` column. Existing commands (`plan`, `activities`, `testing`, `summer`) read the school profile when available.

**Tech Stack:** Markdown skill files, WebSearch/WebFetch tools, HTML dashboard template (vanilla JS)

**Spec:** `docs/superpowers/specs/2026-03-23-school-research-design.md`

---

## File Map

| File | Action | Responsibility |
|------|--------|----------------|
| `references/commands/research-school.md` | Create | Research workflow — search strategy, fetch, extraction, gap assessment, file creation/update |
| `references/commands/kickoff.md` | Modify | Insert research trigger after school name question, weave school data into course load question |
| `SKILL.md` | Modify | Add command to registry, update state schema (School Profile field + Source column), update session start protocol, update gitignore guidance |
| `references/timeline-engine.md` | Modify | Add Source column convention, school-calendar merge guidance, 14-day urgency rule |
| `references/commands/plan.md` | Modify | Check school profile before asking about AP availability |
| `references/commands/activities.md` | Modify | Reference school profile extracurriculars in inventory step |
| `references/commands/testing.md` | Modify | Reference school profile AP offerings for exam strategy |
| `references/commands/summer.md` | Modify | Reference school profile special programs for dual enrollment |
| `references/commands/help.md` | Modify | Add research-school to command guide |
| `dashboard-template.html` | Modify | Add source-based styling for school-calendar timeline items |
| `.gitignore` | Modify | Add `school-profile-*.md` pattern |

---

### Task 1: Add `school-profile-*.md` to `.gitignore`

**Files:**
- Modify: `.gitignore`

- [ ] **Step 1: Append the gitignore pattern**

Append to `.gitignore` (after the existing `dashboard.html` line, before `.DS_Store`). Do NOT replace the file — only insert:

```
# School profile (generated per school)
school-profile-*.md
```

- [ ] **Step 2: Commit**

```bash
git add .gitignore
git commit -m "chore: gitignore generated school profile files"
```

---

### Task 2: Create `references/commands/research-school.md`

This is the core new file — the research workflow command.

**Files:**
- Create: `references/commands/research-school.md`

- [ ] **Step 1: Write the research-school command reference**

```markdown
# research-school — School Research Workflow

Load `references/counselor-styles.md` and apply the student's active style to all communication during research.

---

### Trigger Modes

This command runs in two modes:

1. **Automatic (during kickoff)**: Triggered after the student provides their school name (Step 2, question 2 of kickoff). Runs as a blocking step with a brief progress message before resuming kickoff.
2. **On-demand**: Invoked directly by the student or counselor. Supports natural language variations:
   - "research-school" or "refresh my school info" → full re-research
   - "research-school [URL]" or "here's our course catalog: [URL]" → fetch specific URL and update profile
   - "here's my school's calendar" + URL → targeted update for a specific category

### Pre-Research Check

Before running research:

1. **Generate the school profile filename** using the slug algorithm:
   - Lowercase the school name
   - Strip trailing suffixes in order (longest first): "preparatory school", "high school", "middle school", "academy", "preparatory", "school"
   - Trim whitespace, replace spaces with hyphens
   - Strip non-alphanumeric characters (except hyphens)
   - Append state abbreviation (lowercase) with a hyphen
   - Result: `school-profile-[slug].md` (e.g., "Carlmont High School" in CA → `school-profile-carlmont-ca.md`)

2. **Check if the file already exists:**
   - If it exists and "Last Updated" is within 6 months AND within the current school year (after August 1): read it and skip research. Inform: "I already have [School]'s profile from [date]. Using that."
   - If it exists but is stale (>6 months old OR from a prior school year): ask the student: "I have school data from [date] — want me to refresh it, or use what I have?"
   - If it doesn't exist: proceed with research.

3. **Check tool availability**: If WebSearch or WebFetch tools are not available, skip research. Create a minimal profile with all categories marked "Not found" and inform: "Web search isn't available right now — I'll ask you about your school as we go. If you have a link to your school's website, share it anytime."

### Step 1: Discovery (WebSearch)

Tell the student: "Let me look up [School Name] so I can give you school-specific recommendations. This'll take a moment."

Run up to 5 targeted WebSearch calls (use current calendar year for [year]):

1. `"[School Name] [State] course catalog [year]"`
2. `"[School Name] [State] AP honors classes offered"`
3. `"[School Name] [State] clubs extracurriculars athletics"`
4. `"[School Name] [State] academic calendar [year]"`
5. `"[School Name] [State] staff directory counselors"`

**Search budget**: Maximum 5 WebSearch calls total. If a search returns no useful results, move on.

### Step 2: Deep Fetch (WebFetch)

From search results, identify the school's official website domain. Fetch up to 8 key pages:

- Academics / course catalog page
- Activities / clubs page
- Calendar / important dates page
- Staff directory page
- Graduation requirements page
- Any pathway or program pages found

**Fetch budget**: Maximum 8 WebFetch calls total. If a fetch times out or returns unhelpful content, skip it and mark that category as a gap.

### Step 3: Structured Extraction

Parse fetched content into 8 categories. For each category, extract structured data:

1. **Course catalog** — courses organized by department (Math, Science, English, Social Studies, World Languages, Arts, CTE, Electives). Flag AP and Honors courses specifically.
2. **Course pathways & prerequisites** — prerequisite chains (e.g., "Pre-Calc Honors requires Algebra II/Trig"). Focus on math/science sequences as these are most critical for track planning.
3. **Graduation requirements** — minimum credits by category, required courses.
4. **Staff directory** — counselors (with caseload assignments if found), department heads.
5. **Extracurriculars** — clubs, athletics, arts programs, competitions. Organize by category.
6. **Special programs** — dual enrollment partnerships, CTE pathways, IB program, magnet programs, honors societies.
7. **Key dates** — registration deadlines, course selection windows, AP exam sign-ups, school calendar dates. Include the "Relevant To" field (which grade levels or groups each date applies to).
8. **School profile** — type (public/private/charter), location, enrollment, college-going rate, demographics if available.

### Step 4: Gap Assessment

For each of the 8 categories, assign a status:

- **Found** — data captured, include source URL
- **Partial** — some data found, note what's missing
- **Not found** — no data found

### Step 5: Write School Profile File

Write the structured data to `school-profile-[slug].md` using this format:

```
# School Profile: [Full School Name] ([State])

Last Updated: [today's date]
Sources:
- [URL 1]
- [URL 2]
- [...]

## School Overview
- Type: [Public / Private / Charter]
- Location: [City, State]
- Enrollment: [number if found]
- College-going rate: [percentage if found]

## Course Catalog
### Math
- [course list]
### Science
- [course list]
### English
- [course list]
[...by department as found]

## Course Pathways & Prerequisites
- [prerequisite chain 1]
- [prerequisite chain 2]
[...key chains found]

## Graduation Requirements
- [requirements by category]

## AP & Honors Offerings
- [summary with any restrictions, e.g., "No AP for 9th graders"]
- [total count if determinable]

## Special Programs
- [dual enrollment, CTE, IB, etc.]

## Extracurriculars
### Clubs
- [list]
### Athletics
- [list]
### Arts
- [list]
### Competitions
- [list]

## Staff Directory
### Counselors
- [names and assignments if found]
### Department Heads
- [names if found]

## Key Dates
| Date | Event | Relevant To |
|------|-------|-------------|
[rows — only future dates from current date]

## Research Gaps
- [ ] [Category] — [what's missing, suggestion for student]
[rows for each Not Found or Partial category]
```

### Step 6: Report Summary

After writing the file, report to the student:

- If most categories found: "Found [School]'s course catalog, activities, and calendar. A few things I couldn't find — I'll ask you about those as we go."
- If few categories found: "I found some information about [School] but couldn't locate everything. I'll ask you about the rest as we go. If you have a link to your school's website, that would help."
- If nothing found: "I wasn't able to find information about [School Name] online. No worries — I'll ask you about courses and activities as we go. If you have a link to your school's website, you can share it anytime and I'll pull the details."

### Complete Failure Handling

If all searches return zero useful results:

1. Create a minimal school profile with all 8 categories marked "Not found" in the Research Gaps section
2. Inform the student (see Step 6 failure message)
3. Proceed with kickoff using the conversational approach for all school-related questions
4. The minimal profile serves as a container for student-provided data added later

### On-Demand: URL-Based Update

When the student provides a URL for missing data:

1. Fetch the URL via WebFetch
2. Extract relevant data for the applicable category
3. Update the school profile file — replace the "Not found" entry with extracted data
4. Remove the corresponding item from Research Gaps
5. Confirm: "Updated your school profile with [category] from that link."

### On-Demand: Full Refresh

When invoked with no arguments on an existing profile:

1. Re-run Steps 1-5, overwriting the existing file
2. Preserve any manually-added data (from student-provided URLs) by noting it in the Research Gaps section if it can't be re-verified

### State Update

After creating or updating the school profile:

- Add `School Profile: school-profile-[slug].md` to the Profile section of `counseling_state.md` (if not already present)
- Do NOT duplicate school data into the state file — the state references the school profile file
```

- [ ] **Step 2: Verify the file reads correctly**

Read the file back to confirm formatting is correct and no markdown nesting issues.

- [ ] **Step 3: Commit**

```bash
git add references/commands/research-school.md
git commit -m "feat: add research-school command reference"
```

---

### Task 3: Modify `references/commands/kickoff.md` — Insert Research Trigger

**Files:**
- Modify: `references/commands/kickoff.md:19-49` (Step 2: Profile Collection)

- [ ] **Step 1: Add research trigger after school name question**

After question 2 ("What school do you go to?") in Step 2, insert the research trigger. The new Step 2 should read:

Replace the current question list (lines 21-48) — insert after question 2 and before question 3:

After line 24 (`2. "What school do you go to?"`), add:

```markdown
   **After the student answers question 2**: Run the `research-school` workflow (see `references/commands/research-school.md`). This is a blocking step — tell the student "Let me look up [school] so I can give you school-specific recommendations" and run the research before continuing to question 3. Once research completes (or if skipped due to tool unavailability), resume with question 3.
```

**Note**: Steps 2 and 3 reference line numbers from the original file. After Step 1's insertion, line numbers will have shifted. Use text matching (search for the exact string) rather than line numbers.

- [ ] **Step 2: Update course load question to reference school profile**

Modify question 4 (the current course load question, originally line 26) to reference school data:

Replace:
```markdown
4. "What classes are you taking right now?" (Capture the full course load — this reveals current rigor.)
```

With:
```markdown
4. "What classes are you taking right now?" (Capture the full course load — this reveals current rigor. If the school profile exists and has a course catalog, cross-reference the student's courses against known offerings. If a student names a course not in the catalog, ask about it — the catalog may be incomplete. If the student seems unsure about available courses, reference the school profile: "I see [School] offers [courses] — are you taking any of those?")
```

- [ ] **Step 3: Update Step 5 (Initialize counseling_state.md) to include School Profile reference**

At line 95, the Profile population list includes "name, grade, school, GPA, counselor style...". Add School Profile to this:

Replace:
```markdown
- **Profile**: All data collected in Steps 2-4 (name, grade, school, GPA, counselor style, directness, parent involvement, concerns, current semester based on date).
```

With:
```markdown
- **Profile**: All data collected in Steps 2-4 (name, grade, school, GPA, counselor style, directness, parent involvement, concerns, current semester based on date). Include `School Profile: school-profile-[slug].md` if school research was run.
```

- [ ] **Step 4: Commit**

```bash
git add references/commands/kickoff.md
git commit -m "feat: trigger school research during kickoff after school name"
```

---

### Task 4: Modify `SKILL.md` — State Schema, Session Protocol, Command Registry, Routing, and Mode Detection

**Files:**
- Modify: `SKILL.md:89-100` (Profile schema — add School Profile field)
- Modify: `SKILL.md:194-199` (Timeline Status schema — add Source column)
- Modify: `SKILL.md:29-33` (Session Start Protocol — read school profile, staleness check, Key Dates surfacing)
- Modify: `SKILL.md:41-43` (Mid-Session Save Protocol — school profile URL updates)
- Modify: `SKILL.md:236-253` (State Update Triggers — add research-school)
- Modify: `SKILL.md:387-401` (Command Registry — add research-school row)
- Modify: `SKILL.md:403-419` (File Routing — add research-school entry)
- Modify: `SKILL.md:421-437` (Mode Detection Priority — add school research intent)
- Modify: `SKILL.md:68-75` (Grade-Aware Behavior — add research-school to Freshman)

- [ ] **Step 1: Add School Profile field to Profile schema**

In the `counseling_state.md` Format section (around line 92, after `- School:`), add:

```markdown
- School Profile: [school-profile-[slug].md or "not yet researched"]
```

So the Profile section becomes:
```markdown
## Profile
- Name:
- Grade: [9/10/11/12]
- School:
- School Profile: [school-profile-[slug].md or "not yet researched"]
- Target graduation year:
```

- [ ] **Step 2: Add Source column to Timeline Status schema**

In the Timeline Status section (around line 197), update the table format:

Replace:
```markdown
  | Milestone | Expected | Status | Notes |
  |-----------|----------|--------|-------|
  [rows — Status: ahead / on-track / coming-up / behind]
```

With:
```markdown
  | Milestone | Expected | Status | Source | Notes |
  |-----------|----------|--------|--------|-------|
  [rows — Status: ahead / on-track / coming-up / behind. Source: timeline-engine / school-calendar]
```

- [ ] **Step 3: Update Session Start Protocol to read school profile and check staleness**

In the Session Start Protocol (around line 31), update step 2 to include school profile reading. The current step 2 says "If it exists: Check the student's grade level and current date against the milestone timeline...".

Insert a new sub-step after reading state and before the timeline check:

After "Read `counseling_state.md` if it exists." and before the timeline check, add:

```markdown
   If the Profile section contains a `School Profile` reference, read that file.
   - **Staleness check**: If the school profile's "Last Updated" date is >6 months old, OR the profile was last updated before August 1 of the current school year, suggest: "Your school data is from [date]. Want me to refresh it? (`research-school`)" Continue with the existing data regardless of whether the student agrees — the refresh can happen after the greeting.
   - **Key Dates surfacing**: Use the school profile's Key Dates section during the milestone check — any school date within 14 days gets `!!` (urgent) status and is surfaced in the greeting alongside timeline milestones. Tag these with `school-calendar` in the Source column.
```

- [ ] **Step 4: Add research-school to State Update Triggers**

After the `summer` trigger entry (around line 246), add:

```markdown
- `research-school` creates or updates the school profile file (`school-profile-[slug].md`). Adds `School Profile:` reference to the Profile section of `counseling_state.md`. Copies grade-relevant Key Dates from the school profile into Timeline Status with `Source: school-calendar`. Does not modify other state sections.
```

- [ ] **Step 5: Add research-school to the Command Registry table**

In the Command Registry table (SKILL.md line 401, after the `dashboard` row), add:

```markdown
| `research-school` | Look up school course catalog, activities, calendar, and staff. Auto-triggers during kickoff; can be run on-demand to refresh or update with a URL. |
```

- [ ] **Step 6: Add research-school to File Routing**

In the File Routing section (SKILL.md line 419, after the `dashboard` entry), add:

```markdown
- **`research-school`**: Read `references/commands/research-school.md` for the research workflow. Also read `references/timeline-engine.md` (for Key Dates integration with timeline milestones).
```

- [ ] **Step 7: Add research-school to Mode Detection Priority**

In the Mode Detection Priority section (SKILL.md around line 436, before the "Otherwise" fallback at line 437), add a new entry:

```markdown
12. School research / "look up my school" / "refresh school info" / "here's my school's website" / student provides a URL for school data → `research-school`
```

Renumber the existing entry 12 (`review`) to 13, and "Otherwise" to 14.

- [ ] **Step 8: Add research-school to the Grade-Aware Behavior Adaptation**

In the Grade-Aware Behavior Adaptation section (around line 72), update the Freshman commands list. After `summer`, add `research-school`:

```markdown
- **Freshman (Grade 9)**: ... Commands most relevant: `kickoff`, `discover`, `plan`, `activities`, `summer`, `research-school` (auto-triggered during kickoff).
```

Don't add to every grade — it auto-triggers during kickoff and is on-demand after that.

- [ ] **Step 9: Add school profile to managed files and mid-session save**

In the Mid-Session Save Protocol (SKILL.md around line 43), after "Write to `counseling_state.md` after any major workflow completes", add:

```markdown
Also write to the school profile file (`school-profile-[slug].md`) when it is updated mid-session — e.g., when a student provides a URL for missing school data. Update `counseling_state.md` Timeline Status with any new Key Dates from the updated profile.
```

- [ ] **Step 10: Commit**

```bash
git add SKILL.md
git commit -m "feat: add research-school to command registry, routing, mode detection, and session protocol"
```

---

### Task 5: Modify `references/timeline-engine.md` — Source Column and School Calendar Integration

**Files:**
- Modify: `references/timeline-engine.md:306-319` (Integration Points)
- Modify: `references/timeline-engine.md:34-42` (Status icons)
- Modify: `references/timeline-engine.md:251-283` (Timeline Check Output Format)

- [ ] **Step 1: Add Source column convention to the status icons section**

After the status icons table (line 42), add:

```markdown

**Source tagging:**

| Source | Meaning |
|--------|---------|
| `timeline-engine` | Universal milestone from the tables below |
| `school-calendar` | School-specific date from the school profile |

When displaying timeline items, include the source to help the student understand where dates come from. School-calendar items within 14 days of the current date automatically get `!!` (urgent) status regardless of their original categorization.
```

- [ ] **Step 2: Update Timeline Check Output Format to include school dates**

In the Timeline Check Output Format section (around line 258), after the "Coming Up This Semester" section, add:

```markdown

### School Dates (Next 30 Days)
!! [School Date] — [event description] [school-calendar]
.. [School Date] — [event description] [school-calendar]
```

- [ ] **Step 3: Update Integration Points to include school profile**

In the Integration Points section (around line 315), add to "Interacts with:":

```markdown
- `school-profile-[slug].md` — reads Key Dates section during session greeting; school dates within 14 days are surfaced as `!!` items alongside timeline milestones
```

Also add to "Loaded by:" (line 308):

```markdown
**Loaded by:** `kickoff`, `plan`, `testing`, `apply`, `summer`, `review`, `research-school`
```

- [ ] **Step 4: Commit**

```bash
git add references/timeline-engine.md
git commit -m "feat: add Source column and school-calendar integration to timeline engine"
```

---

### Task 6: Modify `references/commands/plan.md` — Check School Profile Before Asking

**Files:**
- Modify: `references/commands/plan.md:30-33` (Step 2: Map Track to Courses)

- [ ] **Step 1: Update the school offerings check**

Replace line 33:

```markdown
2. **Their school's offerings:** Ask: "Does your school offer AP [key course for this track]?" Adapt recommendations based on what is actually available. If the school lacks a critical course, recommend dual enrollment at a local community college.
```

With:

```markdown
2. **Their school's offerings:** Check the school profile file (referenced in `counseling_state.md` Profile section) for course catalog and AP/Honors offerings. If the school profile exists and has course data, use it directly — don't ask the student to confirm what's already known. Reference it naturally: "I see [School] offers AP Physics C, so we can slot that into junior year." If the school profile is missing, has gaps in the course catalog, or doesn't exist, fall back to asking: "Does your school offer AP [key course for this track]?" If the school lacks a critical course (per profile or student confirmation), recommend dual enrollment at a local community college. If the school profile lists dual enrollment partnerships under Special Programs, reference those specifically.
```

- [ ] **Step 2: Commit**

```bash
git add references/commands/plan.md
git commit -m "feat: plan command checks school profile before asking about courses"
```

---

### Task 7: Modify `references/commands/activities.md` — Reference School Extracurriculars

**Files:**
- Modify: `references/commands/activities.md:7-9` (Step 1: Current Activity Inventory)

- [ ] **Step 1: Add school profile reference to activity inventory**

After line 9 ("For each activity, ask:"), insert before the numbered questions:

```markdown

**If a school profile exists** (check `counseling_state.md` Profile section for School Profile reference): Read the Extracurriculars section. Use it to prompt the student about available opportunities they may not have mentioned: "I see [School] has a Robotics club and Science Olympiad — are you involved in either of those?" This helps surface activities the student might have forgotten or not considered. Don't list every club — focus on those aligned with the student's track and spike.

```

- [ ] **Step 2: Update gap analysis to reference school offerings**

In Step 3: Gap Analysis (around line 68), after the gap categories, add:

```markdown

**If a school profile exists with extracurriculars data**: Cross-reference gap recommendations against what the school actually offers. Don't suggest "join a robotics club" if the school doesn't have one. Instead, suggest starting one or finding an external alternative. Reference the school profile's Clubs, Athletics, Arts, and Competitions sections when making specific activity recommendations.
```

- [ ] **Step 3: Commit**

```bash
git add references/commands/activities.md
git commit -m "feat: activities command references school profile for extracurriculars"
```

---

### Task 8: Modify `references/commands/testing.md` — Reference School AP Offerings

**Files:**
- Modify: `references/commands/testing.md:103-122` (Step 5: AP Exam Strategy)

- [ ] **Step 1: Add school profile reference to AP strategy**

After line 105 ("Connect AP strategy to the student's academic track and spike:"), insert:

```markdown

**If a school profile exists**: Read the AP & Honors Offerings section to know exactly which AP courses the school offers and any restrictions (e.g., "no AP for 9th graders", "maximum 4 APs per year"). Use this to give precise recommendations: "Your school offers 12 AP courses — here are the ones that align with your track." If the school offers limited APs, adjust the strategy: fewer APs available means each one taken carries more weight with admissions. Reference any dual enrollment options from the school profile's Special Programs section as alternatives for subjects not offered as AP.

```

- [ ] **Step 2: Commit**

```bash
git add references/commands/testing.md
git commit -m "feat: testing command references school profile for AP offerings"
```

---

### Task 9: Modify `references/commands/summer.md` — Reference School Special Programs

**Files:**
- Modify: `references/commands/summer.md:5-13` (Step 1: Context)

- [ ] **Step 1: Add school profile reference to context gathering**

After line 10 ("Pull from state: spike area(s), interest discovery results, academic track"), insert:

```markdown
   Also pull from the school profile (if it exists): check the Special Programs section for dual enrollment partnerships, CTE pathways, and any school-affiliated summer programs. These are often overlooked but provide accessible, low-cost options. Reference them when making recommendations: "I see [School] has a dual enrollment partnership with [College] — that could be a great summer option for getting ahead on credits."
```

- [ ] **Step 2: Commit**

```bash
git add references/commands/summer.md
git commit -m "feat: summer command references school profile for special programs"
```

---

### Task 10: Modify `references/commands/help.md` — Add research-school to Command Guide

**Files:**
- Modify: `references/commands/help.md:38-50` (Exploration & Planning table)

- [ ] **Step 1: Add research-school to the command guide**

In the "Exploration & Planning" section, after the `activities` row (around line 43), add a new row:

```markdown
| `research-school` | Look up your school's course catalog, activities, staff, calendar, and more. Runs automatically during kickoff — use this to refresh data or add a URL your school's website. |
```

- [ ] **Step 2: Add to the diagnostic router**

In the "Not Sure What You Need?" table (around line 86), add:

```markdown
| "What courses does my school offer?" | `research-school` — we'll look up your school's catalog and offerings |
```

- [ ] **Step 3: Commit**

```bash
git add references/commands/help.md
git commit -m "feat: add research-school to help command guide"
```

---

### Task 11: Modify `dashboard-template.html` — School Calendar Styling

**Files:**
- Modify: `dashboard-template.html` (CSS and JS sections)

- [ ] **Step 1: Add school-calendar CSS class**

In the CSS section, after the existing `.timeline-status.behind` rule (around line 373), add:

```css
.timeline-source {
  font-size: 0.7rem;
  padding: 2px 6px;
  border-radius: 4px;
  margin-left: 6px;
}
.timeline-source.school-calendar {
  background: rgba(167,139,250,0.15);
  color: #a78bfa;
}
@media (prefers-color-scheme: dark) {
  .timeline-source.school-calendar { color: #c4b5fd; }
}
```

- [ ] **Step 2: Update timeline data structure in the example**

In the example data (around line 501), update the timeline array to show the source field:

```javascript
timeline: [
  { milestone: "Interest exploration complete", status: "ahead", source: "timeline-engine", notes: "Finished early" },
  { milestone: "Academic track mapped", status: "on-track", source: "timeline-engine", notes: "" },
  { milestone: "Course selection opens", status: "coming-up", source: "school-calendar", notes: "Register via student portal" },
  { milestone: "PSAT registration", status: "coming-up", source: "timeline-engine", notes: "October deadline" },
  { milestone: "Start test prep", status: "behind", source: "timeline-engine", notes: "Should have started last semester" }
],
```

- [ ] **Step 3: Update timeline rendering to show source badge**

In the timeline rendering section (around line 739-746), update the `forEach` to include the source badge. **Note**: The actual file has 8 spaces of indentation — match the existing indentation when editing. Use text search to find the exact line.

After the existing line:
```javascript
        html += '<span class="timeline-status ' + esc(item.status) + '">' + esc(item.status.replace("-", " ")) + '</span>';
```

Add immediately after it:
```javascript
        if (item.source === "school-calendar") html += '<span class="timeline-source school-calendar">school</span>';
```

- [ ] **Step 4: Commit**

```bash
git add dashboard-template.html
git commit -m "feat: add school-calendar source badge to dashboard timeline"
```

---

### Task 12: Integration Verification

**Files:** All modified files

- [ ] **Step 1: Verify cross-references are consistent**

Read each modified file and verify:
- `research-school.md` slug algorithm matches examples in spec
- `kickoff.md` references `references/commands/research-school.md` correctly
- `SKILL.md` Profile schema includes `School Profile:` field
- `SKILL.md` Timeline Status schema has 5 columns (Milestone, Expected, Status, Source, Notes)
- `SKILL.md` Session Start Protocol mentions reading school profile
- `timeline-engine.md` mentions `school-calendar` source tag
- `plan.md` references school profile for course data
- `activities.md` references school profile for extracurriculars
- `testing.md` references school profile for AP offerings
- `summer.md` references school profile for special programs
- `help.md` lists research-school command
- `dashboard-template.html` handles `source` field in timeline items
- `.gitignore` includes `school-profile-*.md`

- [ ] **Step 2: Verify no broken references**

Search for any references to the old `[[wiki-link]]` syntax or `school-profile-carlmont-high.md` (old slug format) — should be zero matches.

- [ ] **Step 3: Open dashboard-template.html in browser**

Open the file to verify the school-calendar badge renders correctly with the example data.

- [ ] **Step 4: Final commit if any fixes needed**

```bash
git add -A
git commit -m "fix: address integration review findings"
```

Only run this if fixes were needed. If everything is clean, skip.
