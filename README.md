# BU Dashboard

Private dashboard for one person wearing two hats at Boston University: PI of the Ling Lab, and director of the BBC PhD program (Dept. of Psychological & Brain Sciences).

It has two halves, switched at the top of the page: **Lab** and **Director**. They are deliberately kept separable — see "Separating the two halves" below.

## How it is put together

- `index.html` is the whole app and contains **no student data**. It unlocks with a director key (open it once as `.../#KEY`; the key is then remembered on that device and stripped from the URL).
- Data lives in a private Google Sheet, "BBC Director Dashboard (data)", written by a Google Apps Script web app (`ENDPOINT` in `index.html`). That script holds the key and the roster, so it is deliberately **not** in this repo.
- Live annual-report status and the submitted reviews are read from the BBC Progress Reports sheet by the same script.

## Tabs

### Director

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
| **Tools** | Searchable student handbook, program reference, links, source data, handoff, settings |

### Lab

| Tab | What it holds |
| --- | --- |
| **Overview** | Overdue chores, what is due soon, IRB and grant dates, the cash box |
| **Lab** | Members and their roles, funding and projects |
| **Grants** | Awards, periods, next report due |
| **Compliance** | IRB protocols, status, expiry |
| **Admin** | Lab chores, plus the subject payment cash box |
| **Shortcuts** | Editable recipes for the recurring nonsense, each with steps, links and a prompt to hand to Claude |

## Hiding rows and columns

Every table carries a **Hide rows or columns** control above it. In hiding mode each row
gets a checkbox and each column header gets one; unticking either takes it out of view.
Clicking anywhere on a row toggles it, so you don't have to aim at the box.

A hide is a **view preference, never a delete**. Nothing leaves the store, and the bar above
the table always says how many rows or columns are hidden with a one-click "show them".
Hides are saved to the backend as a single `prefs` row per branch (`prefs-dir`, `prefs-lab`)
in the `history` collection, keyed by table and tagged with `branch`, so they follow you
across devices and split with the two halves like everything else.

Rows are keyed by the text of their first column, so a hide survives sorting and filtering
but not a rename of that first field — rename a student and their row reappears.

## Separating the two halves

The point is that the director half can be handed to the next program director without dragging the lab half along, and vice versa.

- **Every stored row carries a `branch` tag** — `"dir"` or `"lab"`. Nothing is shared between them; a check of the two exports finds zero overlapping ids.
- **Tools → Handoff** exports either half as a single JSON file, with per-collection counts shown before you download.
- **The lab code is one block** in `index.html`, between the banners `LAB BRANCH — SELF-CONTAINED` and `END LAB BRANCH`. Delete that block, drop the `lab` entry from `TABSETS` and the lab keys from `VIEWS`, and the director half runs untouched.
- **Each half keeps its own tab order** under its own localStorage key, so neither inherits the other's layout.

To hand the director side over: download the director half, fork the repo, have the new director stand up their own Apps Script with their own key and sheet, import the JSON collection by collection, and delete the lab block. Your copy keeps working and they never see your lab data.

## Storage

The Apps Script keeps one "Store" sheet with the columns Collection, Id, Updated, JSON.

- Live collections, loaded on every visit: `students`, `tasks`, `notes`, `config`
- Archive collections, loaded only when an archive view is opened: `history`, `records`, `alumni`, `funding`
- Lab rows live in `history` under `kind` of `lab`, `recipe` or `cash`, all tagged `branch: "lab"`

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
