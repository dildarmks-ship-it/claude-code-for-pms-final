# The 4.2 routing spiral — what's happening and why

To: Wen Li, Marcus Oyelaran · cc: Helen Achebe
From: [PM, Rook Dispatch]
Date: 10 September 2026

**Ask:** 30 minutes with Wen to walk through `history.py`'s scoring
logic and agree a fix. This isn't a console complaint or a seasonal
dip — it's a mechanical flaw in routing that's actively cutting a
growing group of responders off from work.

---

## The headline number is hiding the real problem

Aggregate callout acceptance rate looks like it's recovering:

| Week | Acceptance rate | Spread (busiest − quietest responder, pings/week) |
|---|---|---|
| Jun–early Aug (pre-4.2) | steady 75–78% | steady 8–10 |
| 10 Aug (4.2 ships 12 Aug) | 54% | 9 |
| 17 Aug | 66% | 15 |
| 24 Aug | 67% | 19 |
| 31 Aug | 73% | **21** |

The rate climbing back toward baseline isn't recovery — it's the
sample narrowing. By 31 Aug, four responders (Farlight, Meteor Mite,
The Undertow, Vesper) are down to 0–1 callout offers for the week,
while five others (Nightwell, Captain Vantage, Stormwrack, The Gale,
Sgt. Falkirk) are up at 17–21. The "it's mostly seasonal, it'll come
back in September" read doesn't hold against this — the gap is
widening every single week, in one direction, for the same nine
people. (Source: `00-rook/data/callout-history.csv`)

## The mechanism, in the code

`00-rook/code/dispatch-routing/history.py`:

- A decline and a timeout are scored identically — `record_declined()`
  handles both, same penalty. Missing an offer because the timeout is
  now 60s (was 90s) costs a responder exactly as much as refusing one.
- The credit/penalty is asymmetric: `ACCEPTANCE_CREDIT = 0.08` vs.
  `DECLINE_PENALTY = 0.12`. Working the equilibrium out, a responder
  needs to accept **above 60%** of offers just to hold their score
  steady — below that, it drifts to the floor regardless of effort.
- There's no decay or recovery toward neutral. A `TODO(wen, 2019)`
  already asks this exact question — "should this ease back toward
  NEUTRAL_SCORE on its own after a while?" — and the answer on file is
  "leaving it as-is for now." Once a score is floored, the only way
  back up is *more accepted offers* — which a floored score is
  precisely what prevents.
- 4.2 cut `WEIGHT_RECENT_ACCEPTANCE` (0.40 → 0.25), presumably to
  soften acceptance history's influence — but once a score hits the
  floor, its weight stops mattering. `WEIGHT_PROXIMITY` (raised to
  0.60) does all the deciding instead. The mitigation doesn't reach
  anyone who's already floored.

Open question, still unanswered on record: Marcus asked in
`#dispatch-team` on 14 Aug whether the routing change was meant to
treat habitual decliners differently from occasional timeouts — "the
config doesn't distinguish between them as far as I can tell." It
doesn't. Worth settling directly with Wen.

## It's showing up everywhere, not just in the numbers

This same pattern surfaced, unprompted, in Sofia's console-redesign
interviews — none of which were about routing:

- **Kip**, handling two responders on one screen: *"I'm looking at two
  cards on the same screen that might as well be two different
  products"* — Meteor Mite dead quiet, The Gale nonstop, same week,
  same city.
- **Ambrose**, on a near-miss for Captain Vantage: *"there was a period
  where a slower-arriving response of his still landed him the job
  more often than not, and lately that doesn't seem to hold the way it
  used to."*
- **Aunt Dot**: *"It didn't used to feel like a fair race, phone to
  stairs, and now it does, and he loses it more than he wins it."*

Three of four handlers interviewed about console UX raised this
unasked. Combined with the support ticket volume (roughly a third of
all tickets filed since 4.2 are "gone before I could answer," another
cluster are "nothing for weeks") and the Slack thread, this is now
corroborated across four independent channels: the data, tickets,
Slack, and design research.

## What I'd like to leave this conversation with

1. Confirmation from Wen on whether decline vs. timeout being scored
   identically was intentional.
2. A decision on adding decay/recovery to the recent-acceptance score
   — the 2019 TODO — given it's now the mechanism behind a live
   coverage risk, not a hypothetical.
3. A read on whether a partial timeout rollback (e.g. 75s) is worth
   testing independently of the proximity-weight question, since it's
   the more mechanical, easier-to-isolate lever.
