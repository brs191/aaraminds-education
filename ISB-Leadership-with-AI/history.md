# ISB Leadership with AI — Extraction History & Resume Notes

Tracks what has been crawled/extracted from the Emeritus classroom so the work can be resumed cleanly when new modules open.

- **Course:** ISB Certificate Programme in Leadership with AI
- **URL:** https://classroom.emeritus.org/courses/17878
- **Canvas course ID:** 17878
- **Programme length:** 20 weeks (new module released every Wednesday)
- **Last extraction run:** 2026-05-25

## Status

| Module / Week | Released? | Module content | Transcript | Summary deck |
|---|---|---|---|---|
| Programme Orientation | yes | done (`00-Programme-Orientation.md`) | n/a | n/a |
| Pre-programme / Legal Name Survey | yes | done (`01-Pre-programme.md`) | n/a | n/a |
| Week 1 — AI and ML: An Overview | yes | done | done | done |
| Week 2 — AI and ML Technologies and Use Cases | yes | done | done | done |
| Week 3 — AI and Managerial Trade-offs | yes | done | done | done |
| Week 4 — Setting-up Stage for AI-Led Business Transformation | yes | done | done | done |
| Week 5 — Competing in the Age of AI Disruption | yes | done | done | done |
| Week 6 — Business Strategy in the Age of Generative AI | yes | done | done | done |
| Pre-Module Essentials | yes | done (`08-...`) | n/a | n/a |
| Week 7 — Foundations of Generative AI | yes | done | done | done (image-based; text in transcript) |
| Week 8 — Prompt Engineering and Techniques | not yet (opens ~May 27, 2026) | pending | pending | pending |
| Week 9 — Gen AI for Leaders | not yet (~Jun 03, 2026) | pending | pending | pending |
| Week 10 — Leveraging AI for Competitive Advantage | not yet (~Jun 10, 2026) | pending | pending | pending |
| Week 11 — Implementing AI in Organisations | not yet (~Jun 17, 2026) | pending | pending | pending |
| Week 12 — Building Blocks to Leadership I + Bonus | not yet (~Jun 24, 2026) | pending | pending | pending |
| Week 13 — Building Blocks to Leadership II + Bonus | not yet (~Jul 01, 2026) | pending | pending | pending |
| Week 14 — AI-Led Workforce and Organisation | not yet (~Jul 08, 2026) | pending | pending | pending |
| Week 15 — Leadership in the Age of AI I | not yet (~Jul 15, 2026) | pending | pending | pending |
| Week 16 — Leadership in the Age of AI II | not yet (~Jul 22, 2026) | pending | pending | pending |
| Week 17 — Ethics, Privacy, Regulations in AI and ML | not yet (~Jul 29, 2026) | pending | pending | pending |
| Weeks 18-20 — Capstone | not yet (~Aug 05, 2026; due Aug 26, 2026) | pending | pending | pending |

**Resume point: Week 8 (Prompt Engineering and Techniques), expected to open ~May 27, 2026.**

## What "done" produced

- `00-INDEX.md` — master index of all files.
- `00-...` to `08-...md` — module/week content: instructions, video summaries, discussion prompts, **assignment briefs**, resource links.
- `transcripts/Week1-7-Transcript.md` — full word-for-word lecture transcripts.
- `summary-decks/Week1-7-Summary-Deck.md` — slide-by-slide deck text.

## Required assignments captured (Weeks 1-6)

1.2 Applications of AI (25 pts) · 2.2 AI and ML Use Cases (15) · 3.2 AI Implementation Roadmap (25) · 4.2 Case Study PDD Business Model (10) · 5.2 Servitisation in the Auto Industry (20) · 6.2 Leveraging Generative AI in Marketing or Entertainment (20). Week 7 had no required assignment.

## Decisions / scope

- Video files were **not** downloaded and videos were **not** auto-transcribed — the course IP policy prohibits recording/transcribing/distributing materials. Official written transcripts (provided by the course) were extracted instead.
- Binary PDFs were **not** copied into the folder (they sit behind the Emeritus login, no clean automated pipe). Their **text** was extracted instead.

## How to resume (method that worked)

When new weeks open, repeat this process:

1. In Chrome (logged in to Emeritus), connect the Claude in Chrome extension.
2. Pull the module map via the Canvas API in-browser:
   `fetch('/api/v1/courses/17878/modules?include[]=items&per_page=100')`
3. For each module item, fetch page/assignment/discussion bodies via:
   - Pages: `/api/v1/courses/17878/pages/<page_url>`
   - Assignments: `/api/v1/courses/17878/assignments/<content_id>`
   - Discussions: `/api/v1/courses/17878/discussion_topics/<content_id>`
4. For transcript/summary-deck PDFs: get the file's download URL from
   `/api/v1/courses/17878/files/<id>`, then extract text in-browser with PDF.js
   (`cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174`).
5. Save new weeks as `09-Week8-...md` etc., plus `transcripts/Week8-Transcript.md`
   and `summary-decks/Week8-Summary-Deck.md`. Update `00-INDEX.md` and this file.

### Known file-ID pattern

Week 1-7 transcript/deck Canvas file IDs were in the 62414xx-62415xx range. New weeks will have new IDs — always look them up fresh from the module's "Video Transcript and Summary Deck" page.

## Changelog

- 2026-05-25 — Initial crawl: 11 modules / 145 items; Weeks 1-7 transcripts + summary decks extracted.
