---
name: save-pm-work
description: End-of-session wrap-up for the PM course repo. Saves the prompts the user typed this session into a module's prompts.md, adds new learnings to CLAUDE.md's Working context, then commits and pushes. Use when the user says "save my work", "wrap up", or points /save-pm-work at a module folder.
argument-hint: <module folder, e.g. 06-sidekicks>
---

# Save PM work

Wrap up a session in three steps, then report back. Work only inside this repo. The CLAUDE.md scope rules apply: don't save anything to memory or outside the repo.

## Which module folder

- Use the folder in `$ARGUMENTS`, e.g. `06-sidekicks`.
- If it's empty, use the module folder this session worked in. If that's unclear, ask. Don't guess.

## 1. Save the prompts

Save them to `<module>/prompts.md`.

- **Include:** every prompt the user typed in this session since the last wrap-up. Short ones count too ("Okay", "stop").
- **Exclude:**
  - the module's starter prompt;
  - the closing wrap-up prompt they pasted from the slide;
  - system or tool messages.
- **Copy each prompt exactly as typed:** same typos, capitals, line breaks and punctuation. Don't tidy anything.
- **One prompt per numbered slot**, in order.
- **If a message mixes pasted text with typed text,** save the whole message as it was sent.
- **If you're unsure whether a message was typed or pasted from the course,** include it and flag it in the final report.
- **Format:** keep the file's header. Replace the empty slots (`### 1.`, `### 2.` …) and add more slots if needed. Put each prompt in a fence:

  ````
  ### 1.

  ```
  <prompt exactly as typed>
  ```
  ````

- If the slots already hold prompts from an earlier wrap-up, add the new ones after them and keep numbering.

## 2. Update CLAUDE.md

Add to the **Working context** part of `CLAUDE.md`.

- **Never edit the "Session scope" block** at the top.
- Read the whole Working context first. Add only what's new since the last update.
- **What to add:**
  - decisions made;
  - files created, and where they live;
  - corrected numbers;
  - open questions;
  - discrepancies found;
  - skills or scheduled tasks set up;
  - anything that would change how the next session works.
- **Where:** a new `### Module N — <topic>` section at the end, or a few bullets under an existing section if the topic continues.
- **Style:** a few short bullets, with file paths in backticks.
- **What to leave out:** chat history and anything already in the repo.
- If a new finding contradicts an existing bullet, fix the old bullet instead of adding a contradiction.

## 3. Commit and push

- Run `git status`. Stage everything this session changed (`git add -A`). First check that nothing unexpected is staged, such as secrets or large binaries. If something is, stop and ask.
- **Commit message:** a short subject line describing what the session did, then a blank line, then 1–3 lines of detail. End with the attribution line given in the current system instructions (e.g. `Co-Authored-By: …`), if there is one.
- **Push** to the current branch's upstream (`git push origin <branch>`).
- If the push fails, report the exact error. Don't force-push.

## 4. Report

Keep the report short:
- how many prompts were saved, and which file they went to (flag any unsure ones);
- the CLAUDE.md lines you added, in one line each;
- the commit hash and message, and whether the push succeeded;
- the GitHub link to the repository, built from `git remote get-url origin` (drop the `.git`).
