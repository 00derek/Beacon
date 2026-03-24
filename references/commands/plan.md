# plan — Academic Track Mapping Workflow

Load `references/academic-tracks.md` for track definitions, course sequences, math/science guidance, dual enrollment recommendations, and the spike model. Load `references/timeline-engine.md` for academic milestone tracking. Load `references/cross-cutting.md` for spike development, narrative threading, equity check, and MI voice. Load `references/counselor-styles.md` and apply the student's active style to all question delivery, ordering, and phrasing.

---

### Step 1: Confirm Interests from State

Read the Interest Discovery section from `counseling_state.md`. Check for flow activities, strengths, values, and any Holland Code estimate.

**If Interest Discovery data exists:**

- Reflect back what you know: "Based on our earlier exploration, your strongest interests seem to be in [areas], your strengths are [strengths], and you care about [values]. Does that still feel right, or has anything shifted?"
- Propose a primary track: "That pattern maps well to the [Track Name] track. What do you think?"
- If the student disagrees or has shifted, update the Interest Discovery section and adjust the track accordingly.

**If Interest Discovery is empty or sparse:**

- Ask 2-3 quick interest questions (borrow from Phase 1-2 of `references/elicitation-frameworks.md`) to establish a baseline.
- Alternatively, suggest running `discover` first for a stronger foundation: "I can build a course plan now with what we have, but your plan will be stronger if we map your interests first. Want to run through a quick interest exploration, or work with what we've got?"
- Never refuse to run `plan` without discover data — proceed with what is available.

**If the student is undecided:**

- Use the "Undecided" approach: build a broad, rigorous course plan that keeps multiple tracks open. Emphasize courses that transfer across tracks (AP English, AP Statistics, Pre-Calculus/Calculus).
- "When you're not sure yet, the strategy is to keep doors open. A strong foundation in math, science, and writing lets you pivot into almost any track later."

### Step 2: Map Track to Courses

Load the track-specific course recommendations from `references/academic-tracks.md`. Compare the recommended courses against:

1. **The student's current course load:** What are they taking now? Where do they stand in the math and science sequences?
2. **Their school's offerings:** Check the school profile file (referenced in `counseling_state.md` Profile section) for course catalog and AP/Honors offerings. If the school profile exists and has course data, use it directly — don't ask the student to confirm what's already known. Reference it naturally: "I see [School] offers AP Physics C, so we can slot that into junior year." If the school profile is missing, has gaps in the course catalog, or doesn't exist, fall back to asking: "Does your school offer AP [key course for this track]?" If the school lacks a critical course (per profile or student confirmation), recommend dual enrollment at a local community college. If the school profile lists dual enrollment partnerships under Special Programs, reference those specifically.
3. **Their current GPA and comfort level:** A student with a 2.8 GPA should not be pushed into 4 APs. Match rigor to the student's capacity while nudging toward appropriate challenge.

**Apply the course rigor principle from academic-tracks.md:** Admissions officers evaluate whether you challenged yourself relative to what was available. A student taking every AP their school offers is viewed favorably regardless of whether that number is 3 or 15.

**Math sequence check:** The math sequence is the most common source of track misalignment. Verify the student is on the right trajectory for their track using the math sequence table in academic-tracks.md. If they are behind, identify acceleration options (summer coursework, doubling up, dual enrollment).

### Step 3: Build Course Sequence

Generate a year-by-year course recommendation for all remaining years from the student's current grade through 12th. For each year, distinguish:

- **Must-have courses**: Core requirements for the track that should not be skipped
- **Valuable additions**: Courses that strengthen the application and deepen knowledge
- **Elective recommendations**: Courses that explore secondary interests or round out the profile

**Sequencing rules:**

- Respect prerequisites (no AP Calculus BC without AB or equivalent, no AP Physics C without calculus)
- Front-load rigor in junior year (most important year for admissions)
- Balance challenge with well-being — 4 APs is aggressive; 5+ requires strong study habits and time management
- Include at least one elective per year that the student genuinely wants to take, not just what "looks good"
- If dual enrollment is recommended, specify which courses and when

**For the Trades/CTE track:** Map CTE career clusters, industry certifications, and apprenticeship pathways with equal detail and enthusiasm. Dual enrollment at trade schools or career centers is the analog to AP coursework.

### Step 4: Assess Course Rigor

Compare the planned course sequence against what target schools typically expect for the student's intended track. This assessment requires connecting to the student's aspirations (if known) or to general competitive expectations for the track.

**Assessment categories:**

- **Strong match**: The course sequence meets or exceeds what competitive programs expect. The student is well-positioned.
- **Solid foundation with room to strengthen**: The sequence covers the essentials but could benefit from one or two additional challenges (e.g., adding an AP in the track's core area, or accelerating the math sequence).
- **Needs adjustment**: Significant gaps between the planned rigor and what target programs expect. Name the gaps specifically and suggest concrete changes.

**If the student has a college list already** (from the `schools` command), pull specific expectations from those schools. If no list exists yet, use the track-level guidance from academic-tracks.md for general competitive benchmarks.

**Be honest but MI-framed.** At directness level 5: "Your schedule is below what MIT's engineering program expects — here's what to change." At directness level 2: "You're building a solid foundation. The opportunity to make it even stronger is adding AP Physics C, which is what engineering programs look for."

### Step 5: Identify Supporting Actions

Beyond coursework, identify the key supporting actions that complement the academic plan:

- **Testing timeline**: When should the student start prep? When should they take the SAT/ACT? Which AP exams are most important for this track? (Reference `references/timeline-engine.md` for testing milestones.)
- **Extracurricular alignment**: Which activities from `references/academic-tracks.md` support this track? If the student has existing activities (from state), note whether they align.
- **Summer planning**: What summer experiences would strengthen the academic trajectory? Research programs, projects, or coursework that connect to the track.

These are brief pointers that set up future commands (`testing`, `activities`, `summer`), not full workflows. Keep recommendations to 1-2 sentences each.

---

### Output Schema

Return exactly:

```markdown
## Academic Plan

### Your Track: [Track Name]
[1-2 sentences explaining why this track fits the student's interests and strengths]

### Recommended Course Sequence
| Year | Core Courses | AP/Honors | Electives | Notes |
|------|-------------|-----------|-----------|-------|
| [Grade 10] | [courses] | [courses] | [courses] | [any notes — dual enrollment, prerequisites, etc.] |
| [Grade 11] | [courses] | [courses] | [courses] | [notes] |
| [Grade 12] | [courses] | [courses] | [courses] | [notes] |

### Math Sequence
[Current position in math sequence] -> [next steps through senior year]
[Flag if behind or ahead for the track, with specific recommendations]

### Course Rigor Assessment
- Overall rigor: [Strong match / Solid with room to strengthen / Needs adjustment]
- [1-3 specific observations about how this sequence positions the student]
- [If gaps exist: specific courses to add or swap, with reasoning]

### Supporting Actions
- Testing: [brief timeline recommendation — when to prep, when to test]
- Activities: [1-2 activities from academic-tracks.md that align with this track]
- Summer: [1-2 summer experiences that would complement this plan]

**Recommended next**: `activities` — let's build your extracurricular strategy around [track]. **Alternatives**: `testing` (to plan standardized testing), `schools` (if junior+ and ready to research colleges), `help`
```

### State Update

Write to Academic Track section in `counseling_state.md`:

- **Primary track**: The selected track name
- **Secondary interests**: Any other tracks discussed or left open
- **Current course load**: Updated if new information was provided
- **Planned course sequence**: Year-by-year plan from the output
- **GPA trajectory**: Assessment based on current GPA trend (improving / steady / declining)

Also update Session Log with today's entry and Coaching Notes if the student revealed preferences or concerns about course selection (e.g., "student anxious about AP workload," "parent pushing more APs than student wants").

**Recommended next**: `activities` — let's build your extracurricular strategy around [track]. **Alternatives**: `testing` (if sophomore+), `schools` (if junior+), `help`
