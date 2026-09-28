# Fable brief — the standing agenda for the next Fable pass

**This round's product:** unblock the Opus queue, then freeze `protocol-v0.3.md`.
**Written:** session 26 (2026-09-28), by Opus, from implementation and from launch findings.
**Rewrite this file each Fable round.** It is the one place a Fable session starts.

> **Read first, in this order:** this brief → [`v0.3-plan.md`](v0.3-plan.md) §8 and §8b
> (the full write-ups; this brief does not duplicate them) →
> [`roadmap-tracks.md`](roadmap-tracks.md) § "A blyg as a publishing target".
> `CLAUDE.md` has the locked decisions (#1–#29) and the model-routing rule.

## What this session is and is not

**Is:** deciding protocol semantics, cross-client invariants, security/API-surface shape,
and writing normative prose. **Is not:** implementing. Every item below produces a
*decision* plus enough written rationale that an Opus session can build from it without
guessing. Where a decision is "no change", say so explicitly and say why — a recorded
refusal is worth as much here as a change, and several of this project's best calls are
refusals (#11 identity, #12 metrics, #25 unstyled generation).

**Do not relitigate locked decisions** without Venkat in the room. Two are load-bearing
across most of what follows: **#11**, identity is an opaque client-asserted pass-through
and not in the protocol; **#12**, no metrics, ever.

**Governing sequencing rule — #21:** building is how this protocol gets tested, and
**testing precedes normative prose.** That single rule decides most of the partition
below: a construct that has not been built and exercised cannot enter 0.3, however good
the argument for it.

## The situation this round inherits

Measured live 2026-09-28, not recalled (full table in `roadmap-tracks.md`):

| | |
|---|---|
| Live blygs in the directory | 17 listings — 11 blygs, 6 feeds |
| Distinct client implementations | **7** |
| Of those, not ours | **6** |
| Clients with no locatable source repo | **5 of 7** |
| Nodes still on protocol **0.2** | **4 of 11** |
| Third-party tools authoring *into* blygs | **3** |
| GitHub forks of our repos | **0** |

Three consequences bear directly on the decisions:

1. **Arguments from "there is only one implementation" are no longer available.** Several
   open questions were last argued when that was true. §8's citation question in
   particular was written with "revisit when a second implementation exists" as one of its
   three readings — that condition is met six times over.
2. **A decision that requires coordinated client change is now expensive**, and four nodes
   are still on 0.2 a month after 0.3 shipped. Additive-and-optional is not just the
   tasteful shape, it is now the affordable one.
3. **The reference client is no longer privileged.** It was split out to
   `blygger/blygger-studio` this session; this repo is normative-only. "What our client
   does" stopped being an answer, which is why 1.4 (a conformance suite anyone can run) is
   on the roadmap.

---

# Part 1 — Triage: unblock Opus

**Do these first.** Each has Opus work queued behind it and nothing else in its way. None
requires normative prose to be written this round — a recorded ruling is enough for Opus
to proceed.

**The honest scope note:** only **three** questions block Opus, and they block about four
work items. Everything else in the Opus queue — 2.3, 2.5, 2.7 (Phase B tasks 12–16), 3.1,
3.2, 3.3–3.6, and most of the studio backlog — runs in parallel and needs nothing from
this round. So this part is small on purpose; do not let it grow.

### T1. ⚠️⚠️ The write surface: does the protocol say how an authoring client writes to a blyg?

**Blocks:** roadmap-tracks **2.9** (the whole `/api` contract, HIGH, nothing else in its
way) and the **owner-password-reset** backlog item, which must not be built before the
auth model is known or it will be rebuilt immediately.

**Raised by Venkat, session 26:** *multiple authoring clients may be publishing to the
same blyg, so the publishing surface will become a publishing target for many authoring
clients.* Already true.

**Gated on two counts** of the routing rule — API-surface design *and* security/crypto.
Full write-up with the measured state of `/api` and the Micropub comparison:
`roadmap-tracks.md` § "A blyg as a publishing target". The short version of the evidence:
30 endpoints, one principal, a 30-day HMAC cookie over a single shared `OWNER_PASSWORD`,
no scopes, no revocation, no audit, no CORS, no idempotency — so **every third-party
authoring tool that works today does so by holding the owner's master password.**

**What Opus needs from this round** (narrower than the full design):

1. **The routing call** — protocol-normative, non-normative companion note, or each
   client's own business. Note that #21 argues against normative prose here this round:
   nothing is built. A companion note is the shape that lets Opus proceed without
   committing the wire.
2. **The auth model's direction** — tokens with scopes, or something else; and whether
   revocation is in scope. Enough for the password-reset item to be designed once.
3. **Whether `/api` may be versioned and documented as a client contract in the
   meantime**, which is 2.9's actual question and can be answered yes independently of 1.

**Worth weighing:** a normative write API would make a folder-on-a-laptop client
non-conformant for a reason unrelated to publishing — and that client is exactly the
second implementation the release-candidate bar wants. That asymmetry is the strongest
argument for the companion-note reading.

### T2. Should `[[id]]` exist as a plain internal link — and is a link disclosed?

**Blocks:** the plain-linking backlog item's `[[id]]` half. (Its `[anchor](url)` half is
ordinary markdown and is already cleared to ship.)

Reported as a bug twice over; **it is not a bug — the construct does not exist.** Only
`![[id]]` is defined. Detail in §8b.

**The decision is not the syntax, it is the disclosure.** A transclusion is disclosed in
`transclusions[]` and sends a Webmention whose relation is one of exactly
`stub | transclusion | fork`. A link is none of those. So:

- Is an internal link **invisible on the wire** — pure markdown, like any other anchor?
- Or a **disclosed reference**, and if so does it notify the target? A fourth relation is
  a wire change.

**A reason to prefer silence:** it makes linking the one way to cite without telling
anyone, which is a real and arguably necessary affordance in a medium where every other
citation form notifies. Decide it deliberately rather than by omission.

### T3. Staleness — confirm the scope boundary so Opus can ship the useful part

**Blocks:** the bulk-update-stale-snapshots backlog item.

Opus's reading, for confirmation rather than decision: **the simple case needs nothing
from the protocol.** An item document already carries `version`, so a client can poll a
transcluded target and compare. What is genuinely open is *propagation* — the
"staleness-over-DAG" item already listed under v0.4 in [`roadmap.md`](roadmap.md), plus
roadmap-tracks **1.7** (a republish re-sends every reference because `mentions_out` holds
no target version).

**Asked:** confirm the simple freshness check is client-implementable now, so the re-pin
UI can ship against it, and that only DAG propagation waits for 0.4. If that is wrong,
say so — it is the one item here where Opus has assumed rather than asked.

Note the three want the same missing fact: **the last version we saw of something we
point at.** Deciding where that is stored may resolve all three at once.

---

# Part 2 — Freeze protocol 0.3

### F1. ⚠️ Should a citation's human half be on the wire?

**The one true freeze blocker.** Full write-up, three readings, and the session-24
escalation from two constructs to three: [`v0.3-plan.md`](v0.3-plan.md) §8. Do not
re-derive it; it is argued there at length.

Why it gates the freeze: `stub_of`, `transclusions[].origin` and `forked_from` are all
**live 0.3 constructs**. If the ruling is (b) additive, the 0.3 document must carry the
new member, and it cannot be added after the freeze without becoming a 0.4 change.

**What is new since §8 was written**, and it cuts against reading (c):

- Reading (c) was "revisit when a second implementation exists and the cost of *not*
  having it is observable." **Six now exist.** The condition is met.
- Two of the six publish protocol 0.3 and would each have to invent their own label cache
  independently, or show a reader an identity with no words.
- §8b adds a **fourth** construct with the same shape — `generated[].sources`, which
  carries `{id, version}` and not even an `origin`. The accumulation §8 noticed has
  continued.

### F2. Draft `protocol-v0.3.md` (Phase B task 18)

Depends on F1. 0.2 flips to **SUPERSEDED** per decision #23; publishing mechanics are
`spec-publishing-plan.md` §6 and are Opus-safe afterwards (Track 4.4).

**Three documentation-only corrections to fold in** — no decision needed, but they will be
wrong if nobody is told:

1. **The reference implementation moved.** `protocol-v0.1.md` and `protocol-v0.2.md` both
   say "Reference implementation: same repository, `worker/`", which is now false. Those
   two are frozen and were deliberately **not** edited — a superseded spec takes no
   revisions. 0.3 must say `blygger/blygger-studio` correctly.
2. **`generator` is informative** (#18d) and now carries real weight: it is the only
   mechanism by which the ecosystem can be censused, and five clients have no other
   visible trace. Worth stating normatively that no reader may gate behaviour on it.
3. **Feed `<title>` derivation** (`note — excerpt`, items being deliberately titleless) is
   shown in §2.6's example but may deserve prose, since a client changing it unilaterally
   makes the same item read differently in different readers. **Documenting existing
   behaviour only** — the *change* request is deferred, see D4.

---

# Part 3 — Explicitly deferred to 0.4 (do not open this round)

Listed so they are visibly parked rather than forgotten, and so this round does not
sprawl. **All four are barred from 0.3 by #21: none is built, so none has been tested.**
Each is written up in §8b.

| | Item | Why deferred |
|---|---|---|
| **D1** | **Remote TK sources** — may a `[TK]` scope cite a source from another blyg? | Needs `origin` added to `ScopeProvenance.sources`, which today is `{id, version}`. A wire addition plus a disclosure ruling (decision #20's quote-vs-source, now cross-origin). Closest of the four to being worth pulling forward — decide *whether* it waits, if nothing else. |
| **D2** | **Partial quotation** — quote a span, not a whole item | The largest item on the list. Needs a selector *and* a faithfulness guarantee the whole-item form never needed: how does a reader confirm the span is a true extract of the version named? |
| **D3** | **`impyrt`** — externally generated spans | `generated[]` currently asserts *this client generated this, with this model, from these sources.* An imported span asserts something weaker. Whether one field can carry both without devaluing the strong claim is honesty-critical. |
| **D4** | **A leading `#` becomes a real title** | Items are deliberately titleless; `<title>` is a wire field. Introducing titles is a design change, not a rendering fix. (Documenting current derivation is F2.3 and is not deferred.) |

---

## Ordering, and what to hand back

```
T1 write surface ──► unblocks 2.9 + password reset   ┐
T2 [[id]] disclosure ──► unblocks plain linking      ├─ Part 1: do first, keep small
T3 staleness scope ──► unblocks bulk re-pin          ┘

F1 citation's human half ──► F2 draft protocol-v0.3.md ──► (Opus: Track 4.4 publish)

D1–D4 parked for 0.4
```

**Hand back, per item:** the ruling, the reasoning, and — for anything additive — whether
it lands in 0.3 or 0.4. Record each in `v0.3-plan.md` (§8/§8b beside its question) and add
any new locked decision to `CLAUDE.md`'s numbered list, continuing from **#29**. Append a
DEVLOG entry as usual; the devlog is the program's log across all four repos.

**Meanwhile Opus proceeds on:** 2.5 webmention hardening (still the only item with other
people's deployments exposed — three third-party nodes), 2.3 the update path, 3.1/3.2 the
directory alert and health cron, 2.7 Phase B tasks 12–16, and the unblocked studio
backlog. None of it waits on this round.
