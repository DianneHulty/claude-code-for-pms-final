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

I'm the new PM for **Rook Dispatch** at Rook Industries (took over from Priya Raghunathan, who left 21 Aug 2026 with no overlap). Sources: rook-wiki (About Rook, Glossary, Team directory, Releases, Q3 roadmap, Product briefs) and `00-rook/company/notes/handoff-from-priya.docx`.

### The company and products
- Rook sells coordination software to independently operating masked **responders** and the **handlers** and **quartermasters** who support them. Rook doesn't employ responders. Billed per active responder.
- **Rook Dispatch** (mine): availability, proximity, who gets pinged for a callout, and whether they take it. Users: handlers on the desktop console; responders on their phones.
- **Rook Supply**: requisitions, maintenance, field failure reports. Users: handlers and quartermasters.
- Monthly release train, 4.x numbers. Items listed against a numbered release on the roadmap are "locked"; changes to them go through Product.
- **Hard constraint:** we never store or infer cover/legal identities (Security Policy 4.1). Never design or analyze anything that tries to work out who a responder is.

### People (Dispatch team; wiki comments often use role, not name)
| Role in comments | Person | Notes |
|---|---|---|
| Director of Product | Helen Achebe (Site Aleph) | My director. Owns roadmap and commitments. |
| Engineering manager | Marcus Oyelaran (Site Aleph) | Runs Dispatch engineering. Direct; first stop when unsure. |
| Staff engineer | Wen Li (Berlin) | Built the routing (who gets pinged). Only real source on how ranking works. |
| Product designer | Sofia Marino (Site Aleph) | Console + phone app. Ran the September interviews. |
| Data analyst | Ravi Menon (Singapore) | Owns the numbers; weekly acceptance-rate report. |
| Support lead | Nadia Hoffmann (Berlin) | Owns tickets; hears handler pain first. |
| Priya | Priya Raghunathan | Former Dispatch PM (14 months, solo). Gone. |

Named customers in briefs/interviews: Halloran (handler for Sgt. Bulwark, heavy Supply user), Aunt Dot (Vesper), Mr. Ambrose (Captain Vantage), Kip (Meteor Mite, The Gale).

### Vocabulary (Rook-specific meanings)
- **Callout**: a request for a responder to attend an incident; the unit of work.
- **Ping**: one callout offered to one responder's phone. Outcomes: **taken**, **turned down**, or **missed** (no answer before the ping wait ran out). Turned down and missed are counted separately, but both pass the callout to the next responder.
- **Ping wait**: how long a ping stays on the phone. One global value, set in the release.
- **Acceptance rate**: share of pings taken. The headline metric. **Time-to-accept**: median seconds to take a ping.
- **Coverage gap**: no available responder had the needed capability tags. Not the same as low acceptance: a gap means nobody *could* go, low acceptance means nobody *would*.
- **Routing priority**: the score used to rank who gets pinged. Inputs are proximity (travel-time estimate), availability, capability match, and recent acceptance history. **Turning down or missing a ping lowers future priority.**
- **Capability tag**: e.g. flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation.
- **Responder Availability Record**: written by Dispatch, read by Supply.
- **Mutual aid**: responders covering for each other across areas. Not supported; Q4 exploration.
- **Requisition / field failure report / service interval**: Supply terms.

### Releases
- **4.0** (7 Apr): console navigation, profile redesign, routing override audit log.
- **4.1** (16 Jun): proximity uses travel-time estimate, bulk callout, push reliability.
- **4.2** (12 Aug): proximity weighted up relative to acceptance history; **ping wait cut from 90s to 60s**; console filters persist; three defect fixes.

### Where things stand: 4.2 is the live problem
- Since 4.2, fewer pings are taken and callout tickets are running about 3x normal and holding (Nadia). Split: about **2/3 "my phone never goes off"**, about **1/3 "gone before I could answer"**. A handler emailed support directly, which is unusual.
- The 60s ping wait explains the 1/3. **Nobody has explained the 2/3.**
- Open question from Marcus (14 Aug), still unanswered: did the routing change mean to apply to responders who've been turning jobs down, or did that just fall out of the config? Wen deferred it until her return (24 Aug).
- My hypothesis to test (not a conclusion): the shorter wait creates more *missed* pings. Misses lower routing priority, and the 4.2 reweighting may compound that. Responders then sink in the order and stop getting pinged, which would explain "phone never goes off."
- Priya's view: it's mostly the usual **August dip**. Don't revert 4.2; the proximity change was a three-quarter-old ask from wide-area responders. This is **unverified**. Marcus has repeated it, but nobody has shown a year-over-year comparison.
- Nobody has pulled real numbers. Marcus offered "rough" ones. **Ravi owns the weekly numbers and isn't in the thread.** The team planned to regroup on 4.2 with the new PM about a week after 28 Aug.

### Roadmap gaps (Q3 roadmap last reviewed 30 Jun, so stale)
- All items are still owned by "Priya". They need reassigning.
- **Availability Confidence** (Committed, 4.2) **did not ship** and isn't in the 4.2 notes. Priya says items were squeezed out of 4.2. Agreeing with Helen which are still Q3 commitments is an open, overdue conversation.
- "Ping timeout tuning" (Committed, 4.2) shipped only as the blanket 90s-to-60s cut.
- Requisition approval chains (Supply, Committed, 4.3): the brief's real ask is a second sign-off over $2,000, but scope creep has piled on (escalation, a cross-site dashboard, handler tracking, replacing Halloran's spreadsheet). Phasing is TBD.
- Handler phone app and Shared cover: Exploring for Q4. The phone app brief cites Aunt Dot, but her ask was narrower: notify the handler too when a callout comes in.
- Older briefs: Bulk callout (shipped 4.1) and Routing override audit log (shipped 4.0).

### Priya's handoff to-dos
- Write down how routing priority works. No document exists; ask Wen. Local code is in `00-rook/code/dispatch-routing/`.
- Expect noise tickets from filter persistence. They're cosmetic.
- Revisit Priya's fast calls in parts of the product nobody has looked at closely.

### Not yet read
Customer interviews database and the Research page in rook-wiki; the `dispatch-routing` code. (All read in Module 1. Findings below.)

- Data (rook-database pings/callouts, 29 Jun–6 Sep) shows that since 4.2, Farlight, The Undertow, Meteor Mite and Vesper dropped from about 12 to about 3–4 pings a week. Their home-area callouts now go to responders from other areas, while The Gale, Nightwell and Stormwrack gained.
- Likely mechanism, in `00-rook/code/dispatch-routing/`: a missed ping costs the same −0.12 as a turn-down, against +0.08 for a take. Scores never decay (2019 TODO), and the 60s wait raised misses about 4–5x. Stuck responders never get asked, so they can't recover. Unconfirmed: ask Wen. The database has no routing scores.
- Aggregate acceptance recovered (54% to 73%) but hides this. Tickets stayed 3–4x normal through 7 Sep, and untaken callouts roughly doubled. The "August dip" can't be tested because the DB has no data before 29 Jun.
- Most common theme: losing pings ("phone never goes off" plus "gone before I could answer") is about 42% of post-4.2 tickets, and all 4 interviewees raised it unprompted.
- Most reliable sources: the DB and the routing code, then tickets and interviews. Least reliable: Priya's handoff, the Glossary and README (which the code contradicts), and the briefs and roadmap (stale).
- No 4.2 notes from Wen exist. The CHANGELOG in the code folder is unsigned and lists the weight change (proximity 0.45 to 0.60, acceptance 0.40 to 0.25) and the 90s-to-60s wait.
