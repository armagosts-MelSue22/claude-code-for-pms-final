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

<!-- Keep the block above at the top of this file. Everything you add
     during the course goes below this line. -->

---

## Working context

I'm the new PM for **Rook Dispatch**, started the week of 1 September 2026, following Priya Raghunathan (departed 21 Aug 2026 after 14 months).

### The company
Rook Industries builds coordination and provisioning software for the protective-response sector — its customers are independently-operating responders and the handlers/quartermasters who support them. Founded 2014, ~241 employees, HQ at Site Aleph with offices in Berlin, Singapore, and Cornwall. Ships on a monthly release train, point releases numbered 4.x. Contractual rule: production never stores a responder's legal identity — only capability tags, availability, and callout history. Don't try to work out who anyone is.

### The two products
**Rook Dispatch** (mine) — Responder coordination: ranks available responders against an incoming incident and offers the callout to the top-ranked one; if declined or timed out, it moves to the next. Users: handlers (web console — incidents, coverage, overrides, availability/capability management) and responders (mobile — accept/decline, set availability). Headline metrics: **acceptance rate** (offers accepted vs. declined/timed out, reported weekly), **time-to-accept** (median seconds to accept), **coverage gap** (incidents with no capability-matched responder available). Routing config ships with the monthly release, not runtime-adjustable by handlers. Current release: 4.2.

**Rook Supply** (not mine, but adjacent) — Gear provisioning: handler raises a requisition → quartermaster approves → fulfillment tracked → each item gets a maintenance schedule off its service interval; field failure reports can pull maintenance forward. Supply **reads** the Responder Availability Record (written by Dispatch) to schedule maintenance around low-callout windows — one-directional dependency, no write-back.

### Vocabulary
- **Responder** — accepts callouts, not a Rook employee, exists in-system only as capability tags + availability, never a legal identity. **Handler** — manages a responder/small group (availability, gear, readiness); the actual product user most of the time. **Quartermaster** — owns equipment stock/approvals, Supply-side. **Cover identity** — a responder's public persona; Rook holds no mapping to a legal identity.
- **Callout** — a request for a responder to attend an incident. **Callout offer** — a callout presented to one responder, awaiting accept/decline. **Callout timeout** — how long an offer stays live before moving on (currently 60s, cut from 90s in 4.2). **Decline** — an active refusal, distinct from a timeout in the data though both move the callout onward.
- **Routing priority** — the ranking score for available responders on a callout; inputs are proximity (travel-time estimate), availability, capability match, and recent acceptance history (declining/timing out lowers this and thus future priority). **Capability tag** — competency labels matched to incident needs: flight, structural-entry, hazmat-tolerant, cold-weather, aquatic, crowd-management, de-escalation. **Mutual aid** (cross-region coverage) is not supported today — Q4 exploration.
- **Responder Availability Record** — shared record of responder availability; Dispatch writes it, Supply only reads it.
- Supply-side terms you'll hear on shared calls: **requisition**, **field failure report**, **service interval**.

### People
- **Helen Achebe** — Director of Product, Dispatch & Supply, Chicago. Owns the roadmap and commitments; changes to committed roadmap items go through her.
- **Marcus Oyelaran** — Engineering Manager, Dispatch, Chicago. Default first call when unsure about anything; can pull rough numbers on request.
- **Wen Li** — Staff Engineer, Dispatch, Berlin. Built the routing/ranking logic; there is no document explaining how it actually decides who gets pinged — that conversation has to happen with her directly. (Was on PTO 14–24 Aug, missed the early 4.2 discussion.)
- **Sofia Marino** — Product Designer, Dispatch, Chicago. Owns console and phone app.
- **Nadia Hoffmann** — Support Lead, Dispatch & Supply, Berlin. First to see complaint/ticket volume; worth a standing check-in.
- **Ravi Menon** — Data Analyst, Dispatch & Supply, Singapore. Owns the real weekly acceptance-rate numbers; requests go through #data.
- **Priya Raghunathan** — predecessor, departed 21 Aug 2026. Left a handoff doc (`company/notes/handoff-from-priya.docx`) — worth rereading directly for tone/judgment calls, not just facts.

### Where things stand
- **4.2 shipped 12 Aug 2026.** Headline change: routing weight rebalanced to favor proximity over recent acceptance history — a long-requested, deliberate change for responders working wide geographies, not a bug. Also: callout timeout cut 90s→60s, console filter persistence (cosmetic), three defect fixes.
- **Open and unresolved since 14 Aug:** Marcus asked whether the routing change was meant to also apply to responders who habitually decline jobs, or only to "everyone else" — the config doesn't distinguish, and nobody has confirmed whether that was intentional.
- **Live situation:** callout tickets running ~3x normal since ~13 Aug, two distinct themes — (1) "phone never rings" (unexplained) and (2) "offer gone before I could respond" (explained by the timeout cut). Roughly 2:1 split as of 26 Aug, holding steady, not worsening.
- **Priya's read (a judgment call, not settled):** likely mostly seasonal (August is soft every year) confounded with the timeout change, expected to recover in September. She explicitly warned against relitigating or reverting the routing change — "reverting it just trades one angry group of responders for another."
- **Plan going in:** team deliberately held off a proper 4.2 review to give the new PM (me) a week to look at it fresh before forming a conclusion — that regroup is due now.
- **Roadmap (Q3 2026, owner Helen):** Dispatch 4.2 committed the routing change and timeout tuning — both shipped. **Availability Confidence** (support-escalation-driven, surfaces a confidence score alongside a responder's stated availability) was also Q3-committed for 4.2 but does *not* appear in the 4.2 release notes — likely one of the items Priya said got squeezed out, unconfirmed with Helen. Supply 4.3: requisition approval chains, committed. Q4, still exploratory: handler phone app (Supply), shared cover between responders (Dispatch).
- **Still undocumented:** how the routing/ranking algorithm actually decides who gets pinged — on me to get this written down, per Priya.
- **Leading hypothesis on "phone never rings":** 4.2's heavier proximity weighting may interact with the recent-acceptance-history component to suppress recent decliners/timeouts from rotation almost entirely — would explain both the unresolved 14 Aug question and the ticket theme in one mechanism. Not yet confirmed.
- **Single highest-value data pull before acting on anything:** segmented acceptance data — do "phone never rings" complainants cluster among recent decliners, or not (pull from Ravi + Nadia together). This determines real defect vs. seasonal noise, and which feature below is worth building.
- **Two features shortlisted to prioritize once that data lands:** (1) resume/ship Availability Confidence if it was only cut for time; (2) fix the routing/acceptance-history interaction if the mechanism above is confirmed. Don't touch routing weights before the data exists — Priya's warning against relitigating 4.2 still holds until there's an actual mechanism to point to.

### Added 10 Sep 2026 — after reading tickets and interviews side by side
- **The "2:1 split" is the wrong frame.** Not two themes with a ratio — one loop with two observation points: suppressed ranking → long silence → phone isn't in hand when the rare offer lands → 60s timeout → history score drops → deeper suppression. Tickets order themselves this way (silence first, vanish second, never the reverse), which is why T-020 and T-025 file both halves and can't say how they're related. Corollary: the ratio isn't a triage input, and "holding steady, not worsening" is what a loop at equilibrium looks like — not reassurance.
- **Seasonality and the defect are not competing explanations.** August sets *how many* offers exist; routing sets *who* gets them. Both can be true, and a scarce month makes the allocation problem worse. So Priya's read doesn't need overturning — it needs the allocation question asked alongside it. Kip's pair (Meteor Mite dry / The Gale on fire, same week, same city, same handler, same screen) is the strongest single evidence, because season can't produce divergence *inside* a matched pair. His "all the way to one side or the other, no middle" is a population splitting, not a curve dipping.
- **Don't triage off filer severity.** T-019 is the only High in 25; Halloran shrugs off a mechanically identical event ("didn't think much of it"). Severity tracks frustration accumulated during the preceding silence, not impact — which is itself evidence the two themes are coupled.
- **Separate trust problem, currently unowned:** 9 tickets ask literally whether the account/record is broken (T-004, 006, 008, 009, 013, 017, 020, 021, 022). This will outlive any routing fix and isn't addressed by either shortlisted feature.
- **Third candidate feature, cheap and routing-safe:** responder-visible callout history. T-008 — Ironvale asked to see her own history and "there isn't, not on her end." Defuses most of the trust tickets, touches no routing weights, ships independently of the data pull.
- **Research-design gap to raise with Sofia and Nadia:** all four interviews are handlers (zero responders, though 9 of 25 tickets are responder-filed) and all were scoped to console design, so routing only surfaced as asides. Handlers report puzzlement; responders report abandonment. Tickets capture absence, interviews capture events — neither pile should be read alone.
- **Two 4.2 follow-ups nobody has logged:** the 90s→60s timeout cut shipped with no user-facing comms (not one interview participant knows it changed), and filter persistence — the only 4.2 change anyone thanked us for — silently reverts after updates, so Ambrose doesn't trust it. Also: Dot and Kip both independently want a handler-side alert, which is a real mitigation for the timeout race and hasn't been framed that way.
- **Open question for Wen, low confidence:** Halloran says Supply's maintenance scheduling has "gotten smarter about not booking maintenance into a week he's likely to be out." Confirm that inference reads *stated* availability only and nothing offer-volume-derived — otherwise a Dispatch distortion is propagating into Supply and being read as an improvement.
- **For Helen, Supply-side (4.3 relevant):** Halloran's requisition priority field is provably inert (11 days on a cracked vest plate, same queue as boot laces), and field failure reports have no feedback loop back to the filer.

---

## Where the source material lives

All under `00-rook/`. Read the primary sources rather than relying on these summaries when a judgment call depends on them.

### Problem tickets — `00-rook/feedback/tickets/` (T-001 … T-025)
One `.txt` per ticket, 13 Aug – 5 Sep 2026. Handler-filed tickets carry a filer-assigned severity and name the responder; responder-filed ones are one-line mobile messages with no severity.

- **The 2:1 split shows up cleanly here.** ~16 tickets are "phone never rings" (quiet stretch, 6 days to ~3 weeks); ~9 are "offer gone before I could respond." T-011, T-019, T-020, T-025 are the *same responder* reporting both, filed as separate or combined tickets.
- **The pattern worth noticing:** the quiet-stretch responders are the same ones who then lose the rare offer that finally arrives — Nightwell (T-004→T-011), The Undertow (T-005/T-013→T-019), Ironvale (T-008/T-017→T-025), Cindermark (T-020), The Drift (T-022→T-023). That "long silence, then an offer that vanishes in seconds" sequence is direct qualitative support for the routing/acceptance-history hypothesis above.
- **Only one High severity:** T-019 (The Undertow, 31 Aug).
- Responders named across tickets: Captain Vantage, Corporal Ashgrove, Sgt. Falkirk, Nightwell, The Undertow, The Longcast, Ironvale, Halfmoon, Stormwrack, Cindermark, Farlight, The Drift.
- Handlers filing: Ambrose, Yusuf Demir, Marjorie Sung, Desmond Okafor, Teresa Alvarez, Simone Fischer, Graham Petrov, Renata Kovač, Owen Bramwell, Linda Pruitt, Farid Haddad, Beatrice Calloway.

### Customer interviews — `00-rook/feedback/interviews/` (4 transcripts)
All are **Sofia Marino's console-redesign research**, 2–5 Sep 2026 — scoped to design, *not* to the callout problem. The routing/timeout material surfaces as unprompted asides, which is why it's easy to miss.

- **`ambrose.txt`** — Ambrose (handler, Captain Vantage), 2 Sep. Filter persistence landed well but he doesn't trust it (silently reverts after updates); wants capability tag legend closer to hand; larger status-badge text. *Aside:* the half-suited near-miss from T-001, plus the sharpest line in the set — "there was a period where a slower-arriving response of his still landed him the job more often than not, and lately that doesn't seem to hold."
- **`aunt-dot.txt`** — Dorothy Pell / "Aunt Dot" (handler, Vesper), 3 Sep. Works from the kitchen counter, standing; bigger text; wants a handler-side notification, not just the responder's phone. *Aside:* "it didn't used to feel like a fair race, phone to stairs, and now it does" — and, unprompted, she's seen *both* the quiet weeks and the fast-vanishing ones and hadn't connected them.
- **`kip.txt`** — Kip (handler, Meteor Mite + The Gale), 4 Sep. **Dark mode**, repeatedly and emphatically; per-responder alert sounds. *Aside — the most useful data point in any interview:* same week, same city, Meteor Mite bone-dry while The Gale is overwhelmed. Two responders, one handler, one screen: a natural control for "it's just seasonal."
- **`halloran.txt`** — Halloran (handler, Sgt. Bulwark; also runs the gear cage), 5 Sep. **Mostly Supply, not Dispatch** — requisition approval queue (11 days on a cracked vest plate; priority field does nothing, one undifferentiated queue), field failure reports go into a void with no feedback, equipment catalog search is bad. Confirms Supply's maintenance scheduling around low-callout windows is working. *Aside:* Bulwark lost a callout before getting his boots on, sometime in August.

### Other source material
- `00-rook/company/` — `about-rook.pdf`, `dispatch-one-pager.pdf`, `supply-one-pager.pdf`, `glossary.docx`, `release-history.pdf`, `roadmap-q3.pdf`, `who-does-what.xlsx`; `notes/handoff-from-priya.docx` and `notes/dispatch-slack-thread.txt`.
- `00-rook/data/callout-history.csv` — the callout-level data.
- `00-rook/code/dispatch-routing/` — the actual routing code (`routing.py`, `history.py`, `config.py`, `offer.py`, `availability.py`, plus README and CHANGELOG). This is where the undocumented ranking logic actually lives.
