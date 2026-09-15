# Learning Log — Claude Code for PMs (cohort ccpm-2026.1)

Tracks what I'm learning module by module, separate from `prompts.md` (which
is the raw prompt record) and `CLAUDE.md` (which is the working file on the
Rook scenario itself). Updated as each session happens.

## Progress

| # | Module | Superpower | Status | Date |
|---|---|---|---|---|
| 1 | Orientation & Context | Origin Story | ✅ Done | 8 Sep |
| 2 | Listening at Scale | Super-Hearing | ✅ Done | 10 Sep |
| 3 | Reading the Numbers | Rewind | ✅ Done | 15 Sep |
| 4 | Debugging Code | X-Ray Vision | ✅ Done | 15 Sep |
| 5 | Building Yourself Without Coding | Super-Speed | ☐ Not started | |
| 6 | Building Your Own Skills | Sidekicks | ☐ Not started | |

## Module 1 — Orientation & Context (Origin Story)

**Skill practiced:** giving Claude Code the context it needs up front by
building a `CLAUDE.md` from a company's own documents, rather than
re-explaining background every session.

**What I did:** had Claude read Rook's one-pagers, glossary, roadmap, and
team roster, and wrote the initial `CLAUDE.md` working file from them.

## Module 2 — Listening at Scale (Super-Hearing)

**Skill practiced:** cross-reading a large pile of unstructured feedback
(interviews + tickets) to find patterns a manual skim would miss, and
knowing when signal is real vs. anecdotal.

**What I did:** cross-read 25 support tickets and 4 handler interviews
against the callout-history data and the routing source code. Confirmed the
post-4.2 acceptance-rate problem was a scoring/timeout feedback loop, not
seasonal noise or a UX complaint — same conclusion showed up independently
in the data, the tickets, and the interviews, which is what made it a
strong finding rather than a hunch. Wrote it up as a memo
(`01-origin-story/routing-spiral-memo.md`) aimed at Wen/Marcus.

**Takeaway:** the exercise showed the difference between a claim backed by
one source (lower confidence) and one that shows up across independent
sources (data + Slack + tickets + interviews) — worth treating that
distinction as a general filter on feedback, not just for this scenario.

## Module 3 — Reading the Numbers (Rewind)

**Skill practiced:** pulling a defensible number out of a raw data file —
computing it myself from the rows rather than trusting a pre-aggregated
summary, and being able to show which rows produced it.

**What I did:** asked Claude to open `00-rook/data/callout-history.csv`,
compute weekly totals before/after the 12 Aug release, and cite the actual
rows behind each number. The aggregate acceptance rate looked like it
mostly recovered by late August (54%→73%), but breaking it out by responder
showed that was misleading: 4 responders (Vesper, Farlight, The Undertow,
Meteor Mite) were being routed almost no pings at all by the last week of
data, while the other 12 absorbed the volume — which is what pulled the
aggregate rate back up. Confirms the Module 2 finding with the actual
numbers behind it, row by row.

**Takeaway:** an aggregate rate can recover while the underlying population
gets worse — always worth breaking a metric out by the dimension that could
be hiding a redistribution (here, by responder) before trusting a "trend
looks fine" read. Also: asking for the specific rows, not just the
computed number, is what caught this — a summary alone wouldn't have shown
who dropped out.

## Module 4 — Debugging Code (X-Ray Vision)

**Skill practiced:** reading unfamiliar code to trace a mechanism end to
end — not just spot-checking one file, but following how several files
interact to produce a real-world symptom — and turning that into a
prioritized, concrete fix list rather than stopping at "here's the bug."

**What I did:** asked Claude to read through `history.py`, `config.py`,
`routing.py`, and `offer.py` to confirm (not just repeat from the Module 2
memo) what in the code causes the routing spiral, then asked what to do
about it and whether to act on it now. Confirmed the mechanism precisely:
a shorter offer timeout (90s→60s) generates more no-answer events, which
`history.py` scores identically to an explicit decline; the credit/penalty
math is asymmetric (need >60% acceptance just to hold steady) with no
decay back to neutral; and the weight rebalance meant to soften this
doesn't help anyone who's already floored, since it's multiplying against
zero. Got a prioritized fix list (stop equating timeout with decline; add
score decay/recovery; fix the asymmetry; cheap interim lever of a partial
timeout rollback) and a recommendation to send the existing memo now
rather than gather more evidence, given the trend is still actively
worsening.

**Takeaway:** "what's the bug" and "what should we do about it" are two
different asks, and it's worth pushing past the first one to get a
prioritized, ordered answer to the second — especially the distinction
between a structural fix (decay/recovery) and a cheap interim lever
(partial timeout rollback) that doesn't require the harder decision to be
made first. Also useful: asking "should we follow through" got a clear
recommendation with a reason (compounding, still worsening weekly) instead
of just a yes/no.

## Next up

Module 5 — Building Yourself Without Coding (Super-Speed): turning
`05-super-speed/director-request.txt` into a one-page brief and a
clickable prototype.
