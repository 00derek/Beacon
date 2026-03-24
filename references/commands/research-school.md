# research-school — School Research Workflow

Load `references/counselor-styles.md` and apply the student's active style to all communication during this workflow.

---

### Trigger Modes

**Automatic** — triggered during kickoff when a school name and state are captured. Run silently in the background without interrupting the kickoff conversation. Notify the student only when research completes or fails.

**On-demand** — triggered explicitly by the student (three modes):
1. No args / "Research my school" / "Update my school profile" → full re-research
2. With URL / "Here's our course catalog: [URL]" → fetch URL and update profile
3. With category / "research-school calendar" / "Can you look up my school's calendar?" → re-research just that specific category

---

### Pre-Research Check

Before running any searches, complete these checks in order:

**1. Slug algorithm** — Generate the school profile filename:

1. Lowercase the school name
2. Strip trailing suffixes in order (longest first): "preparatory school", "high school", "middle school", "academy", "preparatory", "school"
3. Trim whitespace, replace spaces with hyphens
4. Strip non-alphanumeric characters (except hyphens)
5. Append state abbreviation (lowercase) with a hyphen

Filename format: `school-profile-[slug]-[state].md`

Examples:
- "Carlmont High School" (CA) → `school-profile-carlmont-ca.md`
- "Thomas Jefferson Academy" (VA) → `school-profile-thomas-jefferson-va.md`
- "Phillips Exeter Preparatory School" (NH) → `school-profile-phillips-exeter-nh.md`

**2. File existence check** — Look for an existing `school-profile-[slug]-[state].md` file.

- If the file exists, proceed to the staleness check.
- If no file exists, proceed directly to Step 1: Discovery.

**3. Staleness check** — If the file exists, check its `Last Updated` date.

The profile is stale if either condition is true:
- The last updated date is more than 6 months ago
- The last updated date falls before August 1 of the current school year

If stale, ask the student: "I have school data from [date] — want me to refresh it, or use what I have?" If the student says yes, proceed to Step 1: Discovery. If the student says use existing, read the file and skip research. Continue with existing data regardless — the refresh can happen after the greeting.
If current, skip research. Report: "I already have a current profile for [School Name] on file." Use the existing file for all school-specific recommendations.

**4. Tool availability check** — Confirm WebSearch and WebFetch are available.

If either tool is unavailable, skip all research steps. Create a minimal profile file with all categories marked "Not found — research tools unavailable at session time." Notify the student: "Web search isn't available right now — I'll ask you about your school as we go. If you have a link to your school's website, share it anytime."

---

### Step 1: Discovery (WebSearch)

Run up to 5 targeted searches. Replace `[School Name]`, `[State]`, and `[year]` with the current school year (e.g., 2025-2026).

Search queries (run all 5 unless earlier searches yield comprehensive results):

1. `[School Name] [State] course catalog [year]`
2. `[School Name] [State] AP honors classes offered`
3. `[School Name] [State] clubs extracurriculars athletics`
4. `[School Name] [State] academic calendar [year]`
5. `[School Name] [State] staff directory counselors`

Collect all URLs returned. Prioritize the school's official domain over third-party sources. Note which categories each URL is likely to cover before fetching.

---

### Step 2: Deep Fetch (WebFetch)

Fetch the most promising URLs from Step 1. Budget: **maximum 8 WebFetch calls**.

Prioritize pages in this order:
1. Official course catalog or course offerings page
2. Counseling or guidance staff directory
3. Clubs, activities, and athletics pages
4. Academic calendar or key dates page
5. School overview or "About" page (for enrollment, accreditation, mission)
6. Any remaining pages with high information density for uncovered categories

If a page is very long, extract only the relevant section rather than the full body.

---

### Step 3: Structured Extraction

From all fetched content, extract data for these 8 categories. Mark each as **Found**, **Partial**, or **Not found**.

1. **Course catalog** — Full list of courses offered, organized by department
2. **Course pathways & prerequisites** — Sequences (e.g., Algebra I → Geometry → Algebra II) and stated prerequisites
3. **Graduation requirements** — Credits required by subject area, state or district minimums
4. **Staff directory** — Counselors (names, grade assignments, contact), department heads
5. **Extracurriculars** — Clubs, athletics teams, arts programs, competitions
6. **Special programs** — IB program, dual enrollment partnerships, magnet programs, honors societies, STEM/arts pathways
7. **Key dates** — Registration deadlines, testing windows, grading periods, school holidays, graduation
8. **School profile** — Enrollment size, public/private, grade span, accreditation, demographics (if available)

---

### Step 4: Gap Assessment

For each of the 8 categories, record:
- **Found** — sufficient data captured for counseling use
- **Partial** — some data captured but incomplete
- **Not found** — no usable data retrieved

Count totals. Determine overall research quality:
- 6-8 Found → Strong profile
- 3-5 Found → Partial profile — note gaps prominently
- 0-2 Found → Minimal profile — flag for manual follow-up

---

### Step 5: Write School Profile File

Write the school profile to `school-profile-[slug]-[state].md`. Use this exact template:

```markdown
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

---

## Course Catalog

### [Department Name]
- [Course Name] — [level: standard / honors / AP / IB / dual enrollment] — [prerequisites if known]
- ...

[Repeat for each department. "Not found" if course catalog was unavailable.]

---

## Course Pathways & Prerequisites

[Describe key sequences — especially math, science, and foreign language. Include explicit prerequisites if stated. "Not found" if unavailable.]

---

## Graduation Requirements

| Subject Area | Credits Required |
|---|---|
| English | [N] |
| Math | [N] |
| Science | [N] |
| Social Studies | [N] |
| [others] | [N] |
| Total | [N] |

["Not found" if unavailable — replace table with that note.]

---

## AP & Honors Offerings

[List all AP and honors courses by subject. Note if IB is offered. "Not found" if unavailable.]

---

## Special Programs

[Dual enrollment partnerships, magnet programs, IB pathway, honors societies, STEM/arts academies, etc. "Not found" if unavailable.]

---

## Extracurriculars

### Clubs
[List known clubs. "Not found" if unavailable.]

### Athletics
[List varsity and JV teams. "Not found" if unavailable.]

### Arts
[Performing arts, visual arts, music ensembles. "Not found" if unavailable.]

### Competitions & Academic Teams
[Math team, Science Olympiad, debate, robotics, etc. "Not found" if unavailable.]

---

## Staff Directory

### Counselors
| Name | Grade Assignment | Contact |
|---|---|---|
| [Name] | [Grades] | [email or "Not found"] |

["Not found" if no counselor directory was available.]

### Department Heads
| Department | Name |
|---|---|
| [Department] | [Name or "Not found"] |

["Not found" if unavailable.]

---

## Key Dates

| Date | Event | Relevant To |
|---|---|---|
| [date] | [event] | [e.g., all students / seniors / course registration] |

["Not found" if academic calendar was unavailable.]

---

## Research Gaps

- [ ] Course catalog: [Found / Partial / Not found]
- [ ] Course pathways & prerequisites: [Found / Partial / Not found]
- [ ] Graduation requirements: [Found / Partial / Not found]
- [ ] Staff directory: [Found / Partial / Not found]
- [ ] Extracurriculars: [Found / Partial / Not found]
- [ ] Special programs: [Found / Partial / Not found]
- [ ] Key dates: [Found / Partial / Not found]
- [ ] School profile: [Found / Partial / Not found]
```

---

### Step 6: Report Summary

After writing the profile file, report to the student based on overall research quality:

**Strong profile (6-8 Found):**
> "I researched [School Name] and built a school profile. I found course offerings, extracurriculars, key staff, and important dates. I'll use this to give you more specific advice about course selection and activity planning."

**Partial profile (3-5 Found):**
> "I researched [School Name] and captured what I could. I have good information on [found categories] but couldn't find [missing categories]. I'll work with what's here and flag when I'm missing something relevant to your plan."

**Minimal profile (0-2 Found):**
> "I had trouble finding detailed information about [School Name] online. I've created a profile with what little I could find. If you can share your school's course catalog or website, I can fill in the gaps. For now, I'll ask you directly when I need school-specific details."

---

### Complete Failure Handling

If all 5 searches return no usable results and all fetches fail or return irrelevant content:

1. Create a minimal profile file with all 8 categories marked "Not found."
2. Set Research Quality to "Minimal."
3. Do not retry in the same session.
4. Report to the student:
   > "I wasn't able to find detailed information about [School Name] online. This sometimes happens with smaller or newer schools. I've noted the gap — you can help me fill it in by sharing your course catalog, or I'll ask you directly when your school's offerings are relevant."
5. Continue with the current workflow using manual questions in place of profile data.

---

### On-Demand: URL-Based Update

When the student provides a specific URL (e.g., "Here's my school's course catalog: [URL]"):

1. WebFetch the provided URL immediately.
2. Extract all 8 categories from that page.
3. Merge the new data into the existing school profile file, updating any "Not found" or "Partial" categories.
4. Update the `Last Updated` date.
5. Update the Research Gaps checklist.
6. Report: "Thanks — I've updated your school profile with the information from that page. I now have [updated categories]."

---

### On-Demand: Category-Targeted Re-Research

When the student requests research on a specific category (e.g., "research-school calendar" / "can you look up my school's calendar?"):

1. Identify the requested category (one of the 8 defined categories: course catalog, course pathways & prerequisites, graduation requirements, staff directory, extracurriculars, special programs, key dates, school profile).
2. Run a targeted search for that category only — use the relevant search query from Step 1: Discovery for that category.
3. Fetch the most relevant result(s) for the category (budget: maximum 3 WebFetch calls).
4. Extract data for that category only.
5. Update only that section of the existing school profile file.
6. Update the `Last Updated` date and Sources list.
7. Update the Research Gaps checklist for that category.
8. Report: "I've updated the [category name] section of your school profile."

---

### On-Demand: Full Refresh

When the student requests a full re-research (e.g., "Re-research my school" / "Update my school profile"):

1. Ignore the staleness check — proceed to Step 1 regardless of file age.
2. Run the full workflow (Steps 1-6).
3. Overwrite the existing profile file.
4. Report the new research quality and what changed.

---

### State Update

After the profile file is written, add a reference to it in the **Profile** section of `counseling_state.md`:

```
School Profile: school-profile-[slug]-[state].md
```

Also log the research run in the **Session Log**:
```
[date]: research-school — [Research Quality] profile created for [School Name]. Gaps: [list any Not found categories, or "none"].
```
