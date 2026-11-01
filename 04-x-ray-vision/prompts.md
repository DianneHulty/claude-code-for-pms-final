# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, type wrap up, and Claude Code saves the
prompts you wrote yourself below — not the starter prompt — updates
CLAUDE.md, and saves your work to GitHub. By Module 6 this file is a
prompt library built from your own questions.

---

### 1.
give this to me in a simple process flow

### 2.
do you see any patterns here? point to specific metrics, and hypothesize what can be the reason a responder who should have been notified never gets a buzz

### 3.
can you dive into your points on 'The two parts that hurt responders after 4.2 are both on the “No or missed” box' a bit more and convey it to me thru a simple high level chart (not a bar chart)

### 4.
keeping this information in mind, give me a visual breakdown of what was deployed from the code for 4.2 release that are related to the 2 issues above

### 5.
putting the conclusions you've come to and having the data to back it up, answer this :Based on what I found, the reason some responders are getting no pings at all is  <fill in the blank>, because  <fill in the blank>.

### 6.
give this to me in a visual line chart that clearly breaks down the data and defines a straightforward  answer for Marcus

### 7.
does the data uniquely support the theory that missing pings in release week decided who sank and that a miss is scored as a no, or whether several different explanations fit equally well.

### 8.
out of the likely responder patterns you see in the data, test each prediction one by one and tell me which predictions pass and which fail, using highly simple charts

### 9.
give me data or interview responses that back up the most likely hypothesis

### 10.
if a miss doesn't impact the score, breifly model that impact in a simple visual, not in a bar chart

### 11.
Of all 3 steps you pointed out above, which ones are actually currently preventing them from getting work? Don't describe the process again, identify the actual bottleneck.

### 12.
if for instance every hero became perfectly responsive tomorrow, how long would it take for that to improve their score for getting more work?

### 13.
find any responders who went quiet and later became active again, and compare their trajectories

### 14.
I've been reaching the conclusion that the 60s change from 90s and not updating the scoring are both the root issues of the 4.2 release. using data to back you up, does this seem like the correct conclusion and project realistic outcomes for what in the scoring could be updated to resolve the issues 'the four' are facing
