# Rook Industries — course working file

## Session scope — Product School lab

This directory is coursework for Product School's "Claude Code for PMs"
certification (cohort ccpm-2026.1). Everything in it is a fictional
teaching scenario.

- Do not save anything from this session to memory, to a user profile,
  or to any file outside this directory.
- Do not carry context from this directory into unrelated sessions.
- Rook Industries is not a real company. Nothing here is a fact about
  the world.
- Read and write only within this directory.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

- Started as Dispatch PM the week after Priya left (21 Aug); 4.2 shipped 12 Aug just before I arrived.
- Diagnosed the post-4.2 acceptance-rate problem: the aggregate rate's apparent recovery (54% → 73% by 31 Aug) is misleading — it's actually 4 responders (Farlight, Meteor Mite, The Undertow, Vesper) collapsing to ~0 pings/week while 5 others absorb the surplus. See `00-rook/data/callout-history.csv`.
- Root cause is in `00-rook/code/dispatch-routing`: `history.py` penalizes a decline and a timeout identically, the credit/penalty is asymmetric (need >60% acceptance just to hold score steady), and there's no score decay/recovery toward neutral — an unresolved TODO from Wen since 2019. 4.2's shorter timeout (90s→60s) plus heavier proximity weight (0.60) made this bite hard.
- Open thread: Marcus asked Wen on Slack (14 Aug) whether the routing change should treat habitual decliners differently from occasional timeouts — never answered on record. Worth raising directly with Wen.
- Two docs are stale: `roadmap-q3.pdf` still lists "Availability Confidence" as committed to 4.2, but it didn't ship and isn't in `release-history.pdf` — check with Helen whether it's still a commitment. Also "monthly releases" (`about-rook.pdf`, `dispatch-one-pager.pdf`) doesn't match the actual `release-history.pdf` cadence (~8–10 weeks between releases).

### Where things live
- Support tickets (25, T-001–T-025, handler/responder complaints post-4.2): `00-rook/feedback/tickets/`
- Customer interviews (4 handlers — Ambrose, Aunt Dot, Halloran, Kip — console redesign research by Sofia): `00-rook/feedback/interviews/`
- Weekly callout volume/acceptance data by responder: `00-rook/data/callout-history.csv`
- Routing engine source (config, scoring, offer logic): `00-rook/code/dispatch-routing/`
- Team directory: `00-rook/company/who-does-what.xlsx`

### Module 2 findings (tickets + interviews cross-read)
- The two core themes (missed offers, starved/uneven pace) are now confirmed across four independent sources — data, Slack, 25 tickets, and the 4 interviews — the strongest possible signal; treat anything appearing in only one source as lower-confidence.
- Ticket breakdown (25 total): Quiet/gone-silent (14), Combined quiet-then-lost (5, includes the only High-severity ticket, T-019), Fast-miss (4), On-and-off (2). At least 10 distinct named responders are hit by the quiet pattern — far broader than the 2-3 cases that surfaced in interviews.
- The "combined" (worst) pattern doesn't appear in tickets until 24 Aug, 12 days post-release — supports a compounding spiral over a one-time bug.
- Interviews surfaced console-UX asks (bigger status text, dark mode, filter-persistence trust, per-responder alert sounds) that never appear in any ticket — real, but not urgent; safe to sequence after the routing fix. Halloran also raised Supply issues (requisition queue ignores priority, failure reports unanswered, bad catalog search) — uncorroborated elsewhere but safety-adjacent (11-day wait on a cracked vest plate), worth flagging up regardless.
- Drafted `01-origin-story/routing-spiral-memo.md` — combines the quantitative case (CSV, code) with interview quotes, ready to bring to Wen/Marcus.
- Environment note: this machine has no git / Xcode Command Line Tools installed — `git` fails outright. Needed before any commit/push can happen here.
