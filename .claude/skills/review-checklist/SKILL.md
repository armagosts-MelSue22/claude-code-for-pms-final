---
name: review-checklist
description: Run a fixed six-point check against a brief (or any similar planning doc) before it goes further — owner, success metric, scope drift, problem-before-fix ordering, audience, and assumptions/gaps. Trigger on "review this brief," "run the checklist on this brief," "check this before it goes out," "run the review checklist," "grade this brief," or any similar phrasing that asks to review, check, or grade a brief/doc against the standard checklist — even if worded differently than these examples.
---

# Review checklist

Apply these six checks, in this order, to the brief (or doc) the user points at. Read the whole document first — some checks depend on comparing the start of the document to the end.

For each check, report one of:
- ✅ **Pass** — quote or point to the line/section that satisfies it. Worth 1 point.
- ❌ **Fail** — say so plainly, don't guess or soften it. Worth 0 points.
- 🤷 **Don't know** — the doc gestures at it but doesn't commit; quote the ambiguous part. Worth 0.5 points.

## The six checks

1. **Owner.** Does the brief name a specific person (or role) who owns it — not "the team" or unstated? Approvers, stakeholders, or "CC" lists don't count as ownership.

2. **Definition of success.** Does it say how we'll know it worked — a metric, threshold, or observable outcome? "Improve X" without a way to measure "improved" is a miss. A qualitative bar counts if it's specific enough that two readers would agree afterward on whether it was met.

3. **Scope consistency.** Compare the scope stated at the start (problem framing, goals) against the scope implied by the end (proposed solution, deliverables, asks). Flag anything that grew, shrank, or drifted sideways between the two — e.g., the opener frames a narrow problem but the fix section proposes something broader, or vice versa.

4. **Problem before fix.** Does the document explain the problem — with enough grounding (evidence, mechanism, who's affected) — before it proposes the fix? A brief that opens with the solution and backfills the problem afterward fails this check, even if the problem section eventually shows up.

5. **Audience.** Does it name who the brief is written for (a person, role, or group)? A brief with no stated audience forces every reader to guess the intended altitude and level of detail.

6. **Assumptions and gaps.** Does it call out what it's assuming, what it doesn't yet know, or what's still open — explicitly, as its own section or callouts, not just implied by hedging language scattered through the prose?

## Output format

1. **Scorecard first** — one line per check, numbered 1–6, each with its icon (✅ / ❌ / 🤷), short label, and a one-line reason:

   ```
   1. ✅ Owner — names "Product Manager, Rook Dispatch"
   2. ❌ Definition of success — no metric or threshold stated
   3. 🤷 Scope consistency — end section is broader than the opener implies
   4. ✅ Problem before fix — mechanism + evidence precede the fix
   5. ✅ Audience — addressed to a named person
   6. ✅ Assumptions and gaps — own section lists open items
   ```

2. **Composite score** — add up the points (✅=1, 🤷=0.5, ❌=0), divide by 6, show as a percentage rounded to the nearest whole number, e.g. `Composite: 4.5 / 6 — 75%`.

3. **Verdict** — 2–4 sentences: is this ready to move forward, or what's the single highest-priority gap to close first.

4. **Tiered reaction, based on the composite score.** Always write a short original rhyming poem (4–8 lines) whose *tone* matches the tier — the poem's content should reference the actual score and the actual gaps (or lack of gaps) found, not be generic filler. Wrap it in an ASCII banner in chat that matches the tier's mood. Three tiers, no overlap:

   - **Tier: Celebration — composite > 90%.** Poem is triumphant, proud. Banner is stars/fireworks-style (`***`, `✨`).
   - **Tier: Mediocre — composite 60% to 90% inclusive.** Poem is deadpan, lukewarm, gently roasting ("not bad, not great, technically a brief"). Banner is unimpressed — think a limp party streamer, half a star, a shrug (`~`, `🤷`, `😐`).
   - **Tier: Failed — composite below 60%.** Poem is comedically dramatic about the brief's failure — over-the-top, not mean-spirited toward the user, more "this brief did not survive contact with the checklist." Banner is a big, silly "FAILED" stamp look (`XXX`, `💥`, jagged borders).

5. **HTML report — always generated, every run regardless of score.** Build a single self-contained HTML page (inline CSS/JS, no external dependencies) that mirrors the chat output:
   - A scorecard section: all six checks with their icon, label, and one-line reason (same content as step 1).
   - The composite score, shown prominently (e.g. a large percentage with a progress-bar-style visual, colored to match the tier: green/gold for Celebration, amber/grey for Mediocre, red for Failed).
   - The verdict text from step 3.
   - The tier's poem from step 4, always shown.
   - A tier-appropriate animation, always shown (don't skip animation at low scores — the low scores are exactly what should get the funniest treatment). In every tier, the animation must run as a full-viewport overlay — a fixed-position layer covering the entire screen (`position: fixed; inset: 0`), stacked above the scorecard/poem content (not confined to a small box inside the card, not stuck at the bottom). Use `pointer-events: none` on the overlay so the page underneath stays readable and clickable through it. Every tier uses a **multi-piece scattered animation, like the confetti burst** — many small pictorial pieces (shapes drawn in CSS, or pictographic emoji like 🎉 🤷 💥) scattered across the whole viewport with randomized position/timing/rotation, not one single element. **Never animate words or sentences over the content** — no floating "meh" or "FAILED" text captions drifting across the readable area, since text-on-text is hard to read; a graphic stamp/badge made of shapes is fine, a sentence is not:
     - **Celebration:** the existing confetti burst — many bright rectangles/circles falling and spinning across the whole viewport, with a button to relaunch it.
     - **Mediocre:** the same multi-piece approach, but deliberately underwhelming — e.g. a sparse scatter of grey/drab pieces (or a handful of 🤷 emoji) that drift down slowly and land with a droop, far fewer and slower than the celebration burst. Should read as an intentional joke about mediocrity, not as a broken celebration.
     - **Failed:** the same multi-piece approach, comedically dramatic — e.g. a scatter of cracked shapes, falling 💥 or 🔥 pieces, or jagged red fragments tumbling across the whole screen, optionally paired with a whole-page screen-shake. Keep it lighthearted, aimed at the brief's score, never at the user.
     Reuse one consistent implementation approach across tiers (e.g. all CSS/canvas-based particle systems, no external assets, all full-viewport overlays, all multi-piece) rather than three unrelated techniques.

   Save it in the same directory as the brief being reviewed, named after the brief itself: `<brief-filename-without-extension>-review.html` (e.g. `brief.md` → `brief-review.html`, `Antje-brief.md` → `Antje-brief-review.html`). Never use a fixed generic filename like `review-output.html` — multiple briefs commonly live in the same folder, and a fixed name silently overwrites a different brief's report. Overwrite only the report for *this same brief* on repeat runs (one report per brief, not one per run).

   End the chat response with the full absolute file path to that page (not a shortened or relative link) so the user can copy or open it directly, e.g. `C:\Users\mss14\Projects\claude-code-for-pms-final\05-super-speed\Antje-brief-review.html`. Chat text alone can't show the interactive/animated parts — the HTML page is where those actually live.

Don't rewrite the brief unless asked — this is a check, not an edit pass.
