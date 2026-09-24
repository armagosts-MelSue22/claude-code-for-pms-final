# 06 · Sidekicks — prompts

**Context:** You joined Rook two weeks ago as PM on Dispatch.
Release 4.2 shipped on 12 August, just before you arrived, and
landed badly. You've spent five sessions on it: finding out what
went wrong, then building the fix nobody had — a working prototype,
by hand, in one sitting, for one room.

At the end of the session, ask Claude Code to save the prompts you
wrote yourself below — not the starter prompt. The closing slide has
the exact prompt to paste. By Module 6 this file is a prompt library
built from your own questions.

---

### 1.

I want to save a habit as something I can reuse. When I review a brief before it goes any further, here's what I actually check for: 1. It names who owns it, 2. It says how we'll know it worked, 3. The scope at the end matches the scope at the start, 4. It explains the problem before it proposes a fix, 5.It names the intended audience., 6. It outlines any assumptions or what it's missing. Turn that into a skill called review-checklist, so I can point it at any brief and get the same check every time, without me explaining it again.

### 2.

claude plugin validate .claude/skills

### 3.

Run the review-checklist skill against my product brief "brief.md"

### 4.

Explain to me what my review-checklist skill does, in ELI5

### 5.

Update my review-checklist to give me output that has pass/fail/don't know icons on it.. I want to see a visual of how well it did. Also give me a composite score % based on how much of my checklist it passed.

### 6.

Now run the review-checklist again on my brief.md

### 7.

Now if a brief passes more than 90% grade, give me a new rhyming poem about how awesome the brief is and do something fance on the screen to congratulate me

### 8.

Can you show me something interactive on the screen like confetti in the skill

### 9.

Update my skill to give me an interactive HTML page with the output and a direct link to the full path to get to the HTML page. Includde this in my skill.

### 10.

No, give me the full file path in the skill output not just a short link to the file

### 11.

Trigger the review-checklist skill when I say keywords like "review this brief", "run the checklist on this brief", and anything similar to that.

### 12.

Grade the brief.md file

### 13.

Now, run the review on Antje's brief

### 14.

Now run it against Harsha's brief

### 15.

Update the failed or mediocre animations to be on the whole screen over the top of the text not just the bottom

### 16.

Make the animation more like the confetti where it's multiple animations for whatever the state is, over the top of the text. Don't put animated text on text though because it's hard to read. Only use pictures.

### 17.

Render the review skill on a ficticious brief that fails

### 18.

Schedule review-checklist to run every 3 minutes, and let me know what it finds. Keep a counter on how many times the skill runs. Only run twice on the schedule and then shut the schedule off.

### 19.

Update the skill to do some mediocro funny animation if the brief passes from 60-90% and give a funny FAILED animation if it passes below 60%
[Request interrupted by user]
Update the skill to do some mediocro funny animation if the brief passes from 60-90% and give a funny FAILED animation if it passes below 60%. Make sure the poem goes along with the scores.

### 20.

Can you make the skill run on a schedule even if my Claude isn't running or my computer isn't running?

### 21.

Before we wrap up, three things. First: look back through this session and find the prompts I wrote myself, not the starter I pasted. Save them into 06-sidekicks/prompts.md, one per numbered slot, exactly as I typed them. Don't tidy them up. Second: add a few lines to the Working context in CLAUDE.md, anything we figured out today that isn't in there yet and that I'd want you to already know next session. Third: commit everything that's changed with a short message describing what this session did, then push. Tell me when it's done and give me the link to my repository on GitHub.
