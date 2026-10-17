# Fable round queue — what is waiting on a ruling

**Written:** session 34 (2026-10-04), by Opus 5.5, at Venkat's request, after triaging
blygger-spec#11 (Aneesh Sathe's conformance toolkit and first-round report) and #12.
**Pruned:** session 38 (2026-10-06), Fable 5.1 — items 1–5 and 15 ruled (decisions
#59–#64, reasoning `v0.4-plan.md` §10), the seventh revision reviewed, the 0.27.0 styling
question ruled (#59). Numbers are kept so earlier references resolve.
**Added to:** session 48 (2026-10-13), Opus 5.5, at Venkat's request before a Fable round:
items 17–21, the roadmap rows marked *needs ruling* that are Fable's under #58, and decision
#69 in section C. Venkat asked for 17 by name and is holding its build for the ruling.
**Read with** [`fable-brief.md`](fable-brief.md), which stays the standing agenda; this file
is the itemised queue for the next round. Delete an item once it is ruled; delete the file
when it is empty.

Each item says what it blocks, why it is Fable's under the four-question test (#58), and
where the evidence is. "Report F*n*" is finding *n* of
`conformance/reports/2026-10-03-first-round.html` in blygger-spec#11 (not merged as of
writing; read it on the PR branch — `gh api` with the URL quoted, zsh globs the `?`).

## A. Blocking — none

Items 1–5 were ruled in session 38: #60 (§9.2 vs #20), #61 (path-mounted verification
and withdrawn stubs, one test), #62 (#57 against F6, partiality from the bake), #63
(#43's third party). G11's promotion is unblocked.

## B. Needs a ruling, blocks no build

6. **Partial quotes that cannot be re-checked from the moment they are published.** Report
   F2, Alloy 6b/6c, two live cases. A quote baked from a stale import names a version its
   origin no longer serves, so §10.2's "any reader MAY re-check" never applies. Bears on
   the intent "verifies that the quoted bit is actually part of the transcluded item".
7. **Which version the local closure check walks.** Report F3, Alloy 1a′ fails, 1a passes.
   Walking each local thread's current `transclusions[]` lets a thread contain itself;
   walking the versions actually baked is safe. §10.2 does not say which. #26's "snapshots
   are static bytes" may settle it; if so, this could have been Opus's.
8. **The watermark should outlive the stored copy.** Report F5, Alloy 4c. If a reader
   deletes both on withdrawal (§13.4 says it SHOULD delete the copy), a stale cache
   replaying v1 is adopted as the item coming back. Importer behaviour, so Fable's.
9. *Decided session 38: Venkat said yes to both; Opus wrote it as decision #65 (§5.6 rules 7–8,
   ninth revision), Fable review pending — see section C.*
10. **No list of required members for an item document or manifest.** Report F14. The
    schemas had to infer them. Writing one could narrow what conforms, so Fable's.
11. **Grammar ambiguities, each with a test case already written** (report F17): what CRLF
    means in §10.1; whether a line after a partial quote without `>` still belongs to it;
    whether a target quoted twice gets one `transclusions[]` entry or two (§10.3); §10.6
    rule 3 read literally lets `stub_of.version` move backwards (Alloy 8b); whether a local
    draft with the same id falls through to the import (§10.2 step 1; the studio refuses);
    whether `stub_of` may name an RSS-wrapper id (one live stub does).
12. *Decided session 38 by Venkat on the `pending-calls.md` recommendation: declined, with the
    working pattern; reply posted on blygger-spec#12; parked in `backlog.md` §1.*

13. **A reserved `ignyr` directive in a changelog note** (Venkat, session 34). It would
    tell feed consumers to move that version to a lower-priority queue: the wire half of
    the batching norm the studio's *updates* tab (0.23.0) sets for refreshing stale
    quotes. A refresh republish, or a batch of them, could carry it so readers are not
    flooded with trivial versions. A new reader-visible construct, so Fable's. Questions
    to settle: where it lives (changelog text versus a field), what a consumer that
    ignores it does (nothing, by §13.1), and whether a future maintenance agent's
    refreshes carry it by default.
    *Venkat, session 43 (on `pending-calls.md` #4):* the name stays **`ignyr`**, and an
    agent's refresh republishes carry it **by default**. Where it lives is still Fable's;
    the recommendation on file is an OPTIONAL boolean on the changelog entry, not a word
    in the note.

14. **Listing public hoppers in `blyg.json`** (session 34). Studio 0.24.0 lists public
    hoppers under *Collections* on the homepage and archive, so people can find them;
    other blygs and the blygger.com directory still cannot. A manifest key naming
    each public hopper's page would let them. It adds a key readers see, so it is
    Fable's under #58. Weigh it against decision #12 (hoppers are curation display,
    never re-emitted on the feed): a list of links is not a re-emission, but it is
    the first time a hopper would be discoverable by machine.
    *Parked session 43 (Venkat, on `pending-calls.md` #5):* moved to `backlog.md` §1 with
    its trigger. Leave it out of the next round.

15. **An RFC recommending the pace-layered home page to other clients** (Venkat, session
    46). Studio 0.42.0 ships it as core and the fresh-install default: the diachronic
    view, beside newest first (synchronic). Venkat's ruling: it is part of the premise,
    so it should be a recommended convention, not just one client's feature. The model,
    the wire-only inputs and the open choices are in `blygger-studio/docs/pace-layered-
    feed.md`; `blygger-studio/test/fixtures/feeds/*.json` are worked histories with
    expected orders, written so another client can check itself. To rule: whether it is
    an RFC (comment period on blyg.blygger.org, per the RFC process) or a technical note;
    whether the constants (six layers, halving per layer and per hop, one place per
    version) are normative or illustrative; and the one place the wire is short — it says
    *which* versions are pinned but not *when*, so the reference counts a pin from its
    version's publication. A `pinned_at` in the changelog would fix that; it is a wire
    change.
16. **Should a pin re-send mentions?** (session 46, from the same work). A thread pinned
    after it quoted another blyg sends nothing new, so the quoted blyg never learns its
    item was frozen into someone's pinned version. That is the only way inbound pins
    could count toward a pace-layered order, and it is why the reference algorithm
    counts own-origin pins only. Options: re-send a thread's mentions when one of its
    versions is pinned (§15/§16 territory; receivers would re-verify and record the pin),
    or have verifiers read the source's `changelog[].pinned` on the periodic re-check.
    Neither blocks 0.42.0.

17. **Author-chosen link text: `[[title | id]]`** (Venkat, session 48; ROADMAP row 66,
    and row 25, which is the same request filed earlier — merge them once this is ruled).
    Today an inline `[[id]]` renders with the target's leading heading or an excerpt as
    its anchor text (`anchorText` in `blygger-studio/src/transclusion.ts`). Venkat wants
    to write the text himself, and asked for the form **title first, then id, with spaces
    around the pipe**. Fable's because §10.1 makes `[[…]]` permanent protocol grammar and
    the new form travels in `content_md`, which readers see (#58 question 2). A 0.3
    reader treats it as literal text; `content_html` would still carry a correct anchor.
    The build waits on this ruling (studio side: a parse change in the link resolver,
    plus tests; small). To rule:
    - **Order.** Title first as asked, or Obsidian's `[[id | title]]`, which writers may
      already type? Ids are 26 characters of lowercase Crockford base32, so either order parses
      unambiguously. Accepting both is a third option.
    - **Whitespace and escaping.** Whether spaces around the pipe are required, allowed or
      trimmed, and what a `|` or `]]` inside the text means.
    - **Which version.** A 0.4 construct (the 0.4 document opens this round, G8), so 0.3
      stays frozen.
    - **Interactions to check.** #46 (no title field: this is anchor text, not an item
      title, and §10.1 already calls anchor text presentation); #54 (inert inside code);
      #63 (inherited `[[id]]` links flatten in a fork, so the text has to survive the
      flattening); #43 (a publisher re-parsing another's `content_md`); and item 19,
      which proposes a different meaning for `[[text]]`.
18. **Author-sized inline images** (ROADMAP row 39, [studio#37](https://github.com/blygger/blygger-studio/issues/37),
    one comment). Every inline image renders at full column width; `<img width>` and
    `{width=…}` are literal text because raw HTML is off. A size has to be written in
    `content_md`, so other clients meet it: Fable's for the same reason as 17. To rule:
    whether a size belongs in the grammar at all, and if so its spelling and what a
    reader that ignores it shows (the full-width image, by §13.1).
19. **Tangling: a `[[text]]` operator that publishes a thread and its fragments together**
    (ROADMAP row 45, [studio#20](https://github.com/blygger/blygger-studio/issues/20), no
    comments). On publish, each marked span becomes its own fragment and the document
    becomes a thread that transcludes them. The published `content_md` holds only
    ordinary `![[id]]` directives, so the wire may not change, but it claims `[[…]]` for
    text that is not an id. **Rule it with 17:** `[[title | id]]` and `[[text]]` cannot
    both mean what their proposals say without a rule that tells them apart.
20. **An optional `cid` on pinned versions** (ROADMAP row 46, RFC on
    [blygger-spec#13](https://github.com/blygger/blygger-spec/issues/13), one comment). A
    changelog entry with `"pinned": true` MAY carry an IPFS `cid`, so a pinned version can
    outlive its origin and a third party can prove its bytes. A new wire member, so
    Fable's. Relates to item 15's missing `pinned_at` (both put more on pinned changelog
    entries) and to the IPFS mapping in session 3's withdraw/pin proposal.
21. **Grammar written by TK generation** (ROADMAP row 67, status *next*). A generated
    paragraph ending in an inline `![[id]]` publishes as literal brackets
    ([example](https://venkateshrao.com/blyg/t/4tsyw2ccaps3h0w1dveqc1f35x/)), because the
    grammar is inert inside generated output (studio#5) and only an own-line directive
    there quotes (#60, 0.20.1). The fix is either to render inline `[[id]]`/`![[id]]` in
    generated text as plain links, or to stop generation emitting the grammar (prompt +
    strip). The first reads generated text as grammar, which studio#5 forbade, and may
    reinterpret #60. If Fable confirms the second, Opus builds it with no ruling.
22. **An extension registry** (session 49, [studio#59](https://github.com/blygger/blygger-studio/pull/59),
    Aneesh Sathe, draft PR with a working implementation; `docs/extension-registry.md` on
    the branch lists its own open questions). A central `registry/index.json` that authors
    PR into and operators pull from with `npm run ext:add <name>`: each entry pins an https
    repo, a full commit and a sha256 tree hash, a supported studio range and
    `"server": false`; the pull runs on the operator's machine, and a studio never fetches
    from the registry at runtime (the `docs/extensions.md` rule holds). Fable's twice over:
    it compiles third-party code into the owner's session, and it proposes that
    blygger.org eventually host the index. To rule: whether a registry belongs to the
    program at all or to a third party; who reviews an entry, and what review promises;
    browser-only as a hard rule; and whether blygger.org hosts it. Session 49 told Aneesh
    to expect a ruling in a round or two, after 0.4 opens.

## C. Review after the fact (#58 batch)

- **Decision #69** (Opus, session 47): a static front page is presentation; the front
  page item's live and pinned pages may name the mount root as `rel="canonical"`, and its
  `page`, document, feed entry and index entry do not change. Built as studio 0.43.0.
  Reasoning in `v0.4-plan.md` §10.12. Check the reading of §2.8 rule 3's "live
  permalink" as the page people reach.

- **Session 34's studio fixes made four calls a reviewer may want to confirm.** None
  changes the wire or a reader. Details are in the 0.21.2 changelog. Two were taken up in
  session 38 (#29 → decision #62; #30 is consistent with §8.4); the other two stand on
  the text they cite.
  - studio#27 drops the v0.2 plan's "and the trigger set was non-empty" from the gap
    check, following §13.2's text.
  - studio#28 matches stub version agreement on origin as well as id. The old comment
    cited #26 for id-only matching.
- **Decision #65** (Opus, session 38): fork intents, §5.6 rules 7–8, ninth revision. Check
  rule 8's wording — the first explicit statement that verification does not compose across
  forks.
- **G11 promotion** (Opus, session 39): eleventh revision, §16.6f → §5.6 rule 6. Builds' calls
  to check: links are matched to the bake's anchors by href, so a target with its own `page`
  sends the fork down the whole-HTML rebuild; the parenthesis on forking a quoted fragment
  moved ahead of the list (a trailing paragraph broke blygger.org's renderer).

## Not for Fable

- Report F11, F12, F15 and F16 were **done session 38** (Opus): the spec's tenth revision
  plus back-references on decisions #9, #14, #43 and a corrected section number in #22.
  **F13 is partly done:** the two rules it named as stated only in prose gained their MUST.
  The other ~56 keyword-less sentences need Aneesh's full list (`conformance/clauses/out/
  analysis.json`, gitignored; run `clauses/analyze.py` on the PR branch) and a pass that
  separates restatements from sole statements. Opus's, once #11 is merged.
- Report F10 (Webmention reach: 8 of 19 live blygs advertise an endpoint) works as
  specified. It is a registry or adoption question.
- Merging blygger-spec#11: Aneesh made all three review changes on 2026-10-04 (98af8da). It waits only on Venkat's `intents.json` correction pass.

## Optional

- **Unicode on the wire** (blygger-spec#6), the standing question in the brief. Take it
  deliberately or when a cross-client hash disagreement appears.
