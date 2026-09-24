---
name: review-checklist
description: Review a brief, one-pager, memo or spec against the PM's five checks before it goes any further (named owner, created date and time, how we'll know it worked, scope at the end matches the start, problem explained before the fix). Use when the user points /review-checklist at a document, or asks to review or check a brief before sending or sharing it.
argument-hint: <path to the document>
---

# Review checklist

Review one document against the five checks below, and report pass or fail for each with evidence. This is a read-only review: don't edit the document unless the user asks after seeing the results.

## 1. Get the document

- The path is in `$ARGUMENTS`. If it's empty, ask which document to review. Don't guess.
- Read the whole document before judging anything.
  - `.md` and `.txt`: read directly.
  - `.pdf`: use the Read tool.
  - `.docx`: extract the text from `word/document.xml` (it's a zip file) and review that.
- If the document links to other files (a mock, a data file), note them, but review only the document itself.

## 2. Run the five checks

Judge each check strictly on what's written. Don't give credit for things that are implied, or that you know from elsewhere in the project.

**1. Owner name**
- PASS: a named person is given as owner or author, e.g. `Owner: Priya Raghunathan` or `From: Wen Li`.
- FAIL: only a role or team (`PM, Dispatch`, `Product, Dispatch`), a placeholder (`[PM]`, `TBD`), or no owner at all.

**2. Date and time created**
- PASS: both the date and the time the document was created.
- FAIL: date only (say "date present, time missing"), no date, or only a "last updated" / "revised" date with no creation date.

**3. How we'll know it worked**
- PASS: the document says what signal will show it worked, such as a metric, a threshold, or an observable change.
- If it's there but weak (no baseline, no timeframe, or vague, like "fewer tickets"), still PASS it, but name the weakness in the fix column.
- FAIL: no success measure anywhere.

**4. Scope at the end matches scope at the start**
- Compare what the opening (problem, proposal, summary) says the work covers with what the end (scope, non-goals, decisions, next steps) says.
- PASS: they describe the same work.
- FAIL: the end adds something the start never promised, drops something the start promised, or contradicts it. Also FAIL if either end states no scope. List every mismatch specifically.

**5. Problem before fix**
- PASS: the problem is explained before the first proposed solution appears. A title that names the solution is fine.
- FAIL: the document opens with the solution, or never states the problem.

## 3. Report

Reply in this format:

**Review: `<file name>`: <N> of 5 passed**

| # | Check | Result | Evidence | What to fix |
|---|---|---|---|---|
| 1 | Owner name | ✅ Pass / ❌ Fail | quote the relevant line and its line number, or "not found" | concrete fix, or "—" |

(one row for each of the five checks)

Then give a one-line verdict:
- If all five pass: **Ready to go.**
- Otherwise: **Not ready. Fix <the failed checks> first.**

Rules for the report:
- Always quote the document's own words as evidence. Never paraphrase them as proof.
- Never invent a missing owner name, date or time, or a success metric. Tell the user what to fill in.
- Keep "What to fix" to one concrete action per row.
- After the report, offer to make the fixes. Don't make them unasked.
