# 03 · Rewind — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly.

Last session you read four conversations and every support ticket
since 4.2 — and found the two piles did not agree.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.

After release 4.2, what happened to response speed? Give me the median response time, and show me this info in a simple to read comparison chart

### 2.

give me an analysis of the data by responder activity level/volume, broken down into responders, and show me where you pulled that data from

### 3.

now assume the initial before and after analysis is incorrect or misleading. think about anything you may have missed and give me confidence levels you think they could be alternatives, as well as the underlying rows that do or don't support them

### 4.

show me this info in a digestible way that you think would make most simplistic sense, other than a bar chart

### 5.

Compare the support_tickets table on rook-database against the pings table. Do they agree with each other? When do the numbers actually move?

### 6.

where are there gaps with handlers filing or not filing tickets? Find the dates with the largest disagreement between support_tickets and pings. and present this to me in a digestible kanban/card way like you did earlier

### 7.

If I only looked at the pings table, what conclusion would I draw that turns out to be wrong once I incorporate support_tickets? Show the evidence.

### 8.

What if there were multiple changes happening at different times? Explain as if I were a 5th grader in 5 brief sentences or less

### 9.

what do you conclude is the most surprising thing in the relationship between these tables and isnights? Tell me in 5 short bullet points or less

### 10.

are there gaps with handlers filing or not filing tickets?

### 11.

Pick one responder from the pings table who went quiet. Show me every week since June: how many pings, and what they did with each. Use their name.

### 12.

Pick one responder who was interviewed from the pings table who went quiet, and one from th other 12 that got more active. Show me every week since June: how many pings, and what they did with each. Use their name.

### 13.

make a digestible line chart with the scoring comparing these 2, as well as 1 other that was interviewed vs 1 that wasn't

### 14.

I want to see if these two responders are outliers. Find the five most similar comparison of responders to each and tell me whether they followed the same trajectory or not.

### 15.

if this is a  pattern that reflects a broader 4.2 change rather than individual behavior, what evidence supports that interpretation, and what evidence argues against it?

### 16.

from this information, and using real data to support it, give me a hypothesis you could conclude from what's causing why one responder went quiet and the other became more active.

### 17.

compare all responders whose behavior changed at roughly the same time. What common event or circumstance appears to connect them? Tell me in a couple sentences and convey this point with a line chart
