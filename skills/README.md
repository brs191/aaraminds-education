# Education workspace skills

Local Claude Skills scoped to this learning workspace. They are **not** part of
the AaraMinds brain (`aaramind/`) — per workspace governance, personal coursework
tooling lives with the coursework, not in the company brain. They do, however,
*compose* the canonical AaraMinds Instruction OS modules at load time.

## Skills

### `aaraminds-academic-assignment-coach`

A rubric-aware **drafting scaffold** for MIT xPRO and ISB assignments. It parses
the assignment rubric, grounds a structured draft in the course's own
transcripts/module notes plus your real experience, and emits a Word document
with every unverified claim flagged (`[VERIFY]` / `[YOUR EXPERIENCE]`) for you to
resolve before submission.

**Design posture:** scaffold, not submission. It will not produce a final answer
to be submitted without your reading, verifying, and personalising it — by
design, to respect both programmes' academic-integrity policies.

**How to invoke**

- In a Cowork/Claude session with this folder connected, just describe the task:
  *"Help me draft MIT Assignment 4.1"* or *"Draft ISB 5.2 — Servitisation."*
- The skill reads the matching assignment `.md` under
  `MIT-Technology-Leadership-Course/Assignments/` or
  `ISB-Leadership-with-AI/Assignments/`, pulls the relevant transcripts and
  module notes, then runs its 7-step workflow (parse rubric → gather grounding →
  map rubric to content → outline → draft → self-check → emit `.docx`).

**Dependencies**

- Composes modules from the AaraMinds workspace (`C:\aaraminds\instruction-os/`).
  If that folder isn't present, the skill will ask where it is.
- Uses the `docx` skill to produce the final Word document.

**Two things the local archive does NOT contain (supply them for best results)**

- **The detailed grading rubric.** Local files have instructions, questions,
  points, and learning outcomes — but not the per-criterion scoring grid, which
  lives in the login-gated Canvas template. Paste the rubric when prompted, or
  the skill falls back to the learning outcomes as a proxy.
- **The blank upload template** (`.docx`/`.xlsx`). Not downloaded. Hand the skill
  the template file if the course requires its exact form; otherwise it produces
  a clean document mirroring the required sections, to transcribe before upload.

**Course layout note:** MIT assignment prompts are separate `.md` files; ISB
prompts are embedded in the module notes (`02-…`–`07-…`) plus brief PDFs under
`Assignments/Archieve/`. The skill knows this — it's documented in its layout map.
