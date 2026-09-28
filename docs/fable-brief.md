# Fable brief — the standing agenda for the next Fable pass

**This round's product:** promote what Opus has built and exercised since session 28,
and open `protocol-v0.4.md` when its first construct is live — *if the gates are open.*
Otherwise, nothing.
**Written:** session 28 (2026-09-28), by Fable 5.1, at the close of the round that froze
and published 0.3 and defined 0.4.
**Rewrite this file each Fable round.** It is the one place a Fable session starts.

> **Read first, in this order:** this brief → `CLAUDE.md` locked decisions **#43–#51**
> (the session-28 rulings) → [`v0.4-plan.md`](v0.4-plan.md) (the 0.4 definition, its
> reasoning, and §7 the implementation plan Opus is building from) →
> [`protocol-v0.3.md`](protocol-v0.3.md) **§16** (ruled shapes awaiting builds) →
> [`backlog.md`](backlog.md) (ideas that are neither ruled nor scheduled) →
> [`opus-brief.md`](opus-brief.md) (what the parallel Opus queue is doing).

## What the last round settled, so it is not reopened

Session 28 froze 0.3 (`cited`, `[[id]]`, `generator_url` promoted; published; first
snapshot) and defined 0.4 by a rule rather than a list: the **version boundary rule**
(#43 — a revision adds what readers ignore safely; a new version is needed when a reader
or receiver must change what it does). Ruled: remote generation sources with a fourth
mention relation `source` (#44); transitive staleness does not exist (#45); no title
field (#46); pinned-content feed entries and a responses surface declined (#47); the
conformance partition (#48); partial quotation as a partial transclusion that is a **0.3
revision** (#49 — the rule corrected its own author's example); the reader-side `[[id]]`
affordance (#50); and, from a public proposal, the manifest-located surface (#51).
**There are no open protocol questions.** Partial quotation's authoring case was
answered in session.

## The gates

**#21 is the whole agenda.** Each row opens when Opus's devlog entry says the exercise
ran on both nodes with ids recorded.

| Gate | What must be true | Then Fable does |
|---|---|---|
| **G3** | The studio's token auth exists (`v0.4`-adjacent, roadmap-tracks 2.9) and **at least one third-party tool** authenticates with a scoped token | Review **`tn-4` — the write surface** (non-normative, #31), which Opus drafts from the build. |
| **G5** | The reference agent (2.10) has run against a live node long enough to refresh a snapshot, answer a mention, and author under its own byline | Review `tn-5` for anything that wants a construct — the refresh scope on the re-bake identity (#38, backlog §1) is the candidate. |
| **G6** | The studio emits `changelog[].generated` and the history view has been used across two nodes (2.12) | Promote §16.6c into §5.2 — a 0.3 revision. Small. |
| **G7** | **Partial transclusion** built (`v0.4-plan.md` §7.3, P1–P9) and exercised: a PI stub quoting one paragraph of a venkateshrao thread, verified `stub` on the far side, `blyg-partial` surviving import, a wrong quote refused at publish | Promote §16.4's partial half into §10.1–§10.3 as a **0.3 revision**; add `blyg-partial` to `css-contract.md` §1; note the build's P4 call (plain text vs inline HTML) as the rule. |
| **G8** | **Remote generation sources** built (§7.2, R1–R8) and exercised: a PI scope drawing on a venkateshrao item, `generated[].sources[]` with `origin`+`cited`, the mention **verified as `source`** on the far side, provenance intact on import | **Open `protocol-v0.4.md`**: a standalone superset of the 0.3 text, 0.3 section numbers preserved, §16.3 promoted into §5.7 and §15.4 (relation set gains `source`), §16.6e carried as a ruled shape until G9. Register 0.4 in `sync_spec.py`, flip 0.3 to `("SUPERSEDED", "0.4")`, publish, cut the first snapshot the same day — in that order (#42). Rewrite this brief. |
| **G9** | A client **not written by this project** publishes through `item`/`pin` templates (the WordPress case, blygger-spec#2) and the studio (§7.5, M1–M4) has subscribed to it, transcluded from it, and sent it a mention that verified | Promote §16.6e into §4, §6.1, §12.1 step 4, §12.2 and §5.8 of the living document (0.4 if G8 has opened it; otherwise it waits, because it is a 0.4 construct by #43). |

If none of G3–G9 is true when a Fable session opens, **say so in one line and stop.**

## Sequencing notes

- **G7 before G8** is the order §7.4 asks Opus to build in; G7 lands in 0.3 as a
  revision, G8 opens 0.4. If both arrive together, do the G7 promotion into the 0.3 text
  first and then draft 0.4 from that, so the 0.4 document inherits partial transclusion
  as normative rather than as a §16 item.
- **G9 depends on someone else.** If the proposer confirms on issue #2 that the
  WordPress side is being built, the studio's reader side (§7.5) should land before it
  ships, so the first templated blyg has a reader on day one. Raise this with Venkat if
  the issue goes quiet.
- **The snapshot policy stands:** revisions land in the living text and are not
  snapshotted; a snapshot is cut at publication of a new version and thereafter as a
  deliberate act when a third party needs the text to hold still.

## Do not open

- **#11 identity, #12 metrics** — load-bearing refusals.
- **A normative write API** (#31), **a mention relation for links** (#32), **a title
  field** (#46), **transitive staleness** (#45), **a maintenance flag** (#34).
- **The two declined candidates** (#47) unless the trigger named in `backlog.md` §1
  occurs.
- **`![[id@vN]]`** — still reserved, still no need.
- **A second discovery vocabulary** for manifests (`rel="alternate"` + a media type) —
  #51 chose to extend the existing link instead.

## Hand back

Per gate: what was promoted, into which section, in which document. Continue `CLAUDE.md`'s
locked decisions from **#51**, record reasoning in `v0.4-plan.md` (§8 if needed), append
the DEVLOG entry, file any new ideas in `backlog.md`, and rewrite this brief.
