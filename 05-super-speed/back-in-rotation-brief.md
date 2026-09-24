# Back in Rotation — one-pager

To: Helen Achebe · **Owner:** Manoj Singh, PM, Dispatch · **Created:** 22 September 2026, 14:28 EDT · Draft for discussion

> **Click through it:** [`back-in-rotation-mock.html`](back-in-rotation-mock.html) (in the same folder as this brief). Open it in any browser. It walks through one callout for Kip and Meteor Mite, today and with this change, and ends with how all 16 responders are doing.

## The problem in one line

Once a responder goes quiet, the only thing that brings them back is being asked. Being quiet is exactly what stops them being asked. Today nobody sees this happening. The responder only notices the silence, and the handler can't tell it apart from bad luck.

Kip's two cards show it. Meteor Mite was getting 10–12 offers a week through early August, then 4, 2 and 1. The Gale, in the same city on the same screen, went from 13 to 21. Mite texts Kip asking if something's broken. Kip's answer is "hang in there, it'll pick up." That answer is wrong: under today's rules it won't pick up.

## Why not just ship the fix this afternoon

The quick fix makes scores recover over time. That's the right change, and it's one of the three pieces below. On its own, though, nobody would notice it. Kip would still be looking at a silent card with no reason given, Mite would still get silence, and we'd have changed a number and called it handled. What makes this "properly" is the other two pieces: a guaranteed ask for anyone who has gone quiet, and a missed offer counting less than a decline. All three show up where people can see them, as a reason on Kip's card and a message on Mite's phone.

**Recommendation:** ship all three pieces together, after Wen confirms the mechanism. The four responders who are stuck shouldn't wait on that. This week their handlers can hand-pick them for callouts using the override the console already has, and Nadia's team can contact the handlers directly. Neither needs a code change. Wen could also reset those four scores once by hand. That's a one-off data change, not a change to the scoring code, and it's decision 3 below.

## Who it's for

- **Primary: the handler (Kip, Aunt Dot, Linda Pruitt, Desmond Okafor).** Handlers are the people actually using the console, and they field the "is something broken?" texts.
- **Secondary: the responder who has gone quiet.** They are still marked available and still willing, but they've fallen to the bottom of the list and can't climb back up.

## What we'd build

Three pieces, shipped together. Each one shows up on Kip's console and Mite's phone (see the next section).

1. **Being quiet gets you asked.** A responder who is marked available but hasn't had an offer in 7 days gets the next matching callout within range first. This is a guaranteed ask, not a guaranteed job. If they take it, they're back. If they miss it, the callout moves on in seconds, just as it does today. *(Pre-4.2 every responder got 8+ offers a week, so a silent available week is abnormal, not unlucky.)*
2. **A bad stretch fades with time.** The acceptance score drifts back toward neutral as days pass, so it no longer takes new accepts to recover. That answers Wen's 2019 TODO with "yes, by time." Time is the only thing that reaches someone who is getting no offers at all.
3. **Missing isn't refusing.** An offer that times out costs less than an active decline. That answers Marcus's unanswered 14 Aug question.

## What changes for that person

| | Today | With this |
|---|---|---|
| **Kip, on the console** | Two cards, one silent, one on fire, and nothing explaining why. | Mite's card says *"Quiet: 3 offers in 2 weeks (usually ~11 a week). Missed offers lowered their place in line. Next matching callout goes to Mite first."* The gap now has a reason and a next step. |
| **Kip, to Mite** | "Hang in there." | "You're first in line for the next one. Keep your phone close." |
| **Meteor Mite, on mobile** | Silence. No way to tell whether the phone, the app or they themselves are the problem. | A status line: *"You're back in rotation. The next matching callout comes to you first."* After a miss: *"Missed. It's gone to someone else. A miss doesn't count against you the way a decline does."* |

## What it deliberately doesn't do

- **Doesn't undo 4.2 or change the 60-second timeout.** Wide-geography responders asked for the proximity change. Offers vanishing before people can answer is a separate problem that gets its own decision.
- **Doesn't equalise workload or set quotas.** The Gale stays busy. We guarantee Mite gets *asked*, not that the two share work evenly.
- **Doesn't override a choice to step back.** Re-entry only applies to responders marked available. Someone who keeps declining still carries that until they take work, which is Wen's "against" case, kept on purpose.
- **Doesn't add a setting or show scores.** Handlers get an explanation, not a dial. Nobody sees a score or rank they could game.
- **Doesn't touch the Responder Availability Record.** Supply schedules maintenance from it, so it stays byte-for-byte the same.

## How we'd know it worked (first 4 weeks)

- No available responder goes more than 7 days without an offer.
- Farlight, Meteor Mite, The Undertow and Vesper each get an offer in week 1.
- The gap between busiest and quietest responder falls from 21 offers a week back toward the pre-4.2 level of 8–10.
- Time-to-accept doesn't get worse. **We report the spread weekly next to the aggregate rate**, because the average is what hid this in the first place.

## Decisions I need

1. **Timing:** hold the quick fix and ship all three pieces together, using manual overrides as a bridge until then? I recommend yes.
2. **High-severity callouts:** should re-entry offers skip them? The Undertow's one offer on 31 Aug was the High-severity T-019, and it was lost almost instantly. I recommend skipping them.
3. **The four already at the floor:** a one-time data reset of their scores now, or let re-entry reach them once it ships? I recommend the reset now. It's Wen's call.
4. **Nadia:** Vesper and Meteor Mite have no tickets on file. Support should hear about them before handlers see the new card copy.

*Confidence: who is affected is high confidence (row-level data plus interviews). The scoring mechanism as the cause is medium confidence: it's confirmed in the code and fits the timing, but no score history is logged. Wen should confirm it before we build.*
