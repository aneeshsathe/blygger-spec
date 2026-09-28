# Fable brief — the standing agenda for the next Fable pass

**This round's product:** define protocol 0.4 — *if its gates are open.* Otherwise, nothing.
**Written:** session 27 (2026-09-28), by Fable 5.1, at the close of the round that
discharged the session-26 brief in full.
**Rewrite this file each Fable round.** It is the one place a Fable session starts.

> **Read first, in this order:** this brief → `CLAUDE.md` locked decisions **#30–#42**
> (the session-27 rulings) → [`v0.3-plan.md`](v0.3-plan.md) §8c (their reasoning) →
> [`protocol-v0.3.md`](protocol-v0.3.md) **§16** (what is ruled-but-unbuilt, deferred, and
> reserved — the 0.4 agenda is literally that section) →
> [`roadmap.md`](roadmap.md) § "v0.4 — Canopy".

## What the last round settled, so it is not reopened

Session 27 ruled every open protocol question there was: the citation's human half (#30,
`cited`), the write surface (#31, never normative; companion note later), `[[id]]` (#32,
a wire-silent link), staleness (#33, direct only), client source discovery (#34),
identity practice (#35), groups (#36), imported generation (#37, un-deferred), agents
(#38), the reference agent and its contract (#39), changelog notes and version narratives (#40), discovery via references (#41), publication guidance (#42), and the 0.4 deferrals with their
shapes where a shape could be fixed. It drafted `protocol-v0.3.md` under **strict #21** —
built surface only. There is no Part 1 this time: **nothing in the Opus queue waits on
Fable.**

## The gate, and why this round may be a no-op

**#21 is the whole agenda.** A construct enters normative prose after it is built and
exercised. The next Fable round therefore has work to do only once Opus has shipped, and
the two nodes have exercised, what session 27 ruled:

| Gate | What must be true | Then Fable does |
|---|---|---|
| **G1** | blygger-studio emits `cited` on all three references and reads it on import; at least one remote citation has been rendered from `cited` on the *other* node | Promote spec §16.1 into §5.9 as normative text. Small. |
| **G2** | `[[id]]` renders, and a document containing one has been imported by another client without incident | Promote §16.2 into §10.1. Small. |
| **G3** | The studio's token auth exists and **at least one third-party tool** authenticates with a scoped token instead of the owner password | Review **`tn-4` — the write surface** (non-normative, #31), which Opus drafts from the build; Fable only checks it against #31's direction. |
| **G4** | Track 4.4 has published 0.3 and flipped 0.2 | Nothing — but do not draft a 0.4 document while 0.3 is unpublished; two living drafts is one too many. |
| **G5** | The reference agent (2.10) has run against a live node for long enough to have refreshed a snapshot, answered a mention, and authored under its own byline | Review `tn-5` for anything that turned out to want a construct — the refresh scope on the re-bake identity (#38) is the likely candidate. |
| **G6** | The studio emits `changelog[].generated` and the history view has been used across two nodes | Promote §16.6c into §5.2. Small. Decide then whether pinned-event feed entries should carry pinned content (§16.6c's parked candidate). |

If none of G1–G6 is true when a Fable session opens, **say so in one line and stop.**
Do not fill the round with re-derivations. A recorded "nothing to do" is the correct
product.

## Part 2 — the 0.4 definition (roadmap-tracks 1.3), when G4 is true

v0.4 is "Canopy — AI arrives" in `roadmap.md`, and most of it is studio-side with no
wire change. The wire questions it owns are exactly `protocol-v0.3.md` §16.3–§16.5,
listed there with what is already decided:

1. **Remote generation sources** (§16.3) — the *shape* is fixed (the §5.9 reference,
   `origin` omitted for own-origin). The open question is disclosure: when a generator is
   fed a stranger's words, is that a `source` or a quotation? #20's quote-vs-source line,
   now cross-origin. This is the one most likely to be worth pulling into a 0.3 revision
   if TK-from-remote gets built first — but it cannot be built until the wire can say it,
   so Fable moves first here. That is the one place this round may legitimately write
   before a build, and it should say so when it does.
2. **Partial quotation** (§16.4) — selector + faithfulness guarantee. Largest. Do not
   design it without a concrete authoring need on the table; ask Venkat what the actual
   quoting case was.
3. ~~Imported generated text~~ — resolved by #37; nothing for 0.4.
4. **Titles** (§16.4) — items are titleless by design. Default answer is still no; if
   Venkat wants it, it is a wire field and a feed-derivation change together.
5. **Transitive staleness** (§16.5) — may be nothing. Decide *whether it exists* before
   deciding anything else about it.
6. **Two parked feed/surface candidates** (§16.6c, §16.6d) — pinned-event feed entries
   carrying pinned content, and an optional per-item curated responses surface. Both
   leak nothing and both change a rule every reader relies on today; decide each on a
   measured need, not on tidiness.

Also for 0.4, from the tracks page: **1.4 conformance suite** — the Fable half is only
"which clauses are normative vs advisory"; that partition is now mostly legible from the
0.3 text's MUST/SHOULD/MAY, so this may be a short ruling. And the **technical notes** (1.6): `tn-2`–`tn-5` are Opus-written from #35–#39; Fable
reviews each against its decision and nothing more — the field is empty (every client
emits only `name`+`url`), so the notes exist to converge it, not to referee a dispute.

## Do not open

- **#11 identity, #12 metrics** — load-bearing refusals.
- **A normative write API** — #31 closed it, permanently, with the static-client
  argument. The companion note is the only vehicle.
- **A fourth mention relation** — #32 closed it; links are silent.
- **`![[id@vN]]`** — still reserved, still no need.

## Hand back

Per item: the ruling, the reasoning, and whether it lands in a 0.3 revision or 0.4.
Record in `v0.3-plan.md` (a §8d, if the questions are still 0.3-adjacent) or a new
`v0.4-plan.md`, continue `CLAUDE.md`'s locked decisions from **#42**, append the DEVLOG
entry, and rewrite this brief.
