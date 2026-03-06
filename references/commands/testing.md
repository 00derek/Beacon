# testing — Standardized Testing Strategy Workflow

Use `references/admissions-knowledge.md` for testing landscape, SAT vs ACT comparison, test-optional policies, PSAT/National Merit, and testing timeline. Use `references/timeline-engine.md` for testing milestone tracking.

---

### Step 1: Current Testing Status

Collect the student's testing baseline. Ask one question at a time:

1. "Have you taken the PSAT? (pick one)"
   1. Yes — I know my score
   2. Yes — don't remember the score
   3. No, haven't taken it
   (If yes with score, ask: "What was the score?")

2. "Have you taken a practice SAT or practice ACT? (pick one)"
   1. Practice SAT — I know the score
   2. Practice ACT — I know the score
   3. Both — I know the scores
   4. Yes but don't remember scores
   5. No, haven't taken either
   (If yes with scores, ask for the scores)

3. "Have you taken any official SAT or ACT? (pick one)"
   1. Official SAT
   2. Official ACT
   3. Both
   4. Not yet
   (If yes, ask: "Scores and dates?")

4. "Have you taken or are you planning to take any AP exams? (pick one)"
   1. Already taken some — let me tell you which
   2. Planning to take some this year
   3. Both — taken some and planning more
   4. Not taking any APs

Cross-reference with grade level from state to determine urgency:
- **Sophomore or earlier**: Exploratory. No urgency. Frame as "getting a baseline."
- **Junior (fall)**: Action time. Official test should happen by spring.
- **Junior (spring) with no test yet**: Urgent. Map out the fastest realistic path.
- **Senior (fall) with no test**: Check if target schools are test-optional. If any require scores, this is critical — find the next available test date.

If the student has existing scores in `counseling_state.md`, pull them and confirm: "Last time we talked, your PSAT was [X]. Has anything changed?"

### Step 2: SAT vs ACT Guidance

If the student hasn't decided between SAT and ACT:

1. Recommend taking a timed practice test of each. "The best way to decide is to try both under real conditions — a full timed practice test of each. Go with whichever feels more natural."
2. Briefly explain the key differences (pull from `references/admissions-knowledge.md` SAT vs ACT Comparison):
   - SAT: more time per question, calculator always available, reading-heavy, no separate science section, digital and adaptive
   - ACT: faster pace, separate science reasoning section, more straightforward math questions but covers more topics, no formulas provided
3. "Neither is inherently harder. Different students suit different tests."
4. Note the current testing landscape: "Multiple top schools have reinstated SAT/ACT requirements for 2025-2026. Even at test-optional schools, submitting strong scores helps. Plan to test."

If the student has already decided or taken one:
- Affirm the choice. Don't second-guess unless there's a clear mismatch (e.g., very slow reader chose ACT).
- Focus on optimizing that test.

### Step 3: Target Score Range

**If a college list exists** (pull from College List in state):
- Look up the middle-50% score ranges for each target school (use WebSearch if needed).
- Set the target as the 50th-75th percentile of the student's match schools.
- For reach schools: "Your reach schools have middle-50% ranges of [X-Y]. Getting above [Y] strengthens your case, but a score in the range is competitive."
- For test-optional considerations: "If your score lands at or above a school's 25th percentile, submit it. Below that, going test-optional may be strategic."

**If no college list exists**:
- Set general targets based on academic track and goals:
  - Highly selective schools (Ivy/Stanford/MIT tier): SAT 1500+ / ACT 34+
  - Selective private and flagship state schools: SAT 1350-1500 / ACT 30-34
  - Competitive state universities: SAT 1200-1350 / ACT 26-30
  - Open-admission or rolling schools: Scores less critical, but higher scores unlock merit aid
- "These are general ranges. Once we build your college list, we'll dial in specific targets."

Provide test-optional nuance: "Even at test-optional schools, data consistently shows that applicants who submit scores are admitted at higher rates. The practical guidance: if your score is at or above a school's 25th percentile, submit it."

### Step 4: Prep Plan

Assess the gap between current performance and target:

1. **Determine prep method** (budget-conscious recommendations first):
   - **Self-study (free)**: Khan Academy (official SAT partner — free, personalized, linked to PSAT scores). ACT Academy (free from ACT). Practice tests from College Board and ACT websites.
   - **Structured course ($100-$500)**: Online prep programs. Good for students who need structure and accountability.
   - **Private tutor ($500-$3,000+)**: Most intensive. Recommend only if self-study hasn't moved the needle after 4-6 weeks, or if the student needs to close a large gap (100+ SAT points) in limited time.
   - "I always recommend starting with free resources. Khan Academy is genuinely good for SAT prep. If you plateau after a month of consistent work, then consider a course or tutor."

2. **Build the timeline** (based on grade and test dates):
   - Recommended prep duration: 2-4 months of consistent practice (30-60 min, 4-5 days/week)
   - Map to available test dates (SAT: typically March, May, June, August, October, November, December; ACT: similar schedule)
   - First official test target date
   - Retake window: "Most students improve on a retake. Plan for one retake — it's not failure, it's strategy."

3. **Set weekly practice structure**:
   - Week 1-2: Full diagnostic, identify weak areas
   - Week 3-8: Targeted practice on weak areas (not just full tests)
   - Week 9-10: Full practice tests under timed conditions
   - Week 11-12: Final review and rest before test day

4. **Manage expectations**: "Consistent practice over 2-3 months typically yields 50-100 point improvements on the SAT. Larger jumps are possible but require more time and targeted work."

### Step 5: AP Exam Strategy

Connect AP strategy to the student's academic track and spike:

1. **AP coursework vs. AP exams**: "The coursework matters more for admissions than the exam scores. Taking AP classes shows rigor. The exam scores mainly earn college credit — a 4 or 5 can save you time and money in college."

2. **Which APs align with the student's track** (reference their academic track from state):
   - STEM spike: AP Calculus, AP Physics, AP CS, AP Statistics, AP Biology/Chemistry
   - Humanities spike: AP English Lit, AP History (US/World/Euro), AP Language, AP Government
   - Business/Econ: AP Macro/Microeconomics, AP Statistics, AP Government
   - Arts: AP Studio Art, AP Music Theory, AP Art History
   - "Take APs that align with your interests and intended major. An engineering-bound student doesn't need AP Art History, and a humanities student doesn't need AP Physics C."

3. **How many APs**: "Quality over quantity. 4-6 APs over high school is solid for selective schools. 8-10 is impressive but only if you can handle the workload. One B in an AP class is better than one A in a regular class for admissions purposes — schools want to see you challenge yourself."

4. **AP exam prep timing**: "AP exams are in May. Start focused review 4-6 weeks before the exam. Your class preparation is the main prep — supplemental review fills gaps."

5. **Score reporting**: "You choose which AP scores to send. Don't stress about a 3 — just don't send it to schools that only give credit for 4s and 5s."

Build an AP plan table based on the student's current grade and planned courses.

### Output Schema

Return exactly:

```markdown
## Testing Strategy

### Current Status
- PSAT: [score or not taken]
- Test preference: [SAT / ACT / undecided]
- Practice scores: [SAT practice: X / ACT practice: X / not taken]
- Official scores: [SAT: X (date) / ACT: X (date) / none yet]

### Target Score Range
- Based on: [college list schools / general academic track targets]
- SAT target: [range]
- ACT target: [range]
- Test-optional note: [which target schools are test-optional, submit/don't-submit guidance]

### Prep Plan
- Start prep: [date/timeframe]
- Prep method: [self-study with Khan Academy / structured course / tutor — with rationale]
- First official test: [target date]
- Retake window: [date]
- Weekly commitment: [hours/week]

### AP Exam Plan
| Exam | Year | Aligned With | Priority | Notes |
|------|------|-------------|----------|-------|
[rows — Priority: core to spike / supports narrative / optional]

**Recommended next**: `schools` — let's build your college list so we can dial in exact score targets. **Alternatives**: `plan` (to review course rigor and AP alignment), `review` (to check overall timeline), `help`
```

### State Update

Write to the **Testing** section of `counseling_state.md`:
- PSAT score
- SAT practice / ACT practice scores
- Test preference (SAT / ACT / undecided)
- Official SAT and ACT scores with dates
- Target score range
- Prep plan (method, start date, test date, retake window)
- AP exams (planned and completed, with scores)

If the student reveals stress about testing during this workflow, capture observations in **Coaching Notes** (e.g., "student is anxious about standardized testing — responded well when normalized").

Update **Active Counseling Strategy** if testing emerges as a priority focus area.
