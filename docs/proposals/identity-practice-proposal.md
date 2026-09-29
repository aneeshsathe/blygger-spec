# Proposal: identity practice — a person is a URL they control

**Status:** PROPOSED — records locked decision #35 (ruled session 27,
2026-09-28, Fable + Venkat); written up session 28, 2026-09-28 (Opus 5).
**Recommended practice, never protocol.** Becomes `tn-2` once one client emits
a proof and a second client verifies it (#35's sequencing, the #21 build-then-
prose rule applied to practice).
**Scope:** what clients SHOULD put in the *existing* `author` object (spec
§5.5) and how readers SHOULD treat it. **Zero wire additions.** Locked
decision #11 — identity is never in the protocol — is untouched, and this
document exists to serve that refusal, not to work around it.

This is *a* way, clearly not *the* way. Nothing here is normative, nothing
here is required for conformance at any level, and a client that ignores all of
it is exactly as conformant as one that implements all of it.

---

## 1. The problem, and why the spec's silence is correct

The protocol's fourth invariant is that **identity is never in the protocol**:
the only authenticated entity is the publishing client at its domain, and DNS
is the namespace (spec §1). #11 carries that into the item document — `author`
is optional, per-item, origin-scoped, client-asserted and **opaque**, authors
are never addressable by any construct, and any members beyond `name`/`url` are
"the client's private authorspace grammar" (§5.5).

That refusal is right, and it is worth restating why before proposing anything
that sits next to it. A protocol that names people acquires, immediately and
permanently: a namespace authority (who arbitrates two claims to one name), a
revocation story (what happens when a key or an account is lost), a merge and
split story (when are two names one person), and a reader *obligation* to
enforce all three. Every one of those is a governance problem wearing a
technical costume, and none of them has anything to do with serving files.
#11's refusal is a refusal of those four, not squeamishness about names.

But a refusal has a consequence, and the consequence is the reason for this
document. `author` is an open object with an explicitly unspecified extension
point. An implementer who wants to show a reader "this is the same person who
writes over there" has nowhere to look, so they invent something. The project's
own TODO recorded the worry in exactly those terms: *the spec's refusal is the
right call and is well argued, but it leaves every implementer to invent
something, and three of them now exist.*

So what is missing is not a construct. It is a **convention** — and
conventions are either written down once or re-invented by everyone.

## 2. Measured first, and the measurement changed the argument

Before recommending anything, session 27 read the live population: every
manifest and at least one item document from **all 11 live blygs, published by
all 7 independent client implementations**.

**Result: every one of them emits exactly `name` and `url` in per-item
`author`. Nobody has invented an authorspace grammar. The extension point is
empty across the entire population.**

Two things follow, and the second is the strongest thing in this proposal.

**(a) The premise was false, and in the favourable direction.** The worry was
rival grammars already in the field. There are none. This document is therefore
not arbitration between incompatible inventions — it is a recommendation into
an empty field, which is the cheapest moment there will ever be to make one.
Had the measurement come back the other way, the right document would have been
a different one: a survey, and probably a decision to stay silent rather than
pick a winner among live implementations.

**(b) Seven independent implementers converged, unprompted, on a URL.** Not on
handles, not on emails, not on keypairs, not on `user@server`. That is not a
design being imposed from the spec side; it is the design already in the field,
arrived at by people who mostly never spoke to us — and it happens to be the
same shape as the protocol's own identity primitive. A recommendation that
matches what everybody already does costs nobody anything to adopt, which is
the only kind of recommendation a non-normative document can actually land.

**Method note, so this can be re-run and can be wrong.** It is a census of what
is *live*, not of what clients *can* emit: a client with a rich authorspace
grammar whose operator never filled the fields reads as `name` + `url` from
outside. The claim is deliberately about the wire, because the wire is what
readers see and what a convention has to serve. Re-run the census before
relitigating any of this; it is a few dozen fetches.

## 3. The spine: invariant 4, extended from origins to people

The whole proposal is one sentence, and it is the protocol's own identity rule
applied one level down:

> **A person is a URL they control.**

`author.url` is that URL. `author.name` is a display string and remains what it
already is: a label with no claims attached.

Why this is the right extension, rather than merely the convenient one:

- **It reuses the only namespace that already works.** DNS plus the web has a
  registry, a delegation model, a transfer process and a dispute process,
  none of which this project has to build, fund, or govern. Every alternative
  namespace would need all four.
- **It needs no new authority.** The protocol's existing accountability story
  is "somebody owns that domain." This adds nothing to it.
- **It degrades to the status quo.** An unverified `author.url` is precisely
  what all 11 live blygs emit today, and under this practice it stays exactly
  as meaningful as it is now — a claim. Nothing breaks for anyone who does
  nothing.
- **It is checkable by fetching**, which is how everything else in this
  protocol is checked: resolution fetches (§12), staleness fetches (§10.4),
  mention verification fetches (§15.4). No new verification machinery, no new
  trust root, no new cryptographic dependency for the common case.

## 4. Two proofs, and there is no third

A `url` in `author` is a claim. Two mechanisms turn a claim into something a
reader can act on. Both are existing web practice, reused rather than invented —
the same posture that took OPML for blogrolls and Webmention for notification
(#13).

### 4.1 Reciprocal link (`rel="me"`)

`author.url` names a page that links back to the publishing origin with
`rel="me"`. A reader that cares fetches `author.url`, looks for a `rel="me"`
link whose href is the item's **identity origin** as §12.2 defines it, and on a
match treats the claim as verified.

- **What it proves:** whoever controls that page agrees that this origin
  publishes on their behalf. That is a mutual-consent statement between two
  URLs, which is the strongest claim available and the only one the protocol's
  own model can express.
- **What it does not prove:** that a particular human being is behind either
  URL. Nothing proves that, here or anywhere; pretending otherwise is the
  failure mode of every identity system.
- **Cost:** one cached fetch per `(author.url, origin)` pair, bounded and
  optional. Prior art: IndieAuth and `rel="me"`, already implemented across
  the small web and already supported by the profile pages most people have.

### 4.2 Detached signature over `content_hash`

The item already carries `content_hash` — `"sha256:" + hex(SHA-256(content_md))`
per version (§5.1), computed for integrity and leaned on by pins and by
content-addressing (#2). A signature over that value, carried in a conventional
`author` member, binds a key to this exact text at this exact version.

Why `content_hash` specifically, and not a signature over the item document:

- **No JSON canonicalization.** Canonicalizing JSON for signing is the hardest
  and most bug-prone part of every signed-document scheme ever shipped, and it
  makes every future additive field a potential signature break. `content_hash`
  is already a canonical digest of exactly the bytes readers read.
- **No new item field.** The value being signed exists; only the signature is
  conventional, and it lives inside the extension point §5.5 already grants.
- **It survives the wire moving.** Additive changes elsewhere in the document
  (`cited`, `selector`, `generator_url`, anything #43 permits as a revision)
  do not invalidate a signature over the content digest. Under #21 the wire is
  explicitly unstable pre-1.0, so a proof that does not depend on document
  shape is worth a great deal.
- **What it proves:** the holder of a key signed this text. Binding that key to
  a person is the reader's problem and always was — which is exactly why section 4.1
  exists, and why key-based identity systems still end up publishing a profile
  page.

### 4.3 Why exactly two

A proof of identity is either *an authority I can fetch agrees* or *a key I can
check signed*. There is no third category, and every identity system on offer
reduces to one of them. That is the test to apply to the next one that shows up.

## 5. Where the providers go

This is the part Venkat asked for — technical recommendations rather than pure
agnosticism. The point of the section is that **nothing here competes with the
two proofs; each provider is a route to one of them.**

| Provider | Maps to | How |
|---|---|---|
| OAuth (Google, GitHub, …) | 4.1 | A way to *obtain a profile URL*, not a proof. Sign in, get `github.com/alice`, put it in `author.url`, add the reciprocal link on the profile. The OAuth flow is studio-side machinery — #11's home for exactly this. |
| Wallets / raw keypairs | 4.2 | The address is not a URL and does not become one. Its value is the signature. |
| DIDs | 4.1 or 4.2 | `did:web` *is* DNS, so it is 4.1 with extra steps. Other methods are 4.2 with a resolution step. Either way they land in `author.url` or in the signature member. |
| Fediverse actors | 4.1 | Actor URLs, and most instances already serve `rel="me"`. |
| Email | neither | `mailto:` is an address, not a page; nothing fetches it, so there is no proof to be had. Recommended against as `author.url`, and a needless harvesting target on a public page besides. |

A client MAY of course offer several; they are all studio-side, and the wire
sees one URL and possibly one signature.

## 6. Reader rules

Three, and they are the half of this proposal that protects readers rather than
publishers.

1. **Show a verified claim as verified and an unverified claim as a claim.**
   Never the same chrome for both. An unverified `author.url` is a link the
   publisher typed; presenting it as an identity is the mistake `blyg-tk-gen`
   is deliberately unstyled to avoid (#25) — do not dress a self-assertion as
   a verified fact.
2. **No cross-origin identity merge without a verified claim.** §5.5 already
   forbids treating byte-equal `author` values from different origins as the
   same entity. A verified claim is the only thing that lifts that, and it
   lifts it only between the origins that actually verify.
3. **Never gate anything on identity.** No filtering, ranking, scoring or
   trust weighting keyed on `author` — absence means only that nothing was
   stated (§3.2's posture for informative keys), and **no counts of any kind**
   (#12, #13): a verified badge is a fact about one claim, never a reputation.
   The moment identity acquires a number, the protocol has a follower count by
   another name.

Verification itself is reader-side, cached, bounded, and **optional**. A client
that performs none of it is fully conformant, because none of this is protocol.

## 7. Agent bylines

#38 assigns this convention to this document, and it belongs here because an
agent byline is an identity question, not an AI question.

An agent that writes items is a valid `author` like any other: asserted by the
origin, accountable to it, and indistinguishable to the protocol (§5.5,
invariant 3 — "the protocol cannot and does not tell"). What discloses that
prose is machine-generated is `generated` (§5.7), never the byline.

**Convention:** `name` the agent, `url` the agent's own page, and add an
`operator` URL naming the accountable human or organisation.

Three separate facts, deliberately not collapsed:

- the **byline** says who wrote it,
- **`generated`** says how it was made,
- **`operator`** says who answers for it.

Collapsing any two is what makes agent bylines read as evasive. An agent's own
URL can carry a `rel="me"` proof exactly as a person's can, and so can its
operator URL — which is the useful case, because "who answers for this bot" is
the question a reader actually has.

**What this is not: an "I am an agent" flag on the wire.** #38 refuses one, and
the reason generalises: a spammer would not set it, and an honest agent is
already known by its origin. The same argument refuses a maintenance
declaration (#34) — a flag that only the cooperative set will ever populate
tells you nothing about the set you care about.

## 8. Why this can never be protocol

Four reasons, in ascending order of importance.

1. **It would break the field it describes.** A MUST here makes all 11 live
   blygs non-conformant for a reason unrelated to publishing — the argument
   that made `generator_url` a SHOULD rather than a MUST (#34).
2. **Verification is a network operation.** Its result depends on a third
   party's uptime, redirects and TLS. A conformance clause whose outcome varies
   with somebody else's hosting is not a conformance clause, and #48's
   partition (MUST fails, SHOULD warns) has nowhere to put it.
3. **It would re-import the four governance problems** section 1 lists — authority,
   revocation, merge, and a reader obligation to enforce them.
4. **This document can be wrong at zero cost.** If the recommendation is bad,
   clients stop following it and the wire is unchanged; nothing needs
   migrating, deprecating, or apologising for. That option exists only while it
   stays out of the spec, and under #21 — where no pre-1.0 version promises
   anything and the whole point of building is to discover what was wrong —
   keeping the revisable thing revisable is the substantive choice, not the
   timid one.

## 9. Sequencing, and what would change this

Per #35: **a proposal now**, in `docs/proposals/`, the same way
`author-field-proposal.md` carried #11 before any of it was built. It becomes
`tn-2` after **one client emits a proof and a second client verifies it** —
practice earns the note by being exercised, exactly as constructs earn
normative prose under #21.

What would change the recommendation, stated in advance so the record is
honest:

- **A live client emitting a non-URL authorspace grammar.** Re-run the section 2
  census; if the field stops being empty, the right document is a survey, not a
  recommendation.
- **`rel="me"` failing in practice** under redirects, delegated hosting or
  platform profile pages that strip link relations. That is an argument about
  section 4.1's mechanics, not about the spine.
- **Real demand for cross-origin merge that a verified claim cannot carry** —
  the only case that would reopen section 6’s rule 2, and the one to watch, because it
  is where a follower graph would try to enter.

## 10. What is proposed

- **A person is a URL they control** — invariant 4 extended from origins to
  people. `author.url` is that URL; `name` stays a label.
- **Two proofs, both existing web practice:** a reciprocal `rel="me"` link
  between `author.url` and the identity origin (§12.2), and a detached
  signature over the item's existing `content_hash`, carried in a conventional
  `author` member.
- **Providers are routes to those proofs, not rivals:** OAuth yields a profile
  URL, wallets and DIDs yield signatures or are URLs already, email yields
  nothing and is recommended against.
- **Readers:** verified as verified, claims as claims; no cross-origin merge
  without a verified claim; never gate or count on identity.
- **Agents:** name, the agent's own URL, and an `operator` URL; no agent flag
  on the wire.
- **Zero wire additions, no new fields, no conformance change, #11 untouched.**
  Recommended practice, never protocol — and the spec's refusal to define
  identity remains correct. This document exists only because that refusal
  leaves implementers to invent something, and it is cheaper to converge on the
  URL they already chose than to arbitrate later among the grammars they would
  otherwise build.
