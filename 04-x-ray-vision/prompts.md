# 04 · X-Ray Vision — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. Last session the numbers showed you what happened: a
ping used to wait ninety seconds and now it waits sixty, people
missed pings they used to catch, and missing one counts the same as
turning one down — so four responders stopped hearing from us
altogether.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

### 1.

Open the folder 00-rook/code/dispatch-routing/. This is the part of our software that decides who gets asked to take a job. I have never read code before and I am not going to start now. Walk me through what happens from the moment something goes wrong somewhere to the moment a responder's phone buzzes, in plain English, no jargon. Then tell me which file each step lives in.

### 2.

Has anything in this code changed recently? Walk me through what's different, and why it would matter to a responder.

### 3.

So how did some responders end up getting no pings at all? Answer that question and then also give me a diagram that helps me explain the before state of the code handling and the state after August 12.

### 4.

Give me the full path to the html page to open in the browser

### 5.

Complete this simple sentence (fill in the blank): "Based on what I found, the reason some responders are getting no pings at all is ____, because _____.

### 6.

What code files were changed in 4.2 that caused the issue?

### 7.

What changes would you suggest to the code to eliminate this? What's the right algorithm and weighting to make sure this doesn't happen?

### 8.

Do you have any information on who made the code change in 4.2? Which developer, team, etc.?

### 9.

What is your confidence level that your first fix will change the scoring?

### 10.

Which file has last session's numbers in it? Read that alongside this code. Does it back up what I found in Lab A? And does that answer Marcus's question above?

### 11.

So how would I answer Marcus' question in simple words?"Quick one — did the change to who gets asked first apply to responders who'd already been turning jobs down, or just new ones? Can't tell from the code and don't want to guess on this. Anyone know?"

### 12.

Find me the part of this code that takes points off somebody when they miss a ping or turn one down. Show it to me and explain it in plain English. Then find me every single thing in this code that puts points back on.

### 13.

Somebody has been quiet for a month. Walk me through, step by step, exactly what would have to happen for them to start getting work again.

### 14.

Fill in the blank in this sentence - "According to my findings in the code, for someone who's gone quiet, they would need to ___."

### 15.

How many times would they have to do that before they rise up in the list

### 16.

Revise this in a story that I can use with conversation with my customers, and give me a visual to support.

### 17.

Give me the full path to the html

### 18.

Show me specifically the impact of the having to do it 7 times, and include on the diagram an explanation

### 19.

What's the statistical probability that the responded would be able to go through that sequence 7 times in a row successfully?

### 20.

Summarize in a couple of sentences
