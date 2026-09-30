# Exploration — inline fragment authoring with `[[text]]`, and named anchors for ids

**Status:** DRAFT, exploratory. Written session 29 (2026-09-29) by a research-only Opus
session. **Nothing here is ruled.** Companion: [`publishing-patterns.md`](publishing-patterns.md)
— §4 here is the authoring move that makes several of those patterns cheap.

**Brief as given (Venkat):** inline fragment authoring with a `[[text]]` operator that
*"simultaneously produces a thread and component fragments, and publishes them all
together"*, and *"the possibility of having meaningfully named anchors for ids."*

Two asks. They look like one feature and are three questions, only one of which is open.

---

## 1. What is being asked for

**A. Tangling.** Write one document. Mark spans of it. On publish, each marked span becomes
its own fragment with its own id, address and feed presence, and the document you wrote
becomes a thread that transcludes them. You wrote an essay; you got a commonplace book for
free.

Today the workflow runs the other way: write fragments, then assemble a thread that quotes
them. That is the right model for a curator and the wrong one for an essayist, who
discovers the parts by writing the whole.

**B. Names.** `7c9wk2mhq0v3xj8tn5rzfd41bg` is not a thing a person can hold in their head,
type, or recognize in their own source.

---

## 2. The syntactic space is free

Verified against the live grammar (`src/transclusion.ts`, this session):

```
[[Chapter One]]                   → inert
[[stigmergy is the mechanism]]    → inert
[[7c9wk2mhq0v3xj8tn5rzfd41bg]]    → a link (§10.1)
[[abcdefghjkmnpqrstvwxyz0123]]    → a link — 26 chars, all in the alphabet
```

Both forms require **exactly** 26 characters of the Crockford alphabet
(`DIRECTIVE_LINE`, `LINK_INLINE`), and §10.1 says the sequence is inert text everywhere
else. So `[[text]]` is available.

One caveat worth recording: a 26-character span of lowercase alphanumerics with no
`i`/`l`/`o`/`u` is indistinguishable from an id. Prose will not hit this; a base32 hash
pasted into a sentence might. It is already true today and the new operator would inherit
it.

---

## 3. The crux: what does published `content_md` contain?

Everything below turns on one question, and both halves of the brief hit it.

`content_md` **is published** (§5), **is hashed** (§5.1), and §10.2 calls it *"the authoring
source of truth."* So any authoring convenience written into it is either on the wire for
every other client to understand, or it is rewritten before publish.

### Option A — the construct reaches the wire

`content_md` keeps `[[Some text]]`, and readers must know what it means.

**Against, decisively.** It makes an authoring convenience into permanent protocol surface
that seven client implementations must implement. It breaks §5.7's invariant that *"no
markers, no instructions, no authoring grammar of any kind reaches the wire."* And it would
be the first construct requiring a reader to *do* something with `content_md` rather than
display `content_html`. This is not a close call.

### Option B — the studio rewrites at publish

The author writes `[[Some text]]`; the publish act mints a fragment, publishes it, and
rewrites the thread's `content_md` to `![[newid]]`. **The wire sees ordinary 0.3.**

**This is the only option that costs the protocol nothing**, and it is also the one that
makes the author's document visibly change shape when they hit publish.

Which connects to the open question in
[`rich-editor-proposal.md`](rich-editor-proposal.md) §7 Q3 — *may a saved document differ
byte-for-byte from what was opened?* There I argued that invisible per-open rewriting is
intolerable and an **explicit, visible, one-time act** is not. Tangling is exactly that
shape: the author pressed publish, the transformation is the feature, and it can be shown
before it happens. If Option B is taken here, that is the precedent it sets, and it should
be taken knowingly.

### Option C — a separate draft source

The studio keeps the author's tangled source in D1 and derives `content_md` from it.
`content_md` is then a *build artifact*, not the author's text.

**Against.** §10.2's "authoring source of truth" stops being true; third-party tools writing
to `/api` have no way to participate; and the author's real document now exists only inside
one client's database, which is the opposite of the project's stated shape. It also
resurrects a two-source model — one editable representation, one derived — and positional
drift between paired sources is a failure this project has already been bitten by once.

### Recommendation

**Option B.** It is the only one that leaves the protocol untouched, and the transformation
it performs is honest, visible and author-triggered.

---

## 4. Tangling — the design

### 4.1 What happens

Source, before publish:

```markdown
Here is the opening, which stays in the thread.

[[Stigmergy is the mechanism by which a protocol becomes invisible
to the people inside it. That invisibility is not a failure of
attention; it is the mechanism working.]]

And here is my commentary on that idea.

[[A second standalone claim, worth its own address.]]
```

After publish, `content_md` on the wire:

```markdown
Here is the opening, which stays in the thread.

![[3k7m2p9wqx4vb8ntr5zhdf16cg]]

And here is my commentary on that idea.

![[9wq4vb8ntr5zhdf16cg3k7m2p]]
```

Plus two new fragments, each published, each with its own feed entry, page and id. The
thread's `transclusions[]` names both. **Every byte of this is ordinary 0.3.**

### 4.2 The design questions, in order of how much they bite

**a. The feed flood.** §7 mandates one `<item>` per publish event, and the window is
RECOMMENDED at 50. A twelve-span essay consumes thirteen slots and buries everything a
subscriber saw that week under one publication.

This is the hardest problem in the feature, and I do not think it has a clean answer.

- *Accept it.* Each fragment genuinely is a publication with its own address. The feed is
  lossy by design and the archive index is the lossless surface. Defensible, and it will
  still feel like spam to a subscriber.
- *A quiet publish* — publish without a feed event — is a new protocol construct, and it
  hides content from subscribers, which is exactly what the feed exists to prevent. I
  expect this to be refused and I think it should be.
- *Stagger the publishes* over minutes or hours. Cute; a lie about when things were
  written.
- *Cap the spans per publish* in the client. Crude, honest, no protocol involvement.

**My reading:** this may be the feature's real constraint rather than a wrinkle. Tangling
is cheap for an essay with three marked spans and antisocial for one with thirty. A client
cap plus a visible count ("this will publish 13 items") is the minimum.

**b. Atomicity.** Fragments must exist and be published before the thread can resolve them
(§10.2 resolves only to published local items). So the sequence is N publishes, then one.
If the thread's publish fails — an unrelated unresolvable directive, a validation error —
the fragments are already public and the author is holding orphans.

Needs a recovery story: either pre-flight the whole thread's resolution before minting
anything, or a visible "these N fragments were published, the thread was not" state with a
retry. Pre-flighting is better and is mostly already possible, since the studio knows every
other directive's resolvability before it starts.

**c. Re-publishing.** The second publish sees `![[id]]`, not `[[text]]` — the operator is
gone from the source. So:

- Editing the quoted text now means **opening the fragment**, not editing the thread. That
  is correct (one source of truth per item) and it will surprise people.
- Adding a *new* `[[text]]` span to an already-published thread tangles just that one. Fine.
- Deleting a `![[id]]` line leaves a published orphan fragment that is no longer part of any
  thread. Also fine — it is a real published item — but the client should say so rather than
  let it happen silently.

**d. What the fragment inherits.** Its own `author` (the origin's), its own `page`, `kind:
"fragment"`, no `stub_of`, no `forked_from`. Straightforward. §5.3's 2,000-character
publisher cap (studio-RECOMMENDED 1,000) applies — so a marked span longer than that should
warn, and the natural answer is that it wants to be a thread, not a fragment.

**e. Nesting.** `[[text containing ![[id]]]]` — a marked span that itself transcludes. The
grammar would need a rule; the simple one is *no nesting, reject at publish*, which costs
nothing and can be relaxed later.

**f. Partial transclusion interaction.** Under #49, a directive followed by an attached
blockquote is a partial transclusion. A freshly tangled `![[id]]` has no attached
blockquote, so there is no collision — but the studio's rewrite must not accidentally
attach the author's own following blockquote to the new directive. A blank line after the
inserted directive is mandatory, not cosmetic.

### 4.3 Why this is worth doing

It makes the [`publishing-patterns.md`](publishing-patterns.md) §3.9 commonplace book a
*byproduct of writing essays* rather than a separate discipline. Today an author must
choose between composing atoms and composing wholes; tangling means the atoms fall out of
the whole. For a medium whose thesis is that fragments are the unit, that is close to the
central authoring move.

### 4.4 Precedent

**Literate programming.** Knuth's WEB: one document, `tangle` extracts the machine artifact
and `weave` extracts the human one. `noweb` and org-mode's `<<chunk>>` references are the
same idea. This proposal is **tangle for a blyg** — and the precedent also supplies the
warning, since literate programming's adoption problem was always that the tangled output
is what everything downstream actually consumes, and authors lose track of the mapping.
Here the mapping is visible (the thread shows its directives), which helps.

Closer relatives: **Roam and Obsidian block references**, where every block is addressable
and can be embedded elsewhere; **Ted Nelson's transclusion**, of which this is the
authoring half that Xanadu never shipped.

---

## 5. Named anchors — three questions, not one

This is where the brief splits, and the split matters because two of the three are already
answered.

### 5.1 Naming the permalink — **already available, already conformant, not built**

§5.8 makes `page` the **origin-relative URL of the item's human-readable permalink**, and
readers SHOULD use it. It is free-form. The reference client emits `f/{id}/` and `t/{id}/`
as a *convention* that §4 explicitly says is not a requirement.

So this is legal today, with no protocol change whatsoever:

```json
{ "id": "7c9wk2mhq0v3xj8tn5rzfd41bg",
  "page": "essays/stigmergy-is-invisible/" }
```

The item document stays at `items/{id}.json` — protocol-fixed, §4 — and **that is the
point**: the machine address is stable and opaque, the human address is chosen and
meaningful, and §5.8 already tells readers which to link to.

**This is most of what "meaningfully named anchors" wants, and it needs nothing from the
spec.** What it needs from the client:

- a slug field, derived from a leading heading or typed by hand;
- uniqueness enforcement across the origin;
- a policy for changing one. A slug is a promise the moment someone links to it. The
  honest options are *frozen after first publish* (like hoppers' `slug_frozen`, which the
  importer already does) or *changeable with a redirect from the old path*. The hopper
  precedent is in the codebase and suggests the answer.

**Open question the spec should probably settle:** may `page` change across versions? §5.8
does not say. If a reader caches `page` from an older version and the publisher moves it,
links rot silently. One sentence would fix this — either "`page` SHOULD be stable for the
life of the item" or "readers MUST re-read it per version."

### 5.2 Naming the id itself — **refused, and the refusal is right**

Decision #2 and §5.1: 128 random bits, 26 characters, permanent, never content-addressed.

The arguments against human-readable ids are not aesthetic:

- **Collision.** Ids are scoped to origins (§13.1) but generated with enough entropy that
  cross-origin collision is not a real case. `stigmergy` is a collision on day one.
- **Renaming is identity change.** A wiki solves this with redirects *because it controls
  every page that links in*. **A decentralized medium cannot rewrite other people's
  copies** — a baked transclusion in a stranger's thread, a pinned version, an importer's
  local row. A meaningful id is a link that breaks when the author changes their mind about
  the title.
- **Squatting and meaning drift.** Good names are scarce; an id must be free and
  meaningless.
- **The id appears in `items/{id}.json`,** which §4 fixes. A human id would put the author's
  title into a protocol-mandated file path.

The precedent is emphatic and one-directional: **DOI, ARK, PURL, ISBN, UUID** are all
deliberately opaque identifiers with human-readable metadata layered on top. The systems
that made the name the identifier — MediaWiki page titles, early URL-as-title blogs — all
grew redirect machinery to survive it, and that machinery only works inside one
administrative boundary.

**The protocol already made the right call. What was missing is the layer on top, and
that layer is `page`.**

### 5.3 Naming an authoring alias — **the one open question**

Write `[[on-stigmergy]]` in your draft and have it resolve to an id.

This lands straight back on §3. Under Option B the alias is a **draft-time** convenience,
rewritten to `![[id]]` at publish, and the wire never sees it — which makes it studio-private
and Opus-safe. Under Option A it is a wire construct and every client must resolve
it against a namespace the protocol would then have to define. Option A is not worth it.

The natural shape, if Option B is taken for tangling anyway: **the same rewrite pass
handles both.** `[[text]]` mints a new fragment; `[[alias]]` resolves an existing one. The
palette already does alias-like lookup interactively; this is the same thing for people who
would rather type than pick.

Where it gets sharp: the alias must be unambiguous at publish time, and an alias that
resolves to two items is a publish error by exactly the logic §10.2 uses for ambiguous
imported ids — *"the publisher does not pick an origin on the author's behalf."* Same
principle, same refusal to guess.

---

## 6. Recommendation

1. **`page` slugs first** (§5.1). Highest value, no protocol change, no new grammar, and it
   is the thing that actually makes a blyg's URLs human. Build the slug field, uniqueness,
   and a freeze-or-redirect policy.
2. **Ask the spec one question about `page` stability** (§5.1, end). One sentence.
3. **Tangling second** (§4), under Option B, with the feed-flood cap and the pre-flight
   atomicity fix designed in from the start rather than retrofitted.
4. **Aliases third** (§5.3), as a small extension of the tangling rewrite pass, or not at
   all — the palette may already cover the need.
5. **Named ids: no.** Record the reasoning in the backlog so it is not re-proposed; §5.2 is
   the entry.

Nothing in 1, 3 or 4 touches the wire. Item 2 is a clarification, not a construct.

---

## 7. Open questions

1. **Option B — is an explicit, visible, publish-time rewrite of the author's source
   acceptable?** Everything here depends on it, and it is the same question the editor
   proposal asks from the other side. One ruling settles both.
2. **The feed flood** (§4.2a). Is a thirteen-entry publish acceptable, or is this feature
   capped at a handful of spans? I lean toward a client cap and a visible count, and I do
   not think a "quiet publish" should exist.
3. **May `page` change across versions?** (§5.1) ⚠️ FABLE — small, and links rot silently
   without it.
4. **Slug policy: frozen at first publish, or changeable with a redirect?** The hopper
   precedent (`slug_frozen`) is already in the codebase.
5. **Is tangling's rewrite reversible?** An "untangle" — pull a fragment's text back inline
   and withdraw it — is symmetrical and sounds useful, but withdrawal leaves a permanent
   tombstone (§9), so untangling is not the inverse of tangling. Probably refuse it, and
   say why.
6. **Does the 26-character prose collision** (§2) deserve an escape hatch? It is already
   possible today and nobody has hit it.
