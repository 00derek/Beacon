# Beacon — Version Roadmap

## v1 (current)

Full 4-year college application journey.

- **12 commands**: kickoff, discover, plan, activities, testing, schools, essays, apply, financial, summer, review, help
- **Timeline engine**: Grade-aware milestone database from freshman fall through senior spring. Every session checks the student's position against the timeline and surfaces upcoming deadlines, overdue items, and priority actions.
- **Persistent state**: `counseling_state.md` tracks profile, interests, academic track, activities, testing, college list, essays, financial aid, summer plans, recommendations, timeline status, coaching notes, and active counseling strategy across sessions. Auto-saves after every major workflow.
- **Socratic essay coaching**: Guides students to find their story and voice through questions. Uses anonymized strong examples for illustration. Never writes content for the student. Covers personal statements, supplementals, and activity descriptions.
- **Spike strategy**: "Well-lopsided" extracurricular development. Four-tier activity ranking (National, State, School, Community). Guides students from participation to leadership to impact to recognition. Spike awareness threads through every command.
- **Motivational Interviewing voice**: Open-ended questions (70%+), affirmations, reflective listening, rolling with resistance. Directness calibrated 1-5 per student preference. Assessment stays honest at every level.
- **Safety protocol**: Crisis signal detection (self-harm, abuse, suicidal ideation) with immediate resource routing (988 Lifeline, school counselor, therapist). Reality check layer for self-destructive academic choices. Never diagnoses, never attempts therapy.
- **Nine academic tracks**: Engineering, CS, Pre-Med, Business, Law-Humanities, Psychology-Education, Arts-Design, Trades-CTE, Undecided. Each with recommended course sequences, activity alignments, and testing strategy.
- **Interest elicitation frameworks**: Holland Code (RIASEC), flow activities, Ikigai, Gardner's Multiple Intelligences, VIA Character Strengths. Structured question banks for each.
- **College list building**: Reach/match/safety calibration with fit analysis across academics, activities, culture, and cost. Demonstrated interest strategy. Live web research for school-specific data.
- **Financial aid guidance**: FAFSA and CSS Profile walkthroughs, net price calculator direction, scholarship strategy, aid comparison and negotiation coaching.
- **Equity check**: Always active. Validates all paths (trades, CTE, community college, gap years, military) with equal respect. Reach schools for every student. First-gen awareness. Financial sensitivity. Anti-bias enforcement.
- **Gamification**: Progress pulses every 3-4 questions, micro-summaries for real-time insight, session chunking with named chapters and natural stopping points, achievement system with journey milestones, streaks, depth, and engagement badges.
- **Dashboard command**: `dashboard` generates a self-contained HTML page showing journey progress, achievements, timeline status, stats, and college list. Opens in the browser — no server needed.

## v2 (planned)

Coaching depth.

- **Formal Holland Code assessment integration**: Full structured RIASEC assessment with scoring, not just conversational elicitation. Validated question sets with quantified results that feed directly into track and activity recommendations.
- **School-specific intelligence**: Acceptance rate trend analysis, program ranking research, department-level strengths, and campus culture signals via structured web research. Deeper than current "look it up" -- builds a school intelligence profile that persists in state.
- **Essay revision tracking**: Version history for essay coaching sessions. Track how the student's personal statement evolves from brainstorming through final draft. Surface patterns in revision quality (getting sharper vs. losing voice).
- **Recommendation letter coaching depth**: Structured guidance for students on how to approach recommenders, what materials to provide (brag sheet), timeline management, and how to choose recommenders who can speak to specific competencies that fill gaps in the application.
- **Interview prep for college interviews**: Preparation coaching for alumni interviews and on-campus interviews. Common question frameworks, school-specific research, practice with feedback, and post-interview debrief.

## v3 (planned)

Full lifecycle.

- **Waitlist strategy**: Structured guidance when a student is waitlisted -- whether to stay on the list, how to write a Letter of Continued Interest, what additional information to send, and realistic odds assessment.
- **Gap year planning**: For students who aren't excited about any option or want to defer. Structured planning for a meaningful gap year -- programs, independent projects, work experiences, and how to communicate the gap year to deferred schools.
- **Decision-making framework**: Systematic comparison of admission offers across fit, financial aid, programs, campus culture, location, and career outcomes. Helps students articulate what matters most and make the decision their own rather than defaulting to prestige.
- **Transfer student support**: Guidance for students entering the process from a different starting point -- community college transfers, school transfers mid-high-school, international students adapting to US admissions, and students with non-traditional academic histories.
- **Portfolio review coaching**: For students applying to art, architecture, design, film, or other portfolio-based programs. Guidance on portfolio curation, artist statements, and program-specific requirements.

## v4 (planned)

Platform.

- **Web interface**: Browser-based UI for students and families who prefer a visual experience over the command line. Session state syncs between CLI and web.
- **Collaborative parent/student views**: Shared dashboards where parents see timeline status, milestone progress, and financial aid information while essay content and coaching notes remain student-private.
- **Calendar integration**: Sync milestone deadlines, testing dates, application deadlines, and campus visit schedules to Google Calendar, Apple Calendar, or Outlook.
- **School counselor integration**: Tools for school counselors to monitor caseload progress across multiple students, identify students who are falling behind on milestones, and coordinate with the AI counselor's recommendations.
- **Live dashboard**: Real-time browser dashboard that watches `counseling_state.md` for changes and auto-updates. Replaces the snapshot-based `dashboard` command with a persistent, always-current view using file watching and WebSocket or polling.
