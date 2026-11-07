# Back in Rotation

*Rook Dispatch · Brief for Helen · Draft*
From: Dispatch PM · 7 Nov 2026 · Proposal, nothing built

Since 4.2, four of our 16 responders have been almost entirely dropped from rotation, and nobody can see it happening, including the responders. This brief proposes what they and their handlers should experience instead, using Meteor Mite and Kip as the example.

Clickable version: https://claude.ai/artifact/73RjV71Ta1qGwPA3BzjjtL

---

## What I need from you

1. **Agreed on not shipping the quick fix.** There's also a second reason: we don't yet know which scoring numbers are running in production, so changing one could miss. Wen can confirm this week.
2. **Make the policy call:** a missed ping should not count as a "no," and a bad stretch should fade over time unless the responder tells us they're away. Nobody has ever decided this. It has been an open question in the code since 2019.
3. **Find it a release slot.** Phase 1 is small. Phase 2 needs engineering and design time and competes with Q4 work. It also overlaps Availability Confidence, one of the Q3 items squeezed out of 4.2 that is now overdue.

Meanwhile, at no cost: I'll contact the four affected handlers directly. None of them have filed tickets.

---

## What happened: a reliable responder went silent in three weeks

Before 4.2, Meteor Mite was asked about 11 times a week, said yes about 70% of the time, and hadn't missed a ping since June. 4.2 cut the time to answer from 90 to 60 seconds. Mite missed 4 of 10 pings that week. The system counts a miss as a "no," and scores never recover on their own, so Mite moved to the back of the line and stopped being asked. By 31 Aug, Mite was getting 1 ping a week. The Gale, in the same area, was getting 21.

Kip, their handler (4 Sep): Mite keeps texting to ask if something is broken, while The Gale is exhausted. Kip has nothing to tell either of them. Farlight, The Undertow and Vesper followed the same path.

**Why it matters beyond these four:** we bill per active responder, so a responder who stops getting work is likely to become one we stop billing for. Callouts nobody took have also roughly doubled since 4.2.

**How sure we are:** the pattern in the data is strong. All four sank right after their release-week misses, and neither a seasonal dip nor past turn-downs explain who sank. The exact mechanism is unconfirmed until Wen answers, because scores rebuilt from the code don't fully match who sank.

**How current this is:** our data stops on 6 Sep. We don't know whether the four are still this quiet two months later. I'll ask Ravi for October numbers by Fri 13 Nov.

*Source: pings table, 29 Jun to 6 Sep; Kip interview, 4 Sep. The billing risk is my inference and has not been measured.*

---

## The gap the changes would close: about 120 pings never reached the four in the month after 4.2

Weekly pings to Meteor Mite, Farlight, The Undertow and Vesper combined. "Actual" is what happened. "Modeled" is what we'd expect if the changes had been live on 12 Aug. (The clickable version shows this as a line chart.)

| Week of | All pings | Actual to the four | Modeled to the four |
|---|---:|---:|---:|
| 29 Jun | 172 | 49 | – |
| 6 Jul | 170 | 48 | – |
| 13 Jul | 174 | 48 | – |
| 20 Jul | 176 | 50 | – |
| 27 Jul | 170 | 50 | – |
| 3 Aug | 172 | 49 | – |
| 10 Aug (4.2 ships, 12 Aug) | 177 | 43 | 50 |
| 17 Aug | 158 | 16 | 45 |
| 24 Aug | 162 | 6 | 46 |
| 31 Aug | 165 | 3 | 47 |

*How the modeled line works: once misses stop moving the four down the line, they keep their pre-4.2 share of all pings (28%). Weekly totals are real, so the citywide drop in callouts is already included. This is an estimate, not a replay of the routing code. It's likely a bit high, because some responders would answer a check-in with "away." Ravi should check it.*

---

## What we would build: four changes, each one something a person notices

**The trade-off:** each missed ping costs about 60 seconds before the callout moves on. In our data, the next responder takes it about 61 seconds after the miss. If misses stop moving people down the line, someone who keeps missing stays first and slows down real incidents. The check-in is what prevents that, so it's part of the fix, not an extra.

| Change | Who notices | What they experience | Phase |
|---|---|---|---|
| **A miss is not a no** | Responder | When a ping runs out, Mite's phone says the callout went to someone else, and that a miss doesn't count against Mite the way turning one down does. | Phase 1 · scoring rule + one message |
| **A bad stretch fades** | Both | A low score drifts back toward normal over time, so a bad week doesn't follow a responder forever. | Phase 1 · scoring rule |
| **A check-in instead of silence** | Responder | After a run of misses, Mite's phone asks "Still on tonight?" Yes puts Mite back in line straight away. No answer marks Mite as away, which Kip can see. | Phase 2 · also covers Availability Confidence, which was due in 4.2 and didn't ship |
| **The card says why** | Handler | Kip's console flags when a responder is being asked much less than usual, gives the reason, and shows who is covering for them. | Phase 2 · needs scores to be saved, which they aren't today |

---

## Click through it: Kip's console and Mite's phone, before and after

*Example screens, not built. "Today" uses real numbers; later steps are illustrative. The clickable version lets you play each step.*

1. **Today (4.2), Mon 31 Aug.** Kip's console shows Mite with 1 ping this week against a usual 11, and The Gale with 21. Nothing on screen explains it, so Kip tells Mite to hang in there. Mite's phone is just quiet: "No callouts. Last ping: 4 days ago."
2. **A ping runs out, Wed 12 Aug.** A callout arrives (warehouse fire, Eastgate, 60 seconds to answer) and Mite doesn't get to it in time. Mite's phone: "You missed a callout. It went to The Gale after 60 seconds. A miss doesn't count against you the way turning one down does. You're still first in line here." Kip's card: still first in line for Eastgate callouts.
3. **Four quiet days, Sun 16 Aug.** Mite has missed a few more and hasn't taken anything in four days. Kip's card flags it: asked 2 times since Wed, usually about 6; why: 4 pings missed in a row; Eastgate callouts are going to The Gale first; check-in sent to Mite. Mite's phone: "Still on tonight? If you're available, we'll put you back first in line for Eastgate. No answer by 23:00 sets you as away until you say otherwise."
4. **Mite answers, Sun 16 Aug.** Mite taps "I'm on tonight." That answer is what restores standing, not a timer or a support ticket. Kip's card: Mite checked in, next Eastgate callout goes to Mite first.
5. **Two weeks later, Mon 31 Aug.** Same week as "Today," with the change. Mite is back near a normal week (10 pings). The Gale's load has eased (14). Kip's two cards tell the same story again.

---

## How we'd know it worked

- The four are back to about 10 to 12 pings a week within 4 weeks of launch.
- Callouts nobody took fall back to pre-4.2 levels.
- Guardrail: The Gale and the other covering responders return to normal load, and overall acceptance (73% now) holds.

---

## Before we build: four answers we need first

| Owner | Question | By |
|---|---|---|
| Wen | Which scoring numbers are actually running? A score rebuilt from the code doesn't match who sank. | Fri 13 Nov |
| Wen | Why do most unanswered callouts stop after one ping, when the code is supposed to keep asking down the list? | Fri 13 Nov |
| Ravi | Are the four still this quiet? Our data stops on 6 Sep; we need October. | Fri 13 Nov |
| Marcus | Rough size of Phase 1 and Phase 2. Saving scores is new work. | Wed 18 Nov |

If Wen can't make time this week, I'll need your help freeing it up. She's the only person who knows how routing works.

**I'll come back with a recommendation and a size estimate by Fri 20 Nov.**

*Also lined up: value tuning with Ravi, testing the phone messages with responders through Sofia, and Supply and Security reviews of the new "away" status. Not in scope: the 60-second wait itself (a separate decision) and cross-area cover (Q4).*
