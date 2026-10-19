# Fable brief — the standing agenda for the next Fable pass

**This round's product:** review every decision labelled `Fable review pending` (#58);
take the queued items in [`fable-round-queue.md`](fable-round-queue.md) if chosen on
purpose; otherwise nothing. **Since #58, promotions are Opus's**; Fable reviews them after
the fact.
**Written:** session 38 (2026-10-06), by Fable 5.1. **Rewritten:** session 49
(2026-10-17), by Fable 5.1, after two rounds in one day: the first opened
`protocol-v0.4.md` (#72), reviewed #65, the G11 promotion and #69, and ruled #70–#71;
the second ruled the rest of the queue, #73–#77, in the 0.4 text's second revision.
A third round ruled inline fragment authoring (#78, third revision): tangling is a
studio act, a multi-item publish is one event per item with the thread last, readers MAY
fold a burst; `ignyr` (#77) does not fire on new fragments, so folding is a second derived
input to the same queue.
**Rewrite this file each Fable round.** It is the one place a Fable session starts.

> **Read first, in this order:** this brief → `CLAUDE.md` locked decisions **#70–#77**
> (session 49; #59–#69 the two rounds before; #43–#51 the 0.4 definition) →
> [`v0.4-plan.md`](v0.4-plan.md) **§11–§12** (the reasoning) →
> [`protocol-v0.4.md`](protocol-v0.4.md) **§16.9–§16.10** (ruled shapes awaiting
> builds: `pinned_at`, `cid`, author-sized images) and §16.6 (the write surface) →
> [`fable-round-queue.md`](fable-round-queue.md) (**item 22 only**) →
> [`backlog.md`](backlog.md) → [`opus-brief.md`](opus-brief.md) § session 49.

## What session 49 settled, so it is not reopened

**0.4 is the living text**: remote generation sources (§5.7, `source` in §15.4) and the
manifest-located surface (§4, §6.1, §12, §15) are normative; 0.3 is SUPERSEDED, first
snapshot `2026-10-18` (UTC date). **The label** `[[id | text]]`, `![[id | text]]`
(#70), id first under **#71, received conventions trump cosmetic preference**; any other
bracketed text literal; tangling gets no construct. **The second revision**: member
tables for the item document and manifest, measured against all 22 live blygs (#76); the
conformance clarifications F2, F3, F5, F17 (#76); a same-hash republish is maintenance
and readers MAY queue it low — `ignyr` is a reader-side queue, not a flag (#77, the one
ruling that ran against a shape Venkat had assumed; he said record it); a pin MAY re-send
its mentions (#73); `pinned_at` and `cid` are ruled shapes on pinned changelog entries
(#73, #74, §16.9); author-sized images take Pandoc's `{width=…}` (#75, §16.10); the
pace-layered order becomes **RFC-3** with reference constants (#73).

## Review first — decisions Opus ruled (#58)

A Fable round begins by reading every entry labelled `Fable review pending`, in order:
confirm (strike the label), amend (edit the entry, note the amendment with the date), or
reverse (a new numbered decision that says why). Check each against the four questions as
well as on its merits — an entry that should have stopped is the finding that matters most.
**Pending now: nothing.** Opus's builds from session 49 (label, TK strip, `pinned_at`,
image size, L0 `stub_of` refusal, the *ignyr* queue) are implementation of ruled text.
**Promotions Opus performs under #58 when their gates run**, for Fable to review after:
§16.9 `pinned_at` → §5.2 and §8 once the studio emits it and a second client reads it;
§16.9 `cid` → the same once any client emits it and a mirror reads it; §16.10 image size
→ §10.1 once emitted and imported across two nodes.

## The gates

| Gate | What must be true | Then Fable does |
|---|---|---|
| **G3** | Kyle's token auth has shipped under #52's four invariants (it has, 0.28) and **at least one third-party tool** authenticates with a scoped token | Review **`tn-4` — the write surface** (non-normative, #31), which Opus drafts from the build. |
| **G5** | The reference agent (2.10) has run against a live node long enough to refresh a snapshot, answer a mention, and author under its own byline | Review `tn-5` for anything that wants a construct — the refresh scope on the re-bake identity (#38, backlog §1) is the candidate. |
| **RFC-4 draft** | Opus has drafted RFC-4, responding from anywhere (#82; `opus-brief.md` item t) | Edit and rule it as RFC-3; publish on blyg.blygger.org with a comment period as RFC-1 and RFC-2 had |
| **RFC-3 draft** | Opus has drafted RFC-3 (pace-layered order) from `blygger-studio/docs/pace-layered-feed.md` and the fixtures (`opus-brief.md` item i) | Edit and rule it as #67/#68 were; publish on blyg.blygger.org with a comment period. |

If nothing is pending review and no gate is true when a Fable session opens, **say so in
one line and stop** — or take item 22 if Venkat has decided to.

## Next round

0. **The four unreviewed session-29 explorations in `plans/`** (Venkat, session 49):
   `unicode-support-proposal.md` — including whether its §0 conformance-cost criterion
   ("a normative rule must be satisfiable from a standard library") belongs in spec §3;
   `pour-over-links-proposal.md`; `rich-editor-proposal.md`; `publishing-patterns.md`,
   whose §4 blocked genres are the protocol-relevant part and whose publication on
   blygger.org as a fourth genre is an open question. File what survives in `backlog.md`.
   `inline-fragment-authoring.md` was ruled in session 49 (#78, plan §13).
1. **Item 22 — Aneesh's extension registry** ([studio#59](https://github.com/blygger/blygger-studio/pull/59)),
   **only when Venkat says so** — he is thinking about it (session 49). Promised to Aneesh
   as "the round after 0.4 opens". To rule: whether a registry belongs to the program or
   a third party; who reviews an entry and what review promises; browser-only as a hard
   rule; whether blygger.org hosts the index. Read `docs/extension-registry.md` on the PR
   branch first.
2. **RFC-3**, when drafted.
3. **The RFC-1 and RFC-2 comment periods close 2026-11-04**: read the stubs under each on
   `blyg.blygger.org` and the two issues (blygger-spec#14, #15), revise or rule, then gate
   RFC-1's studio work.

## The one standing question — Unicode on the wire (blygger-spec#6)

Not a gate; it opens on choice. `content_hash` differs between NFC and NFD spellings of the
same visible text, the same visible origin serializes to two different strings, three caps
say "characters" without a unit, and two truncations say nothing about where they may cut.
#76 settled line endings for the grammar (LF or CRLF) but not for the hash. Take it only
when no gate is open, or when the first cross-client hash disagreement appears in the
wild. **Measure first:** hash every live item both ways, serialize every live origin
through WHATWG, Python's `urllib` and Go's `net/url`, and count what the caps actually count
in each live client. Nothing here is pre-decided.

## Sequencing notes

- **`tn-2` identity** can be written now (#61, #64, #67 ruled). **`tn-3`** is revised.
- **A technical note on IPFS** (CID profile, pin log) waits on a client emitting `cid`.
- **blygger-spec#12 (Glass Bead Game)** is declined (session 38); parked in `backlog.md`.
- **Kyle's PRs are not gates.** Opus reviews them against #52's four invariants.
- **The snapshot policy stands:** revisions land in the living text and are not
  snapshotted; a snapshot is cut at publication of a new version and thereafter as a
  deliberate act when a third party needs the text to hold still. The second revision was
  not snapshotted.
- **Opening 0.5** needs a construct of #43's second kind built and exercised; none is
  ruled. Do not open 0.5 for a revision.

## Do not open

- **#11 identity, #12 metrics** — load-bearing refusals.
- **A normative write API** (#31), **a mention relation for links** (#32), **a title
  field** (#46), **transitive staleness** (#45), **a maintenance flag** (#34, and now
  #77: a same-hash republish is detected, never declared).
- **Version-significance markup in either direction** — #19, TN-1, #77.
- **The two declined candidates** (#47) unless the trigger named in `backlog.md` §1 occurs.
- **`![[id@vN]]`** — reserved; the variorum (blygger-spec#8) is its named trigger.
- **A second discovery vocabulary** for manifests — #51 chose to extend the existing link.
- **`.well-known` discovery for auth, or any auth key in the manifest** (#52).
- **A general `meta` extension bag on the item document** (blygger-spec#10).
- **Softening #57 for withdrawn quotes** — ruled explicitly in #62.
- **A query-string manifest** — out of scope by #64.
- **Label-first brackets, an escape for `]]`, a wire meaning for `[[text]]`** — #70, #71.
- **A bundle CID, or any media addressing on the wire** — #74 named the document only.
- **Image attributes beyond `width` and `height`** — #75.

## Hand back

Per gate: what was promoted, into which section, in which document. Continue `CLAUDE.md`'s
locked decisions from **#77**, record reasoning in `v0.4-plan.md` §13 (or a new section),
append the DEVLOG entry, file any new ideas in `backlog.md`, prune `fable-round-queue.md`,
and rewrite this brief.
