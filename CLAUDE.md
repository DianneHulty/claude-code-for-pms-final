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
  Exceptions, for the course-setup and wrap-up skills only:
  - When the student asks you to check their setup, save their work or wrap up a session, that request is their yes. You may run the GitHub command-line program installed at ~/.ccpm/gh for those checks and saves, and look in that folder to find it.
  - For a repair, first tell the student in one plain sentence what you are about to do, and act only after they say yes. Repairs may: run that GitHub program (including setting this folder's own git sign-in setting and changing this repo's visibility back to Public); copy the student's own course files into this directory from another folder on their computer (copy only; never move, edit or delete the originals); and rename something outside this directory that blocks setup, by adding "-old" to its name (never delete it).
  Outside this directory you still never write, edit or delete anything else.

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---


## Working context

I'm the new PM for **Rook Dispatch** at Rook Industries. I took over from Priya Raghunathan, who left 21 Aug 2026 with no overlap. Sources: rook-wiki and `00-rook/company/notes/handoff-from-priya.docx`.

### Company and products
- Rook sells coordination software to independent masked **responders** and the **handlers** and **quartermasters** who support them. It doesn't employ responders. Billed per active responder.
- **Rook Dispatch** (mine): availability, proximity, who gets pinged for a callout, and whether they take it. Handlers use the desktop console; responders use their phones.
- **Rook Supply**: requisitions, maintenance, field failure reports. Used by handlers and quartermasters.
- Monthly 4.x release train. Roadmap items against a numbered release are locked; changes go through Product.
- **Hard constraint:** never store, infer or analyze responder cover/legal identities (Security Policy 4.1).

### People (wiki comments use roles, not names)
| Role in comments | Person | Notes |
|---|---|---|
| Director of Product | Helen Achebe (Site Aleph) | My director. Owns roadmap and commitments. |
| Engineering manager | Marcus Oyelaran (Site Aleph) | Runs Dispatch eng. Direct; first stop when unsure. |
| Staff engineer | Wen Li (Berlin) | Built routing. Only real source on how ranking works. |
| Product designer | Sofia Marino (Site Aleph) | Console + phone app. Ran the September interviews. |
| Data analyst | Ravi Menon (Singapore) | Owns the numbers; weekly acceptance report. |
| Support lead | Nadia Hoffmann (Berlin) | Owns tickets; hears handler pain first. |
| Priya | Priya Raghunathan | Former PM (14 months, solo). Gone. |

Customers in briefs/interviews: Halloran (handler for Sgt. Bulwark, heavy Supply user), Aunt Dot (Vesper), Mr. Ambrose (Captain Vantage), Kip (Meteor Mite, The Gale).

### Vocabulary
- **Callout**: request for a responder to attend an incident. **Ping**: one callout offered to one responder. Outcomes: **taken**, **turned down**, **missed** (ping wait ran out). Turned down and missed are counted separately; both pass the callout on.
- **Ping wait**: how long a ping stays on the phone. One global value per release.
- **Acceptance rate**: share of pings taken (headline metric). **Time-to-accept**: median seconds to take.
- **Coverage gap**: nobody available had the needed capability tags (nobody *could* go), not low acceptance (nobody *would*).
- **Routing priority**: score ranking who gets pinged, from proximity (travel time), availability, capability match and recent acceptance. Turn-downs and misses lower it.
- **Capability tags**: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation.
- **Responder Availability Record**: written by Dispatch, read by Supply. **Mutual aid**: cross-area cover; not supported, Q4 exploration.

### Releases
- **4.0** (7 Apr): console navigation, profile redesign, routing override audit log.
- **4.1** (16 Jun): travel-time proximity, bulk callout, push reliability.
- **4.2** (12 Aug): proximity weighted up vs. acceptance (0.45→0.60, acceptance 0.40→0.25 per unsigned CHANGELOG); **ping wait 90s→60s**; filters persist; three defect fixes.

### 4.2 page comments, mapped to people
- **Marcus** (13–14 Aug): release went clean. Asked if the routing change was *meant* to hit responders who turn jobs down, or just fell out of the config. **Wen** was away to 24 Aug and said she'd look. Still unanswered.
- **Nadia** (18–26 Aug): tickets ~3x normal; split ~2/3 "phone never goes off", 1/3 "gone before I could answer". She can explain the 1/3 (60s wait), not the 2/3. Asked for the acceptance trend; a handler emailed her directly ("never happens"). Marcus offered rough numbers, not Ravi's weekly ones.
- **Priya** (19 Aug): "August is always soft", wait for September. **Marcus** (26 Aug) repeats it as "the August dip."
- **Marcus** (28 Aug): give the new PM a week, then regroup. Nadia will bring the ticket breakdown. Wen back: "message me directly." **Sofia** only commented on filter persistence. **Ravi and Helen never appear.**

### Where things stand: 4.2 is the live problem
- Tickets ran 3–4x normal through 7 Sep. The 60s wait explains the 1/3.
- **The 2/3, from Module 1:** Farlight, The Undertow, Meteor Mite and Vesper fell from ~12 to ~3–4 pings/week; their home callouts go to other areas' responders (The Gale, Nightwell, Stormwrack gained). Likely cause in `00-rook/code/dispatch-routing/`: a miss costs −0.12 (same as a turn-down) vs +0.08 for a take, scores never decay, and the 60s wait raised misses ~4–5x. Sunk responders never get asked, so they can't recover. **Unconfirmed: ask Wen.** The DB has no routing scores.
- Aggregate acceptance recovered (54%→73%) but hides this; untaken callouts roughly doubled.
- The "August dip" is **unverified and untestable** in the DB (no data before 29 Jun). Priya also says don't revert 4.2: the proximity change was a three-quarter-old ask from wide-area responders.
- Priya's handoff says Marcus "can pull numbers"; Ravi owns the real ones. Go to Ravi.
- Source reliability: DB and code first, then tickets and interviews. Priya's handoff, Glossary, README (code contradicts them), briefs and roadmap are least reliable.

### Roadmap gaps (last reviewed 30 Jun, stale)
- Every item is still owned by "Priya" and needs reassigning.
- **Availability Confidence** (Committed, 4.2) didn't ship. Which squeezed-out items are still Q3 commitments is an overdue conversation with Helen.
- "Ping timeout tuning" (Committed, 4.2) shipped only as the blanket 60s cut.
- Requisition approval chains (Supply, 4.3): real ask is a second sign-off over $2,000; scope has crept (escalation, cross-site dashboard, handler tracking, replacing Halloran's spreadsheet). Phasing TBD.
- Handler phone app and Shared cover: Exploring, Q4. The app brief (8 Sep, Sofia) cites Aunt Dot, but she only asked to be notified too when a callout comes in.
- Briefs are stale: Bulk Callout (Jan) and Routing Override Audit Log (Mar) still read as unstarted but shipped in 4.1 and 4.0. Bulk Callout never got its time-to-full-coverage baseline.

### Priya's to-dos for me
- Write down how routing priority works (no doc exists; ask Wen).
- Filter-persistence tickets are cosmetic noise.
- Revisit Priya's fast calls in parts of the product nobody has looked at closely.

- Cross-source audit: the 60s ping wait plus miss-as-turn-down scoring explains ~35 of 107 post-4.2 tickets and all 4 interviews. Sunk four now miss 59% of pings (was ~3%). Correction: The Gale is Meteor Mite's same-area rival (Eastgate), not cross-area; Falkirk and Cindermark also took Farlight's Uptown callouts.
- Not the same cause: Halfmoon and Corporal Ashgrove "quiet" tickets (10) match ~30% fewer callouts in Westbury/Northfield. Meteor Mite and Vesper have zero tickets (seen only in interviews), so tickets undercount. All 45 issue tickets are still open.
- Halloran's interview asks for faster approvals (11-day wait, priority field ignored), not the brief's $2,000 second sign-off. Filter tickets aren't just cosmetic: silent resets and shared computers leave handlers watching the wrong list.
- Open: are routing scores persisted (code keeps them in memory)? Where does the 4-ping limit come from? Why did callouts drop ~15% on 12 Aug? No data after 7 Sep, and no release listed after 4.2. DB can't measure time-to-accept or coverage gaps.
- Module 2 weekly pings test: the sunk four's misses jumped first (2%→50% in the 12 Aug week) and their pings collapsed after (51→36→12→6→2 a week; taken 37→0). That order fits the scoring penalty, not a seasonal dip. Everyone else's miss rate rose to 10–18%.
- Data-only findings: most untaken callouts stop after one ping (40 of 49 since 4.2, ~8→13 a week); pings to other areas' responders rose 17%→39% despite the proximity upweight; callouts fell citywide (−5% to −32% by area, ~−16% overall), not just Westbury/Northfield.
- Tickets describe symptoms, not causes. Interviews and tickets agree only on lost callouts; ~75 tickets cover topics no interviewee raised, and some interview lines match ticket text almost word for word, so the two aren't fully independent.
- Support seems to close on reply, not fix: 38 ticket topics were closed, then filed again. Quiet but risky: idle sign-out plus 1-hour password resets, shared sign-ins, medical notes in a 250-character field, double billing, screen reader unusable, no leave status.
- Ruled down: duplicate pings aren't routing (none in the pings table); Monday 8–9am slowness isn't callout load; the phone-number save bug showed no miss spike for Stormwrack.
- Open: ask Wen why untaken callouts stop after one ping and which weights actually shipped (the CHANGELOG's lower acceptance weight should have softened the penalty); ask Ravi whether the citywide callout drop is real demand.
- Module 3: all 16 responders' first post-4.2 miss fell 12–15 Aug. Before 4.2, 0 of 186 out-of-area pings were taken; after, 36% were, so cross-area routing changed too. Time-to-accept isn't measurable (no response timestamp), but turn-downs always come within 40s, while takes can be slower (Captain Vantage was "suiting up").
- Handoff mechanism: a sunk responder misses as first ping and a neighbour takes the same callout about 61s later (Meteor Mite→The Gale, Farlight→Cindermark, Vesper→Captain Vantage, The Undertow→Stormwrack). Asked-first order on home callouts flipped within about 48h; Eastgate first pings went to The Gale 15 of 17 by 31 Aug.
- Release-week miss share (40–57% vs 6–20%) separates the sunk four better than the rebuilt score (+0.08/−0.12). Vesper sank at −0.20 while Nightwell (−0.32) and Stormwrack (−0.20) rose, which hints that live scoring weighs misses more than turn-downs. Ask Wen.
- Tickets vs pings: all 30 numeric ticket claims match the pings table exactly, but ticket times often share minutes and seconds with ping rows, so they may not be independent. Ask Nadia how tickets get created. Quiet tickets lag the ping drop by about a week.
- Filing gap: Aunt Dot (Vesper), Kip (Meteor Mite, The Gale), Halloran and Mr. Ambrose file almost nothing, and never did. The two worst-hit responders have zero tickets, while Halfmoon and Ashgrove (−20%, demand) have 10. Reach the silent handlers directly.
- Module 4 code read: 4.2 changed only three numbers in config.py (wait 90→60s, closeness 0.45→0.60, says-yes 0.40→0.25), no rules. Only a take (+0.08) adds points; any non-take costs −0.12 (offer.py treats miss = no); no fade, no reset; scores live only in memory. The code walks the whole list, yet the data stops after one ping, so something outside this folder cuts the search.
- Marcus's question: 4.2 applied to everyone alike. Old turn-downs didn't predict who sank (Vesper 15% sank, Halfmoon 28% didn't); release-week misses did (44–63% vs ≤40%). Whether it was intended isn't written down anywhere. Ask Wen (how), Priya via Marcus (intent).
- Theory tests: seasonal dip, old turn-downs, and instant demotion at deploy all fail; locals stayed first until 15 Aug, after the misses. Rebuilt code score picks only 3 of the 4 (Nightwell scores as low as Vesper but rose), so live weights may differ. "Phone never goes off" means truly no pings; tickets and Dot show pings did buzz, so delivery failure is weak.
- Bottleneck now is answering, not being asked: last 2 weeks the four got 9 pings (all first pings), missed 7, took 0. Before 4.2 everyone bounced back from 2–4 day quiet spells; after, only the four didn't. Recovery needs ~6 takes in a row (4–12 weeks).
- Replay of real histories: miss-free scoring keeps the four within 1–5 min of their replacements (vs 14–18 today); miss −0.04 gives 6–9 min; fade alone barely helps. A one-time reset to 0.5 only holds if the miss penalty also changes. Ticket wording from Farlight's and The Undertow's handlers is near-identical, so ask Nadia how tickets are made.
- Module 5: Helen (director-request.txt) asked for a one-pager plus something clickable, from the view of a handler like Kip and a quiet responder, not a quiet number change; she wants Wen's 2019 "should standing fade?" TODO in history.py answered. Example responder used: Meteor Mite (handler Kip, Eastgate); the four sunk got ~49 pings/week before 4.2, 3 by 31 Aug.
- Proposal ("Back in Rotation", 05-super-speed/brief.md + artifact): a miss is not a no; low standing fades back; a "Still on tonight?" check-in after 3 misses in a row (overlaps unshipped Availability Confidence); Kip's card says why. Phase 1 = scoring rule + message, Phase 2 = check-in + console "why" (needs scores saved). Trade-off: each miss costs ~60s, so the check-in is part of the fix.
- Brief's asks to Helen: agree to hold the quick fix (live weights unknown), make the miss/fade policy call, find a release slot. Blockers with placeholder dates: Wen (live weights; why search stops after one ping) 13 Nov, Ravi (October data, nothing after 6 Sep) 13 Nov, Marcus (sizing) 18 Nov, recommendation to Helen 20 Nov. Billing-risk line is an unmeasured inference.
- Modeled chart: keeping the four at their pre-4.2 28% share of real weekly pings gives ~45–50/week vs actual 43→3, ~120 pings lost in 4 weeks. An estimate, not a routing replay; Ravi should check it.
- prototype.html now holds: Kip's console (Needs-you list, place in line from closeness 0.60 + standing 0.25, tonight's ping dots, handler actions incl. accept/pass for Mite, rest breaks, per-responder alert tones, night mode), Mite's escalating full-screen alert, a Both-phones hand-off tab, and a Rook-team phone-trouble pattern view (seed reports RD-1033–1040 are made up). With assumed 4 vs 12 min travel, 4.2 rules drop Mite behind The Gale after exactly 4 misses, matching release week.
- Not yet in the brief: handler actions, phone-trouble reporting, the escalating alert. Open: handler accepting on a responder's behalf is a policy/Security call; breaking through silent mode needs iOS critical-alert / Android full-screen permissions.
