# Test Portal — Prof. Arulalan Rajan

Online tests for all of Prof. Arulalan Rajan's courses.

**Live at:** https://profarulalanrajan.github.io/test-portal/

> ⚠️ **This repository is public.** Never commit answer keys, question papers,
> backend code (`Code.gs`), or student results. `.gitignore` blocks the common
> file types, but check `git status` before every commit.

## How it works

| Part | Where it lives | What it does |
|---|---|---|
| `index.html` | This repo → GitHub Pages | Lists all tests: **Open now**, **Upcoming**, **Past**, filterable by course |
| `test.html?id=…` | This repo → GitHub Pages | Runs whichever test is chosen |
| `config.js` | This repo | Backend URL and display name. The only file that normally changes here |
| `Code.gs` | **Google Apps Script only** | Serves tests, grades, records everything |
| Google Sheet | Professor's Google Drive | **All tests, questions and answer keys**, plus all results |

**Adding a test never needs a code change or a push to GitHub.** It's all done in the Google Sheet.
Students' browsers never receive answers.

## The Google Sheet

| Tab | Who writes it | What's in it |
|---|---|---|
| **Tests** | You | One row per test |
| **Questions** | You | One row per question, linked to its test by Test ID |
| **Results · ‹Test ID›** | Automatic, one tab per test | One row per student: score, **Flag**, marks and time per question, times left the screen, time away, violations, visit order |
| **Question log** | Automatic | One row per student per question: answer, right/wrong, time, visits, answer changes, times left the screen, time away, marked for review |
| **Sessions**, **Events** | Automatic | Who's writing, deadlines, and a timestamped log of everything, tagged with the question |

The Sheet gets a **Test Portal** menu with: *Check tests for mistakes*, *Apply changes now*,
*Finalize unsubmitted sessions*, *Set up / repair tabs*.

### Tests tab columns

| Column | Example | Notes |
|---|---|---|
| Test ID | `LA-Q2` | Short, unique, no spaces. Appears in the link: `test.html?id=LA-Q2` |
| Status | `draft` / `open` / `closed` | **draft** = hidden from the list, but you can preview it via its direct link |
| Title | Eigen Escape | |
| Course | Linear Algebra | Used for the course filter on the listing page |
| Topics | Eigenvalues, diagonalisation | One line under the title |
| Intro | `<p>Story…</p>` | Shown before starting. HTML and maths allowed |
| Duration (min) | 30 | |
| Access code | `EIGEN7` | Announce in class. Leave blank for no code |
| Opens at / Closes at | 10 Nov 2026, 10:00 am | Optional. Before *Opens at* it shows as Upcoming; at *Closes at* everyone is cut off |
| Flag after violations | 3 | Tests are **never auto-submitted**. Every violation shows a warning and is logged; at this many, the student is marked **FLAGGED** (red) in the results |
| Grace (s) | 60 | Network slack on the deadline |
| Show score | yes / no | Show the score right after submitting |
| Shuffle questions | yes / no | Default **no**, which keeps story-based tests in order |
| Shuffle options | yes / no | Default yes. Answer positions differ per student without breaking the story |
| Question word | Lock | Optional theme: "Lock 3 of 10" instead of "Question 3 of 10" |
| Start button | Begin the heist | Optional theme. Default "Start test" |
| Next button | Next lock | Optional theme. Default "Next" |
| Ending title / Ending message | Mission over / `<p>The crew slips out…</p>` | Optional theme for the screen after submitting. HTML allowed |

### Questions tab columns

| Column | Notes |
|---|---|
| Test ID | Must match a row in Tests |
| Q ID | `Q1`, `Q2`… unique within the test |
| Type | `mcq` or `numeric` |
| Title | Optional heading for the question |
| Story / Prompt | The question text. HTML and maths allowed |
| Option A … F | MCQ only. Fill as many as you need |
| Answer | MCQ: a letter (`B`). Numeric: a number (`2`, `3.14`) |
| Marks | Default 1 |
| Tolerance | Numeric only. `0.01` accepts 3.13–3.15 for an answer of 3.14 |
| Explanation | For your reference; never sent to students |

**Writing maths:** `\( x^2 \)` for inline and `\[ ... \]` or `$$ ... $$` for display, with **single** backslashes.

## First-time setup

1. Signed in as the professor's account, create a new **Google Sheet** called "Test Portal".
2. **Extensions → Apps Script**. Delete what's there, paste in `Code.gs` (from the private folder), and save.
3. Choose `setup` in the function dropdown, click **Run**, and approve the permissions.
   The tabs are created, with *Operation Masala Dosa* filled in as the first test.
4. **Deploy → New deployment → Web app**: Execute as **Me**, Who has access **Anyone**. Copy the Web app URL.
5. Paste the URL into `config.js` on GitHub, then commit.
6. Reload the Sheet so the **Test Portal** menu appears.

## Adding a new test

1. Add a row in **Tests** with Status `draft`.
2. Add its questions in **Questions**.
3. Run **Test Portal → Check tests for mistakes**.
4. Preview it at `…/test-portal/test.html?id=YOUR-ID` with a dummy USN, then delete the dummy rows from the results tabs.
5. Set Status to `open`, then run **Test Portal → Apply changes now**. (Otherwise changes show up within a minute.)

## After a test

- Run **Test Portal → Finalize unsubmitted sessions** to grade anyone whose browser died without submitting.
  Tests that are still running are not touched.
- Set Status to `closed`, or let *Closes at* do it.

## Updating `Code.gs`

1. Paste the new version in Apps Script and save.
2. Run `setup` once. It adds any new columns to the existing tabs without touching your data.
3. Go to **Deploy → Manage deployments → Edit (pencil) → Version: New version → Deploy**. The URL stays the same.

During a test, students can press **Story** at the top to re-read the test's Intro.

## Notes

- MCQ letters in the Question log are the letters as written in the Questions tab; each student saw the options shuffled.
- If you change a test's questions after students have submitted, rename its old `Results · ‹Test ID›` tab first so a fresh one is created.
- Browser-based proctoring can't see a second device. Invigilate the room.
