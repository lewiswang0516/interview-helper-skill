# Interview Helper Skill - Design Spec

Date: 2026-07-06
Status: Approved by user

## Problem

Many university courses (UQ, USYD, and others) assess coding projects with an oral interview.
After submitting the project, the student attends an interview session where a tutor asks hard questions to confirm the student truly understands their own code.
Students often use AI to "help them understand" in a shallow way, and then fail under real questioning.
This skill helps students genuinely master their own project before the real interview.

## Goals

1. Guide a student step by step to fully understand their own project, including fine details.
2. When the student is ready, run a mock interview at one of three difficulty levels: Easy, Hard, Hell.
3. Each level demands a different depth of mastery and uses different question styles.
4. Produce a post-interview report that shows weak points and what to study next.

## Non-goals

- No code execution, static analysis scripts, or external tooling. The model reads the code directly.
- No cross-session score persistence beyond the report files themselves.
- Not a tool for writing or improving the assignment code itself.

## Distribution

- The skill is a self-contained folder for other students to install.
- Target install location: `~/.claude/skills/interview-helper/` (also usable on claude.ai skill upload).
- Pure prompt-driven: no scripts, no platform dependencies.

## Skill structure

```
interview-helper/
├── SKILL.md              # Entry: trigger, project intake, mode selection, control flow
└── references/
    ├── study-mode.md     # Guided understanding mode, five-layer flow
    ├── levels.md         # Three difficulty levels: question strategy, judging, follow-up rules
    └── report.md         # Post-interview report template and grading standard
```

- `SKILL.md` stays under ~150 lines; details live in `references/` and load on demand.
- Auto-trigger: the description covers keywords such as interview, viva, code walkthrough, oral assessment, code interview, plus Chinese equivalents (拷打, 代码面试, 作业答辩).
- Manual trigger: `/interview-helper`.

## Startup flow (shared by both modes)

1. Confirm the project path to examine. Read the code and build an "examiner view": module structure, key design decisions, likely exam points, suspected weak spots.
2. Ask once for optional extra inputs:
   - Assignment spec / marking rubric / lecture material. If given, questions target rubric points.
   - A custom question list (past interview questions, course question bank, or the student's own worry list). Any format: file, markdown, or pasted text.
3. Student picks a mode: `study` or `interview`.

## Study mode (guided understanding)

Socratic, five layers, strictly in order.
At each layer the student must explain first; the AI only corrects and fills gaps, never lectures first.

1. Project overview: what the program does, entry point, overall flow.
2. Module/file level: each part's responsibility and how parts connect.
3. Function/line level: walk key functions one by one, including "why is this line written this way".
4. Design decisions: why this data structure/algorithm, what the alternatives were.
5. Edges and weak spots: what inputs break it, where a tutor would likely attack.

Each layer ends with a short self-check quiz; the student must pass it to move on.
If a custom question list exists, its questions feed the self-check quizzes, so after study mode the student can handle every question on the list.
Study mode follows the student's language.

## Interview mode (three levels)

Shared rules:

- Questions are asked in English (realistic, and trains spoken expression).
- One question at a time.
- No hints, no on-the-spot correction. Behave like a real tutor: only follow up.
- Silently track per-question performance; never reveal it mid-round.
- The student can stop at any time.
- Question budget per round: Easy ~8, Hard ~10, Hell ~12. When the budget is spent, the round ends.
- If the student asks for the answer mid-round, refuse (stay in character), record it, and explain in the report afterwards.

Custom question list integration:

- List questions are mixed in first, before AI-generated questions fill the remaining budget.
- The same list question is treated by level: Easy accepts a surface answer; Hell chases follow-ups to the bottom.
- Unused list questions carry over to the next round and are noted in the report.

| Level | Core focus | Style and tolerance |
|---|---|---|
| Easy | "What" | Code visible. Function descriptions, data flow, basic concepts. A roughly correct answer passes. Answers may be in Chinese or English. |
| Hard | "Why" | Design decisions, alternatives, edge cases, complexity, "what happens if X becomes Y". Accuracy required; 1-2 follow-ups per question. Chinese or English answers accepted. |
| Hell | "Did you really write this" | Everything in Hard, plus: integrity-interrogation style ("What were you thinking here? Where did you learn this pattern?"), trap questions (the tutor states something wrong to see if the student agrees blindly), live coding (describe a section without looking at the code, or write a modification on the spot). Follow-ups go to the bottom; near-zero tolerance; answers must be in English. |

## Post-interview report

Written as a markdown file into the student's project directory, named `interview-report-<date>.md`.
Report language follows the student.

Contents:

- Per question: the question, key points of the student's answer, and a grade (mastered / fuzzy / failed).
- Questions from the custom list are marked separately, so the student sees performance on "real past questions" at a glance.
- Aggregated weak-point list, deduplicated, pointing at concrete code locations.
- Overall verdict: predicted performance in the real interview at the current state.
- Concrete advice: which study-mode layers and which files to revisit.

## Language policy

- Interview questions: English at all levels.
- Student answers: Chinese or English at Easy/Hard; English required at Hell.
- Study mode and reports: follow the student's language.

## Error handling

- Project path invalid or unreadable: say so and ask again; never invent code content.
- Empty or trivial project: state that there is not enough material for a full round and shrink the budget accordingly.
- Custom question list unreadable: report which part failed to parse instead of silently dropping it.

## Testing / acceptance

- Install the skill locally and dry-run both modes against a real course project.
- Check: auto-trigger fires on interview-related phrasing; levels differ visibly in depth and tolerance; report file is produced and matches the template; custom list questions appear first and are marked in the report.
