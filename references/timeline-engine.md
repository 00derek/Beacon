# timeline-engine — Grade-Aware Milestone Tracker

This module powers proactive timeline awareness across the AI Counselor skill. It is loaded by `kickoff`, `plan`, `testing`, `apply`, `summer`, and `review` commands, and drives the session greeting logic.

---

## Session Greeting Logic

Every session begins with a timeline-aware greeting. The counselor checks the student's **grade level** and **current date** against the milestone tables below.

**How it works:**

1. Read `counseling_state.md` for grade level and target graduation year.
2. Determine current semester from the date:
   - **Fall semester**: August 15 – December 31
   - **Spring semester**: January 1 – May 31
   - **Summer**: June 1 – August 14
3. Scan all milestones for the student's current grade and earlier grades.
4. Assign a status label to each milestone.
5. Surface the 2-3 most actionable items in the greeting.

**Greeting format:**

```
Hey [Name]! You're a [grade] in your [fall/spring] semester — here's where things stand:

[Icon] [Milestone] — [Status]
[Icon] [Milestone] — [Status]
[Icon] [Milestone] — [Status]

[Recommendation: what to work on today based on status]
```

**Status icons:**

| Icon | Status |
|------|--------|
| `>>` | Ahead |
| `--` | On Track |
| `..` | Coming Up |
| `!!` | Behind |
| `  ` | N/A |

---

## Status Label Definitions

| Label | Meaning | How to assign |
|-------|---------|---------------|
| **Ahead** | Milestone completed before its expected semester | Student finished this before the semester listed in the table below |
| **On Track** | Completed during the expected semester | Student completed this during the semester it appears in |
| **Coming Up** | Due this semester, not yet done | Current semester matches the milestone's semester, but the milestone is incomplete |
| **Behind** | Past the expected semester, not completed | Current date is past the milestone's semester and it remains incomplete |
| **N/A** | Not yet relevant | Milestone belongs to a future grade level or semester |

---

## Framing Guidance

**"Behind" milestones are opportunities, not failures.**

When a student has "Behind" milestones, use Motivational Interviewing voice. Never shame, guilt, or catastrophize. Frame the gap as an opportunity to catch up, and normalize that timelines are guides, not rigid deadlines.

**Examples of MI-framed "Behind" language:**

- "You haven't taken a practice SAT or ACT yet — that's totally fine. A lot of students pick this up later. Want to talk about when might work for you?"
- "It looks like you haven't started building a college list yet. No stress — you've got time, and now is actually a great moment to start. Want to explore that today?"
- "I see you haven't asked for recommendation letters yet. That's one of those things that feels awkward but goes better than you'd expect. Want to talk through who to ask and how?"

**Never say:**

- "You're behind on X"
- "You should have done this by now"
- "You're falling behind your peers"
- "This is late"

**Instead say:**

- "This is something that's coming up — want to work on it?"
- "A lot of students tackle this around [semester]. You've got a chance to jump on it now."
- "This is one you haven't gotten to yet. No judgment — let's figure out a plan."

---

## Milestone Tables

### Freshman Year (9th Grade)

#### Fall Semester

| Milestone | Category | Details |
|-----------|----------|---------|
| Complete initial interest discovery | Exploration | Run through at least Phase 1-3 of the elicitation framework (warm-up, flow detection, strengths mapping). The goal is a preliminary sense of interests, not a final answer. Interests will evolve. |
| Join 2-3 clubs or activities aligned with emerging interests | Activities | Breadth over depth this year. Try different things. No leadership pressure yet. This is the exploration year — the goal is exposure, not commitment. |
| Establish strong study habits, target high GPA | Academics | Cumulative GPA starts now and is easier to maintain than recover. Build systems: planner/calendar, consistent homework routine, active study techniques (recall, spaced repetition). |
| Take honors-level courses where confident | Academics | Don't overload, but don't coast. Take honors in subjects where the student feels strong. 4 core subjects + 1-2 exploratory electives is a good baseline. |
| Meet with school counselor to map 4-year plan | Academics | Get the lay of the land: what courses are offered, what AP options exist, what graduation requirements apply. This informs everything else. |

#### Spring Semester

| Milestone | Category | Details |
|-----------|----------|---------|
| Identify 1-2 "spike" activities to go deeper on | Activities | From the 2-3 activities tried in fall, which ones pulled the student in? Start narrowing. A spike = deep, sustained engagement in 1-2 areas, not surface participation in many. |
| Map preliminary academic track | Academics | Based on emerging interests, sketch out which track fits (STEM, business, humanities, arts, pre-med, trades, etc.). This is tentative — it will likely shift, and that's fine. Use the `plan` command. |
| Explore electives to discover additional interests | Exploration | Encourage trying at least one elective outside the student's comfort zone. Art student takes a coding class. Math student tries debate. Breadth now enables depth later. |

#### Summer

| Milestone | Category | Details |
|-----------|----------|---------|
| First meaningful summer experience | Summer | This does not need to be a prestigious program. A part-time job, volunteering, a personal project, a camp, or a family responsibility all count. The point is doing something intentional, not padding a resume. |

---

### Sophomore Year (10th Grade)

#### Fall Semester

| Milestone | Category | Details |
|-----------|----------|---------|
| Take leadership role in spike activity | Activities | Move from participant to contributor: committee chair, section leader, team captain, project lead. Show progression, not just membership. |
| Begin honors/AP coursework aligned with track | Academics | Increase rigor with 1-2 AP or Honors courses. Good first APs by track: AP Human Geography (accessible), AP World History (humanities), AP Computer Science Principles (tech), AP Environmental Science (science-curious). |
| Take PSAT in October | Testing | Serves dual purpose: practice for SAT AND qualifier for National Merit Scholarship Program (NMSQT). Scores don't go to colleges — low-stakes practice. |
| Identify emerging "spike" and begin deepening | Activities | By now, the student should have a clearer sense of which 2-3 activities matter most. Begin increasing weekly hours and seeking impact opportunities in those areas. |

#### Spring Semester

| Milestone | Category | Details |
|-----------|----------|---------|
| Take one practice SAT and one practice ACT | Testing | Try both. They test similar knowledge but with different formats, pacing, and emphasis. Go with whichever feels more natural. Neither is inherently harder — different students suit different tests. SAT: adaptive, calculator always available, 2h14m. ACT: linear, separate science section, 2h55m. |
| Research summer programs (Tier 1-3) | Summer | Tier 1 (free, selective: RSI, SSP, MITES) applications due December-February. Tier 2 (LaunchX, YYGS, Beaver Works) similar timelines. Tier 3 (university pre-college, paid) are good for exploration but less impressive for admissions. Self-directed projects are equally valid. |
| Start building preliminary college list | Schools | Casual research phase: browse schools, attend virtual info sessions, notice what appeals. Not a final list — just building awareness of what's out there. |
| Plan junior year course load with counselor | Academics | Junior year is the most important academic year for college applications. Course selection here is strategic: take APs aligned with the intended track, maintain or increase rigor. Meet with the school counselor to finalize. |
| Seek increasing responsibility in activities | Activities | Committee roles, mentoring newer members, organizing events. The narrative is progression: joined freshman year, contributed sophomore year, led junior year. |

#### Summer

| Milestone | Category | Details |
|-----------|----------|---------|
| Tier 1/2 program or self-directed project | Summer | Apply to selective programs if aligned with interests. If not accepted or not interested in formal programs, a self-directed alternative can be equally powerful: start a meaningful project, cold-email a professor for research, get a real job, or deep-dive into an independent study with tangible output. |

---

### Junior Year (11th Grade)

**This is the most critical year for college applications.** Junior year grades carry the most weight, testing happens, the college list takes shape, and the application narrative begins to form.

#### Fall Semester

| Milestone | Category | Details |
|-----------|----------|---------|
| Finalize course rigor (AP/IB alignment with track) | Academics | Peak rigor year: 3-4 AP courses recommended for competitive colleges. Take APs that align with intended major/track. This year's grades matter MOST. Admissions officers evaluate course rigor relative to what the school offers. |
| SAT/ACT first official attempt | Testing | Take the first official sitting in fall or early spring. If the student took practice tests sophomore year, they should have a test preference by now. Begin targeted prep 2-3 months before the test date. |
| Begin college research in earnest | Schools | Move from casual browsing to structured research. Use the `schools` command to build a reach/match/safety list. Attend virtual info sessions. Research specific programs, professors, and culture. |
| Compete at highest level in spike activities | Activities | This is the year for awards, competitions, and peak achievement in core activities. State/national competitions, leadership positions, and measurable impact. |

#### Winter

| Milestone | Category | Details |
|-----------|----------|---------|
| Start brainstorming personal statement topics | Essays | Begin the Socratic brainstorming process — not drafting yet, but exploring what stories, moments, and themes might anchor the personal essay. Use the `essays` command. Focus on depth of reflection, not drama of the topic. |

#### Spring Semester

| Milestone | Category | Details |
|-----------|----------|---------|
| SAT/ACT retake if needed | Testing | Most students improve on a second attempt. If the first score is within range of the target, one retake is usually sufficient. Study the score report to identify specific weak areas. |
| Visit colleges (or virtual tours) | Schools | In-person visits are ideal (sign in — some schools track demonstrated interest). Virtual tours and info sessions are good alternatives. Focus on match and reach schools. |
| Ask 2 teachers for recommendation letters | Applications | Ask in person, privately. Choose junior year core subject teachers who know the student as a person, not just a grade. One teacher should align with the spike/intended major. Give them a "brag sheet" with activities, accomplishments, goals, and specific class memories. Give at least 1 month before the earliest deadline. |
| Run net price calculators on target schools | Financial | Every school has a net price calculator on its website. Run them for the top 5-8 schools to get a realistic sense of cost. This prevents sticker shock and informs the college list. |
| Take AP exams | Testing | AP scores of 4-5 demonstrate mastery. Some selective schools (e.g., Yale) now accept AP scores in lieu of SAT/ACT. AP credit can also reduce college costs. |
| Register for AP exams (if taking AP courses) | Testing | Registration typically happens in the fall/winter for May exams, but confirm deadlines with the school. |

#### Summer (Before Senior Year)

| Milestone | Category | Details |
|-----------|----------|---------|
| Draft personal statement | Essays | Multiple drafts over the summer. Use the Socratic process — don't write FOR the student. The essay should reveal something about who they are, not summarize their resume. Show, don't tell. Go small to go deep. |
| Finalize college list (2-3 reach, 4-5 match, 2-3 safety) | Schools | 8-12 schools total. Every student needs at least 2 true safeties — schools where admission is very likely AND the student would genuinely attend. |
| Research supplemental essay prompts for target schools | Essays | Most supplements are available by summer. Start thinking about "Why This School?" and "Why This Major?" essays. Specificity is what separates strong supplements from generic ones. |
| Create application accounts (Common App, Coalition, UC) | Applications | Set up accounts, input basic info, start the activities section. The Common App activities section allows 10 activities with 150-character descriptions — these need to be impact-focused and carefully worded. |
| Prepare "brag sheet" for recommenders | Applications | A document for each recommender including: activities and accomplishments, specific memories from their class, intended major, college list, and what you hope they can speak to. |

---

### Senior Year (12th Grade)

#### September

| Milestone | Category | Details |
|-----------|----------|---------|
| Finalize ED/EA/RD strategy | Applications | Decide which school (if any) gets an ED application (binding). File EA applications (non-binding early) for schools where available. Map remaining schools to RD. Use the `apply` command. |
| Finalize and polish personal statement | Essays | The personal essay should be done by early September. Multiple rounds of Socratic revision, read aloud for voice, get feedback from a trusted reader (teacher, counselor, parent). |
| Prepare activities list with impact-focused descriptions | Applications | Each of the 10 Common App activities gets 150 characters. Formula: [leadership verb] + [specific scope/scale] + [measurable impact]. "Led 12-member team to state finals; designed autonomous system; mentored 8 freshmen" beats "Member of robotics club for 3 years." |

#### October

| Milestone | Category | Details |
|-----------|----------|---------|
| FAFSA opens — file early | Financial | Submit as soon as possible after October 1. Some aid is first-come, first-served. Gather tax documents in advance. FAFSA is free and required by virtually all schools for any financial aid. |
| CSS Profile submitted (if needed) | Financial | Required by ~200 mostly private schools for institutional aid. Costs $25 first school + $16 each additional (fee waivers available). More detailed than FAFSA — includes home equity, business assets, non-custodial parent income. Unlocks access to $14+ billion in non-federal aid. |
| SAT/ACT final retake if needed | Testing | Most colleges accept October/November scores for RD. This is the last realistic testing window. |

#### November

| Milestone | Category | Details |
|-----------|----------|---------|
| ED/EA/REA applications submitted (Nov 1-15) | Applications | Early Decision is binding — only for a clear first-choice school. Early Action is non-binding. Restrictive Early Action (Harvard, Stanford, Yale, Princeton) means you cannot apply EA/ED elsewhere. |
| UC applications submitted (November) | Applications | One application covers all UC campuses. 4 Personal Insight Questions (350 words each) chosen from 8 prompts. No letters of recommendation required (with some exceptions). |
| Complete and submit RD supplemental essays | Essays | Don't wait until January. Supplement quality drops when students rush. Write "Why This School?" essays with hyper-specific details — name professors, courses, programs, traditions. |

#### December

| Milestone | Category | Details |
|-----------|----------|---------|
| EA/ED decisions arrive | Decisions | If accepted ED, withdraw all other applications and commit. If deferred, send an update letter with new achievements, continued interest, and any significant updates (awards, grades, scores). If rejected, process the feelings and redirect energy to RD schools. |

#### January

| Milestone | Category | Details |
|-----------|----------|---------|
| RD applications submitted (Jan 1-15) | Applications | Final deadline for most schools. Double-check every application: supplements attached, scores sent, recommenders confirmed, FAFSA/CSS linked. |
| Send mid-year grade reports | Applications | Schools receive mid-year transcripts. Senior year grades still matter — admissions can be rescinded for significant grade drops. Maintain rigor. |

#### February-March

| Milestone | Category | Details |
|-----------|----------|---------|
| ED2 decisions (if applicable) | Decisions | Second chance at binding early commitment for students who were deferred/rejected ED1 or discovered a new first-choice school. |
| Scholarship applications (ongoing) | Financial | Local scholarships have less competition than national ones. Check with the school counselor, community organizations (Rotary, Lions Club, local businesses). National: Fastweb, Bold.org, College Board Scholarship Search. Smaller amounts ($500-5,000) add up. |
| Continue applying for scholarships | Financial | Many merit scholarships require essays. Reuse and adapt Common App essay themes. Apply broadly — many have fewer applicants than expected. |

#### March-April

| Milestone | Category | Details |
|-----------|----------|---------|
| RD decisions arrive | Decisions | Compare offers across all dimensions: academics, campus culture, financial aid, location, specific program strength. Don't decide on prestige alone. |
| Compare financial aid packages | Financial | Aid packages are not always apples-to-apples. Look at grants vs. loans vs. work-study. Check the net cost (total cost minus grants/scholarships). You can appeal financial aid offers — yes, you can negotiate. |
| Revisit campuses (admitted student events) | Schools | Attend admitted student days, sit in on classes, talk to current students. The vibe on a visit as an admitted student is different from a prospective visit. |

#### May

| Milestone | Category | Details |
|-----------|----------|---------|
| National Decision Day — commit by May 1 | Decisions | Commit to one school. Send your deposit. Notify all other schools you are declining. This is a firm deadline — missing it can forfeit your spot. |
| Take final AP exams | Testing | AP scores can earn college credit and save money. Even after committing, strong AP scores in May matter. Check the admitted school's AP credit policy. |
| Celebrate | Decisions | The journey is done. Acknowledge the work, the growth, and the student's agency in getting here. |

---

## Timeline Check Output Format

When the `review` command or session greeting runs a timeline check, output uses this format:

```
## Timeline Check — [Name], [Grade] ([Semester] [Year])

### Coming Up This Semester
.. [Milestone] — [one-line action step]
.. [Milestone] — [one-line action step]

### Behind (Opportunities to Catch Up)
!! [Milestone] — [MI-framed encouragement + suggested next step]

### On Track
-- [Milestone] — [brief acknowledgment]

### Ahead
>> [Milestone] — [brief acknowledgment + what this enables]

### Looking Ahead (Next Semester)
[1-2 items from the next semester to be aware of, framed as preview, not pressure]
```

**Rules for the timeline check output:**

1. Lead with "Coming Up" — what the student can act on now.
2. Frame "Behind" items with MI voice — opportunities, not failures.
3. Acknowledge "On Track" and "Ahead" items to build confidence.
4. Keep "Looking Ahead" brief — awareness, not overwhelm.
5. Never list more than 5 items in any section.
6. If the student is ahead on most milestones, celebrate that and focus on what's next.
7. If the student is behind on multiple milestones, prioritize the 2-3 most impactful ones to address first. Do not pile on.

---

## Milestone Categories

Each milestone belongs to one of these categories. Categories help the counselor route to the right command and help the student see the big picture of their progress.

| Category | Description | Primary Command |
|----------|-------------|-----------------|
| **Exploration** | Interest discovery, self-reflection, values clarification | `discover` |
| **Academics** | Course selection, GPA, study habits, academic rigor | `plan` |
| **Activities** | Clubs, sports, leadership, spike development, extracurriculars | `activities` |
| **Testing** | PSAT, SAT, ACT, AP exams, test prep strategy | `testing` |
| **Schools** | College research, list building, campus visits, demonstrated interest | `schools` |
| **Essays** | Personal statement, supplemental essays, Socratic brainstorming | `essays` |
| **Applications** | Common App, recommendation letters, application strategy (ED/EA/RD) | `apply` |
| **Financial** | FAFSA, CSS Profile, scholarships, net price, financial aid comparison | `financial` |
| **Summer** | Summer programs, projects, jobs, internships, volunteering | `summer` |
| **Decisions** | Admissions decisions, comparing offers, commitment | `review` |

---

## Integration Points

**Loaded by:** `kickoff`, `plan`, `testing`, `apply`, `summer`, `review`

**Trigger conditions:**
- Every session greeting (read state, check grade + date, surface milestones)
- The `review` command runs a full timeline check across all categories
- Commands that modify state (e.g., `testing` updating scores, `apply` updating application status) should update milestone statuses in `counseling_state.md`

**Interacts with:**
- `counseling_state.md` — reads grade, graduation year, and milestone completion status; writes updated statuses after changes
- `elicitation-frameworks.md` — the Exploration milestones map to the elicitation phases
- `academic-tracks.md` — the Academics milestones reference track-specific course sequences
- `admissions-knowledge.md` — the Applications and Schools milestones reference admissions timelines and strategy
