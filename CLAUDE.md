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
