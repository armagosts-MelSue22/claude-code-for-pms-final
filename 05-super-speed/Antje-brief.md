# What we'd build alongside turning the timer back up

For Helen. Rough on purpose.

**Owner:** Antje Barth, PM on Dispatch. Marcus's team builds, Sofia designs
the screens. Figures come from `callout-history.csv`, the routing code, the
tickets and the interviews; the working is in `4.2-investigation.md`,
`03-rewind/prompts.md` and `04-x-ray-vision/prompts.md`.

---

Engineering can put the callout timeout back to 90 seconds this afternoon. It
is one number in `config.py`, and it would help The Undertow catch the one
offer he still gets each week. It would not get him more. The timer decides
how long he has to answer. The recent-acceptance score decides whether he is
asked at all, and his is on the floor. So we should turn the timer back, in
the same release as what follows and said out loud, not on its own.

**Wen's 2019 note asked whether the score should ease back toward neutral on
its own. It should.** Today a bad week lasts forever: only accepting an offer
adds points, and only people with points get offers. Decay is not in this
build. It drifts people toward the middle, which rescues the next person, not
someone already on the floor.

That sets the order. In this code the score lives in memory and is never
saved. If production does the same, every deploy resets all sixteen
responders to the middle, and an alert or a lift built on the score would not
last. I don't know yet, so the piece that depends on nothing ships first.

## Who it is for

**The Undertow**, aquatic-tagged. Six steady weeks of eleven to thirteen
callouts, nine or ten taken. Then the release: eleven offers, four caught.
Then four, one, one, and none taken. When a callout goes elsewhere he sees
nothing. On 26 August he wrote to us from his phone, which responders almost
never do:

> "nothing again this week. starting to wonder if im still even in the system"

He is. Nobody could tell him, because nothing records that he was considered
and passed over.

**Desmond Okafor**, his handler, sees a console card that never changes. He
filed T-005 on 19 August marked Low, calling the quiet "a little puzzling." On
31 August he filed T-019 marked High, after the one callout that arrived
vanished before The Undertow could read it. Nothing happened in between,
because there was no signal to act on.

## What changes, in order

1. **Record every offer.** Who it went to, which callout, their place in the
   order, whether the phone showed it, and what came back. The code knows all
   of this when it takes a point off someone, and keeps none of it. It depends
   on nothing else, so it ships first. It makes T-013 answerable the day it is
   filed, and tells us from data whether scores survive a deploy.
2. **Tell Okafor.** When a responder's score drops below a threshold, the
   handler gets an alert: who, when, and how many callouts since. If every
   score started release week at 0.5, as the code suggests, all four affected
   responders crossed 0.2 that week. Okafor would have known around 16 August
   instead of working it out by the 19th.
3. **Tell The Undertow.** His phone says he is still active, where he stands,
   and what came up near him this week. Not a score.
4. **Let someone lift him.** If scores survive deploys, the other fifteen sit
   at the top of the range, and from the floor he needs thirteen accepts in a
   row to reach them, at about one offer a week. He cannot climb out. The lift
   goes to the top of the range: the middle would leave him below all fifteen
   for seven more accepts. Every lift is recorded with who did it and why.

## What a lift costs

A lift from the floor to the top is worth about nineteen minutes of travel
time in the ranking, enough to reorder two responders who live near each
other. Kip handles Meteor Mite and The Gale in the same city. Since the
release Meteor Mite has gone from eleven offers a week to one, and The Gale
from thirteen to twenty-one. Lift Meteor Mite and The Gale is asked less. That
is the point, and it should be said before we build it.

Three decisions are not mine alone:

- **Who gets the alert** when a handler holds several responders.
- **Who may lift someone**, Okafor or only Marcus. Helen's call.
- **What a second lift in a month means.** If it keeps happening, the ranking
  is wrong.

## What it does not do

- **Change the ranking weights.** A separate argument.
- **Reset anyone silently.** That is the failure being fixed.
- **Promise The Undertow work.** It tells him where he stands, including when
  he is behind.
- **Touch Supply.** Halloran's eleven-day wait on a cracked vest plate is real,
  and it is not this.

## How we'll know it worked

No responder stays below the threshold for more than a week without their
handler told and a decision recorded. A week, because that is one cycle of
Ravi's weekly report. For The Undertow it was nineteen days, and no decision.
Fewer tickets would not prove it, because the worst affected never filed. The
offer record shows it directly.

## Something to click

`prototype.html` walks his month in five screens, with a switch between today
and the proposal. A mock, not a build.

## What I still need

- **Wen, first:** where is the score stored, and what happened to it on the
  12 August deploy? If it resets on every deploy, the alert and the lift need
  a different design.
- **Wen:** is 0.2 the right threshold, or just where these four landed?
- **Marcus:** is anything like the offer record already kept, in the 4.0
  routing override audit log or elsewhere?
- **Ravi:** callouts created per week since June. Accepted callouts fell by
  about a hundred over the four weeks after release while offers held steady.
  Either a hundred incidents found nobody, or the file means something else.
  That is bigger than this brief.
