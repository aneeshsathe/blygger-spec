# Fable brief — the standing agenda for the next Fable pass

**This round's product:** review every decision labelled `Fable review pending` (#58);
take the queued items in [`fable-round-queue.md`](fable-round-queue.md) if chosen on
purpose; otherwise nothing. **Since #58, promotions are Opus's**; Fable reviews them after
the fact.
**Written:** session 38 (2026-10-06), by Fable 5.1. **Rewritten:** session 49
(2026-10-17), by Fable 5.1, at the close of the round that opened `protocol-v0.4.md`
(decision #72), reviewed #65, the G11 promotion and #69, and ruled #70 and #71.
**Rewrite this file each Fable round.** It is the one place a Fable session starts.

> **Read first, in this order:** this brief → `CLAUDE.md` locked decisions **#70–#72**
> (this round; #59–#69 the two rounds before; #43–#51 the 0.4 definition) →
> [`v0.4-plan.md`](v0.4-plan.md) **§11** (the reasoning behind #70–#72 and the review)
> and §7 (the implementation plan Opus built from) → [`protocol-v0.4.md`](protocol-v0.4.md)
> **§16** (what is still ruled-but-unbuilt: only §16.6, the write surface, and the
> reservations) → [`fable-round-queue.md`](fable-round-queue.md) (items 6–8, 10–11, 13,
> 15–16, 18, 20, 22; none blocking; **22 was promised for this round**) →
> [`backlog.md`](backlog.md) → [`opus-brief.md`](opus-brief.md).

## What the last round settled, so it is not reopened

Session 49 opened **version 0.4**: the 0.3 text carried forward with its section numbers,
remote generation sources promoted into §5.7 and §15.4 (the relation set is `stub`,
`transclusion`, `fork`, `source`), the manifest-located surface promoted into §4, §5.8,
§6.1, §12 and §15 (one fixed filename, `blyg.json`; everything else a default the manifest
may relocate), and 0.3 flipped to SUPERSEDED. Three rulings: **`[[id | text]]` and
`![[id | text]]`** — an author's label after the id, ignored by resolution, anchor text on
a link and a comment on a directive; any other bracketed text literal; client-private
bracket forms rewritten before publish (#70) — **received conventions trump cosmetic
preference** (#71, Venkat's principle, the reason the label is id-first), and **tangling
gets no construct** (part of #70). Item 21 was confirmed as prompt-plus-strip. The #58
review confirmed #65, the G11 promotion and #69, the last with its citation corrected to
§8.4 rule 3 and a sentence added to the text.

## Review first — decisions Opus ruled (#58)

A Fable round begins by reading every entry labelled `Fable review pending`, in order:
confirm (strike the label), amend (edit the entry, note the amendment with the date), or
reverse (a new numbered decision that says why). Check each against the four questions as
well as on its merits — an entry that should have stopped is the finding that matters most.
**Pending now: nothing.** Opus's next builds under #58 — the label grammar (#70, ROADMAP
row 66), the TK grammar strip (row 67) — are implementation of ruled text, not new rulings.

## The gates

**#21 is the whole agenda.** Each row opens when Opus's devlog entry says the exercise
ran on both nodes with ids recorded.

| Gate | What must be true | Then Fable does |
|---|---|---|
| **G3** | Kyle's token auth has shipped under #52's four invariants (it has, 0.28) and **at least one third-party tool** authenticates with a scoped token | Review **`tn-4` — the write surface** (non-normative, #31), which Opus drafts from the build. |
| **G5** | The reference agent (2.10) has run against a live node long enough to refresh a snapshot, answer a mention, and author under its own byline | Review `tn-5` for anything that wants a construct — the refresh scope on the re-bake identity (#38, backlog §1) is the candidate. |

G6, G8, G9, G10 and G11 are closed: everything they gated is normative text in 0.4.

If nothing is pending review and neither gate is true when a Fable session opens,
**say so in one line and stop** — or take queue items deliberately, starting with 22.

## Next round, as staged

1. **Item 22 — Aneesh's extension registry** ([studio#59](https://github.com/blygger/blygger-studio/pull/59)).
   Promised "the round after 0.4 opens", which is this one. To rule: whether a registry
   belongs to the program or to a third party; who reviews an entry and what review
   promises; browser-only as a hard rule; whether blygger.org hosts the index. Read
   `docs/extension-registry.md` on the PR branch first.
2. **Items 15 and 20 together** — the pace-layered RFC (`pinned_at` on changelog entries)
   and `cid` on pinned versions (blygger-spec#13). Both put more on pinned changelog
   entries; rule the shape once.
3. **Item 18** — author-sized inline images (studio#37): whether a size belongs in the
   grammar at all. #71 applies: if it does, the spelling follows a received convention
   (Pandoc's `{width=…}` is the one markdown processors share).
4. **Session-38 leftovers 6–8, 10–11, 13, 16**, any of them, if chosen.

## The one standing question — Unicode on the wire (blygger-spec#6)

Not a gate; it opens on choice. `content_hash` differs between NFC and NFD spellings of the
same visible text, the same visible origin serializes to two different strings, three caps
say "characters" without a unit, and two truncations say nothing about where they may cut.
Take it only when no gate is open, or when the first cross-client hash disagreement appears
in the wild. **Measure first:** hash every live item both ways, serialize every live origin
through WHATWG, Python's `urllib` and Go's `net/url`, and count what the caps actually count
in each live client. Nothing here is pre-decided.

## Sequencing notes

- **The RFC-1 and RFC-2 comment periods close 2026-11-04.** At close: read the stubs under
  each on `blyg.blygger.org` and the two issues (blygger-spec#14, #15), revise or rule, then
  gate RFC-1's studio work.
- **`tn-2` identity** can be written now (#61, #64, #67 ruled). **`tn-3`** is revised.
- **blygger-spec#12 (Glass Bead Game)** is declined (session 38); parked in `backlog.md`.
- **Kyle's PRs are not gates.** Opus reviews them against #52's four invariants.
- **The snapshot policy stands:** revisions land in the living text and are not
  snapshotted; a snapshot is cut at publication of a new version and thereafter as a
  deliberate act when a third party needs the text to hold still. 0.4's first snapshot
  is `2026-10-18` — the UTC date; it was cut on 2026-10-17 PT, at publication.
- **Opening 0.5** needs a construct of #43's second kind built and exercised; none is
  ruled. The version ladder's "Grove" (`roadmap.md`) is RAG and presentation, and its one
  protocol deliverable, the CSS contract, is written. Do not open 0.5 for a revision.

## Do not open

- **#11 identity, #12 metrics** — load-bearing refusals.
- **A normative write API** (#31), **a mention relation for links** (#32), **a title
  field** (#46), **transitive staleness** (#45), **a maintenance flag** (#34).
- **The two declined candidates** (#47) unless the trigger named in `backlog.md` §1 occurs.
- **`![[id@vN]]`** — reserved; the variorum (blygger-spec#8) is its named trigger.
- **A second discovery vocabulary** for manifests — #51 chose to extend the existing link.
- **`.well-known` discovery for auth, or any auth key in the manifest** (#52).
- **A general `meta` extension bag on the item document** (blygger-spec#10).
- **Softening #57 for withdrawn quotes** — ruled explicitly in #62.
- **A query-string manifest** — out of scope by #64.
- **Label-first brackets, or an escape for `]]` in a label** — #70 and #71; a label that
  needs `]]` is rewritten.
- **A wire meaning for `[[text]]`** — #70: client-private forms are rewritten before
  publish.

## Hand back

Per gate: what was promoted, into which section, in which document. Continue `CLAUDE.md`'s
locked decisions from **#72**, record reasoning in `v0.4-plan.md` §12 (or a new section),
append the DEVLOG entry, file any new ideas in `backlog.md`, prune `fable-round-queue.md`,
and rewrite this brief.
