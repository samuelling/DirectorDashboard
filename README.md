# DirectorDashboard

Private dashboard for the BBC PhD program director (Dept. of Psychological & Brain Sciences, Boston University): tasks, per-student milestones, notes, funding by year, the live annual-report cycle, and the program archive.

## How it is put together

- `index.html` is the whole app and contains **no student data**. It unlocks with a director key (open it once as `.../#KEY`; the key is then remembered on that device and stripped from the URL).
- Data lives in a private Google Sheet, "BBC Director Dashboard (data)", written by a Google Apps Script web app (`ENDPOINT` in `index.html`). That script holds the key and the roster, so it is deliberately **not** in this repo.
- Live annual-report status and the submitted reviews are read from the BBC Progress Reports sheet by the same script.

## Tabs

Ordered roughly by how often they get used.

| Tab | What it holds |
| --- | --- |
| **Overview** | Overdue and upcoming tasks, flagged students, recent notes, report-cycle counts |
| **Tasks** | The to-do list, by category and due date |
| **Students** | Current roster, milestone progress, per-student detail |
| **Notes** | Dated bookkeeping notes, taggable and linkable to a student |
| **Annual Report** | One subtab per cycle. The current year has submission status and results; past years hold the program report and the student progress reports |
| **Funding** | One subtab per year, newest first, plus an all-years comparison |
| **People** | Everyone the program has a record of, current and former, with their reports, funding and milestones in one place |
| **Program** | Learning outcomes by cycle, earlier cohorts, and program documents |
| **Settings** | Milestone template, JSON backup, sign out |

## Storage

The Apps Script keeps one "Store" sheet with the columns Collection, Id, Updated, JSON.

- Live collections, loaded on every visit: `students`, `tasks`, `notes`, `config`
- Archive collections, loaded only when an archive view is opened: `history`, `records`, `alumni`, `funding`

Backend actions: `load`, `put`, `del`, `bulk`, `report`, `reviews`, `archive`.

## Accessibility

Tabs and subtabs are real tablists with arrow/Home/End keys and roving tabindex. Every table sorts by clicking **or** keyboard-activating a column header. There is a skip link, a `<main>` landmark, a polite live region for save status, and dialogs are labelled and manage focus.

## Archive provenance

The archive was built from the director's own files, with university IDs, emails, addresses and phone numbers stripped:

- Program annual reports, cycles 2023 and 2025
- Student progress reports: 2011-12, 2022-23, 2023-24, 2024-25
- Milestones reports: June 2017 and April 2018
- Funding: PBS AY 24-25, Summer 2026 projections, FY 26-27 obligations, and a Jan 2013 BBC/GPN support snapshot
- Program documents: 2014 strategic plan, April 2014 meeting agenda

Alumni career outcomes gathered by web search carry a confidence flag; anything marked *unconfirmed* or *unverified* should be checked before it is used in a report.
