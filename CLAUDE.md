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
- Environment note (updated): git now works on this machine and `origin` is set to `github.com/dildarmks-ship-it/claude-code-for-pms-final` — commit/push are fine here now.

### Module 3 & 4 findings (numbers, code, and a correction to the Module 2 read)
- Full distribution, all 16 responders, weekly pings before (7-wk avg) vs. after (3-wk avg) 12 Aug: 4 collapsed 79–89% (Farlight, The Undertow, Vesper, Meteor Mite), 2 dipped mildly 24–27% (Corporal Ashgrove, Halfmoon), 10 gained 17–46%. It's a sharp split, not a general slowdown.
- Ruled out "it's just August being quiet" (the previous PM's read) for this specific pattern: the 6 pre-release weeks are flat at 75–78% acceptance (peaking the week before release), then cliff exactly at the release week, concentrated in 4 people who don't recover — not what a seasonal lull looks like. Caveat: this file only covers 10 weeks in one year, so it can't confirm/deny whether August is quiet *in general* at Rook, only that this pattern isn't that.
- **Correction to the Module 2 ticket read:** cross-checking all 25 tickets against the weekly CSV shows only 2 of 12 named responders (The Undertow, Farlight) have a ticket that matches a real, sustained drop in their numbers. The other 8 (Falkirk, Nightwell, Longcast, Ironvale, Stormwrack, Cindermark, Drift, Vantage) filed "quiet for weeks" or fast-miss tickets, but their weekly `pings_sent` is flat-to-rising over the same period — unresolved discrepancy, no day-level data to explain it. Don't cite "10+ named responders hit by the quiet pattern" without this caveat. Two tickets (T-001, T-019, etc.) describe a separate, real, code-confirmed "fast-miss" phenomenon (offer window now 60s, not 90s) that hits people regardless of whether their overall volume is declining — distinct from the 4-person collapse.
- Vesper and Meteor Mite (2 of the 4 truly collapsed) have **no ticket on file at all** — the only record of their situation is their handlers (Dorothy "Aunt Dot" Pell, Kip) raising it unprompted in Sofia's console-redesign interviews. Support (Nadia) may not have this on their radar.
- Escalation contacts (`00-rook/company/who-does-what.xlsx`, updated 2 Sep): **Wen Li** (Staff Eng, Berlin — owns the scoring code, back from leave after 24 Aug), **Marcus Oyelaran** (Eng Manager — approval gate per `config.py`'s comment), **Helen Achebe** (Director of Product — roadmap/commitments), **Nadia Hoffmann** (Support Lead — owns tickets, should be looped in given the Vesper/Meteor Mite gap above).
- Per-responder handler contacts: Farlight → Linda Pruitt; The Undertow → Desmond Okafor; Vesper → Aunt Dot (Dorothy Pell); Meteor Mite → Kip.
- Confidence read: who-was-affected and the ticket/data mismatch are **high confidence** (row-level counts, independently corroborated by interviews). The `history.py` mechanism (asymmetric credit/penalty, no decay, shorter timeout) as *the cause* is **medium confidence** — code-confirmed and timing-consistent, but there's no logged score history to trace directly, so it's a strong hypothesis to bring to Wen, not a proven verdict.
- The Undertow (handler Desmond Okafor) is the clean worked example: steady 11–13 pings/wk and ~9–10 taken through 3 Aug, drops to 4 taken the release week itself, offers collapse to 4/wk by 17 Aug and 1/wk by 24–31 Aug — and the one offer he did get on 31 Aug is the High-severity ticket (T-019), lost almost instantly. Good one to walk Wen through directly.
- How to get someone re-started once they're stuck: (1) handlers *can already* manually override routing to hand-pick a responder — confirmed via `06-sidekicks/briefs/routing-override-audit-log.txt` — but it's a fragile stopgap: one accept only adds `ACCEPTANCE_CREDIT = 0.08`, easily wiped out by one more miss, and overrides aren't currently logged anywhere. (2) The real fix is whether the decay/recovery mechanism Wen builds eases scores back **on elapsed time**, not just on new accepted offers — only a time-based decay actually reaches people who've stopped being offered anything at all. Worth asking Wen to confirm that design choice specifically. (3) Also worth asking about a one-time manual reset of the four already-floored scores in parallel, rather than waiting on decay alone.
