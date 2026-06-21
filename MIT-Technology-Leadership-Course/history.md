# Crawl History — MIT xPRO Advanced Program in Technology Leadership & Innovation

Course: Emeritus classroom, course ID **16614** (March 2026 cohort)
URL: https://classroom.emeritus.org/courses/16614

## Status

| Date | Action | Result |
|---|---|---|
| 2026-05-25 | Initial full crawl | Captured all published content: Orientation + Pre-course + Pillars I & II (Modules 1–5), incl. all 54 video transcripts |

**Last crawled through:** Module 5 — Supply Chain and Computational Approaches to System Thinking (released May 21, 2026).

## Captured so far (do NOT re-crawl)

| Canvas module ID | Module | Items | Output file |
|---|---|---|---|
| 226276 | Course Orientation | 12 | `00-Course-Orientation.md` |
| 226277 | Pre-course Expectations | 4 | `01-Pre-Course-and-Capstone-Overview.md` |
| 226278 | Capstone Component Project | 1 | `01-Pre-Course-and-Capstone-Overview.md` |
| 226279 | Legal Name Survey | 1 | `01-Pre-Course-and-Capstone-Overview.md` |
| 232531 | PILLAR I header | 1 | `02-Module-1-...md` |
| 232532 | Module 1: Introduction to Critical Thinking | 30 | `02-Module-1-...md` + `Transcripts/Module-1-...md` |
| 233963 | Module 2: Critical Thinking in Context | 30 | `03-Module-2-...md` + `Transcripts/Module-2-...md` |
| 234972 | Module 3: Structured Decision Making | 35 | `04-Module-3-...md` + `Transcripts/Module-3-...md` |
| 236608 | PILLAR II header | 1 | `05-Module-4-...md` |
| 236609 | Module 4: Foundations of Systems Thinking | 57 | `05-Module-4-...md` + `Transcripts/Module-4-...md` |
| 237771 | Module 5: Supply Chain & Computational Approaches | 42 | `06-Module-5-...md` + `Transcripts/Module-5-...md` |

## RESUME HERE — content not yet released as of 2026-05-25

Modules unlock weekly on **Thursdays**. Next file numbers to use: `07-`, `08-`, etc.

| Week / Module | Title | Releases (Thursday) |
|---|---|---|
| Module 6 | Modern System Architecture | June 4, 2026 |
| Module 7 | Modeling with DSM and Modularization | June 11, 2026 |
| Module 8 | Value-Oriented Decision Making (+ Capstone Component 2) | June 18, 2026 |
| Module 9 | Creating an AI-Based Product | July 2, 2026 |
| Module 10 | Machine Learning | July 9, 2026 |
| Module 11 | Deep Learning | July 16, 2026 |
| Module 12 | Designing AI Machines to Solve Business Problems | July 23, 2026 |
| Module 13 | Introduction to Quantum Computing | July 30, 2026 |
| Module 14 | Introduction to AR/VR | August 6, 2026 |
| Module 15 | Broader Implications of XR (+ Capstone Component 3) | August 13, 2026 |
| Module 16 | Introduction to Radical Innovation | August 27, 2026 |
| Module 17 | Urgency and Spirit of Radical Innovation | September 3, 2026 |
| Module 18 | Leading in Innovation (+ Capstone Component 4) | September 10, 2026 |
| Cesim Simulation Parts I–II | Global Challenge Simulation | September 17 & 24, 2026 |
| Module 19 | Technological Change and Organizational Strategy | October 1, 2026 |
| Module 20 | Building High-Performance Teams | October 8, 2026 |
| Cesim Simulation Parts III–IV | Global Challenge Simulation | October 15 & 22, 2026 |
| Module 21 | New Perspective of Leadership in Technical Teams | October 29, 2026 |
| Module 22 | Navigating and Leveraging Networks | November 5, 2026 |
| Module 23 | Culture, Conclusion, and Capstone | November 26, 2026 |
| Module 23 (cont.) | Simulation Exercise | December 17, 2026 |

Course end date: December 17, 2026. Program access through December 17, 2027.

## How to resume the crawl

1. Open the course in a logged-in Chrome tab and confirm the Claude-in-Chrome extension is connected.
2. List current modules to find newly published ones (compare against the table above):
   `GET https://classroom.emeritus.org/api/v1/courses/16614/modules?per_page=100`
3. For each new module ID, fetch items:
   `GET https://classroom.emeritus.org/api/v1/courses/16614/modules/{id}/items?per_page=100`
4. For each item, fetch the body: Pages via `/api/v1/courses/16614/pages/{page_url}`; assignments via `/assignments/{content_id}`; discussions via `/discussion_topics/{content_id}`; quizzes via `/quizzes/{content_id}`.
5. Video transcripts: open each module's "Summary and Video Transcripts" page, find the `files/{id}` link for "Video Transcripts", and extract text from the PDF (`/api/v1/courses/16614/files/{id}` → `url` → fetch PDF → pdf.js text extraction).
6. Write new module files as `07-Module-6-...md`, `08-Module-7-...md`, etc., and transcripts as `Transcripts/Module-6-Video-Transcripts.md`, etc.
7. Update the Status table and move completed modules from "RESUME HERE" into "Captured so far" above.

## Method notes (for whoever resumes)

- Crawl was done via the Canvas REST API using the logged-in browser session — much faster than page-by-page navigation.
- Large text was routed through the page DOM (rendered into an `<article>`, read back with the browser text extractor) because the JS console truncates output and the privacy filter blocks long token-like strings. URLs and long base64 tokens were stripped before retrieval.
- Quiz question text is not exposed by the platform; only quiz type, points, and instructions are available.
- Summary Deck PDFs (graphical slide decks) were intentionally not converted.
