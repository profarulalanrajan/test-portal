# Test Portal — Prof. Arulalan Rajan

Online tests for Prof. Arulalan Rajan's courses.

**Live at:** https://profarulalanrajan.github.io/test-portal/

> ⚠️ **This repository is public.** Never commit answer keys, question papers,
> backend code (`Code.gs`), or student results. `.gitignore` blocks the common
> file types, but check `git status` before every commit.

## How it works

| Part | Where it lives | What it does |
|---|---|---|
| `index.html` | This repo → GitHub Pages | The page students open to take the test |
| `Code.gs` | **Google Apps Script only** (attached to a Google Sheet on the professor's account) | Holds questions + answer key, grades, stores results |
| Google Sheet | Professor's Google Drive | Results, per-question log, sessions, events |

Students' browsers never receive the answer key.

## What gets recorded

**Results** tab — one row per student:
score, mark per question, **time spent on each question**, total time,
**times left the screen**, **total time away**, violations, late / auto-submitted,
resumes after a crash, **order the questions were visited in**, and a violation log
showing which question the student was on each time.

**Question log** tab — one row per student per question:
position shown to that student, answer given, correct answer, right/wrong/unanswered,
**time on the question**, number of visits, **answer changes**,
**times left the screen while on it**, **time away while on it**, marked for review.

**Events** tab — timestamped log of every start, resume, violation, return
(with how long the student was away) and submit, tagged with the question.

## Setting up a test (first time)

1. Signed in as the professor's account, create a new **Google Sheet** (for example "Test Portal — Results").
2. In the Sheet: **Extensions → Apps Script**. Delete what's there and paste in `Code.gs` (from the private folder, *not* from this repo).
3. Pick `setup` in the function dropdown and click **Run**. Approve the permissions. Four tabs get created.
4. **Deploy → New deployment → Web app**
   - Execute as: **Me**
   - Who has access: **Anyone**
   - Copy the **Web app URL**.
5. In `index.html`, paste that URL into `BACKEND_URL` near the top of the `<script>`, then commit and push.
6. Take the test yourself once with a dummy USN, check the Sheet, then delete that row from every tab.

## Running a test

- Change `ACCESS_CODE` in Apps Script before each test, and announce it in class when students should start.
- After editing `Code.gs`: **Deploy → Manage deployments → Edit → Version: New version → Deploy**.
  (Keeps the same URL, so `index.html` doesn't need to change.)
- To stop new starts: set `QUIZ_OPEN: false` and redeploy. Students already writing can still submit.
- After the test: run `finalizeUnsubmitted` from the editor to grade anyone whose browser died without submitting.

## Notes

- MCQ letters in the Question log are the **paper** letters (A–D as printed in the question paper);
  each student saw the options in a shuffled order.
- Browser-based proctoring can't see a second device. Invigilate the room.
- Use a fresh Sheet when the column layout changes. Old tabs keep their old headers.
