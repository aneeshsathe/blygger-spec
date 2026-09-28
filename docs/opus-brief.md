# Opus brief — the standing queue for an implementation session

**Written:** session 28 (2026-09-28), by Fable 5.1, for an Opus session running **in
parallel** with the Fable round that froze 0.3 and defined 0.4.
**Rewrite this file when the queue changes materially.** It is where an Opus or Sonnet
session starts (`CLAUDE.md` → At Session Start, step 4).

> **Read first:** this brief → `CLAUDE.md` locked decisions **#30–#50** (dense; the
> reasoning is in `v0.3-plan.md` §8c and `v0.4-plan.md`) → the latest `DEVLOG.md`
> entries → [`blygger-studio/CLAUDE.md`](../../blygger-studio/CLAUDE.md) backlog, which
> carries every build task below with its fixed shape.

## Running beside a Fable session — file ownership

Two sessions are editing this program at once. The partition that worked in session 27:

- **Opus owns** `blygger-studio/`, `blygger-com/`, `blygger-org/` — code, their
  `CLAUDE.md`s, `CHANGELOG.md`, releases, deploys.
- **Fable owns** `blygger-spec/` this session: `docs/protocol-v0.3.md`,
  `docs/v0.4-plan.md`, `docs/fable-brief.md`, `CLAUDE.md`'s locked decisions and doc
  map. **Do not edit those.**
- **Opus writes into `blygger-spec/` exactly two things**, pulling first each time: its
  own `DEVLOG.md` entry, titled `## Session N (parallel, Opus) — …` and placed above the
  Fable entry of the same session; and ticks on its own items in `CLAUDE.md`'s TODO and
  carry-over lists. Nothing else.
- If a build finds the spec wrong or silent, **do not improvise**: record it in your
  devlog entry's open threads and, if it blocks, stop and tell Venkat — the Fable
  session is live and can rule the same afternoon.
- Deploys authenticate with `wrangler login`, not the registry tokens
  (`docs/deploy-protocol.md` § Authentication); unset `CLOUDFLARE_API_TOKEN` first.
  `deploy:all` is blocked in auto mode — use per-target wrangler commands and verify
  each node by hand. Migrations: apply by hand before a dev-server smoke test.

## The queue, in order

Everything here is unblocked. Semantics are fixed by the decisions cited; implement,
do not redesign. Priorities follow `roadmap-tracks.md`.

1. **The `[[` picker** (carry-over; Venkat hit it). The composer palette lives only in
   `threadEditPage` and triggers only on `![[` at line start, while `[[id]]` is legal
   inline and in fragments. Factor it into all three composers (index composer, fragment
   editor, thread editor); the trigger distinguishes `![[` from `[[` and the insertion
   matches. Studio only. **Reader end, ruled #50 (2026-09-28):** an affordance on a
   reading-feed entry that copies or inserts `[[id]]` is allowed and does not collide
   with #27 — a link is not a response — provided it is named for what it does ("copy
   `[[id]]`", "insert link"), sits beside copy-permalink, and is never a peer of
   `stub ↗` or called respond/reply/answer.
2. **2.9 — `/api` becomes a contract** (#31, HIGH): versioning, per-client bearer tokens
   with coarse verb scopes, owner-minted and revoked, revoke-all, CORS for token
   requests, `rel`-link discovery on the studio page; the owner password becomes a root
   credential no tool holds; password reset after tokens, and it MUST offer revoke-all.
   Include a **read scope** for studio-private material (#39). This unblocks gate G3
   (tn-4), the reference agent, and every third-party tool now holding a master password.
3. **Bulk re-pin against the direct freshness check** (#33, the second half of migration
   0011's fact): for imported items, fetch `{origin}items/{id}.json`, compare `version`
   to each reference's; a studio surface to refresh a thread's stale snapshots as one
   authoring act (a refresh is a republish — version bump, feed entry, mentions).
4. **`[TK]impyrt=…[/TK]` in the composer** (#37): output is the pasted text verbatim,
   wrapped `blyg-tk-gen`, `generated[]` entry with `sources: []`, `model`/`at` only if
   supplied. Small.
5. **2.12 — generated changelog notes + history view** (#40): note from the local diff
   when the author leaves it blank, **pin-bounded** (full between pins, descriptive over
   unpinned), editable before publish; emit `changelog[].generated: true`. That emission
   is what lets Fable promote spec §16.6c into §5.2 — record in your devlog when both
   nodes have exercised it (gate G6).
6. **Remote generation sources** (#44, the first 0.4 construct — full shape in the studio
   backlog and spec §16.3; **implementation plan: `v0.4-plan.md` §7.2**, tasks R1–R8 with
   acceptance checks). Buildable now; the shape will not change and nothing else is
   sequenced with it. Exercise it across both live nodes: that exercise is what opens
   the 0.4 document.
7. **Partial transclusion** (#49, spec §16.4; a 0.3 revision once built — full shape in
   the studio backlog; **implementation plan: `v0.4-plan.md` §7.3**, tasks P1–P9). Do this
   one *before* item 6 — §7.4 says why. Directive plus attached blockquote in the grammar; substring
   check against the snapshot's text content at publish; `selector` on the
   `transclusions[]` entry; `blyg-partial` beside `blyg-transclusion` in the bake;
   select-to-quote in the reading view; the stub action prefills it for long targets.
   Record in your devlog when both nodes have exercised it — that is what lets Fable
   promote it into §10.
8. **2.13 — discovery surfaces from references** (#41): chain view first.
9. **Technical notes** (tracks 1.6): `tn-3` groups and aggregation is writable now
   (#36); `tn-2` identity practice starts as `docs/proposals/identity-practice-proposal.md`
   (#35). Both are `blygger-spec/docs/` files — the one ownership exception, since they
   are Opus-written by decision; tell the Fable session when you open one.
10. **2.3 / 2.4 — update path and fork-friendliness**, then **3.1 / 3.2** — version
   surfacing and the health cron. The three exposed third-party nodes are still on
   pre-0.4.1 code; the notice went out, the `npm run upgrade` path does not exist.
11. **Phase B remainder** (`v0.3-plan.md` tasks 12–16): share, own items in hoppers,
    stub templates, threads tab, `stub_of` at import.
12. **The conformance runner** (#48): `blygger-spec/conformance/` — a publisher suite
    that takes an origin and a reader suite of fixtures. New directory, Opus-owned.

## Hand back

Studio backlog ticks with what was built and what it found; `CHANGELOG.md` entries
stating `Migrations:`; tagged releases; both nodes deployed and verified; your
`(parallel, Opus)` devlog entry; and anything a build found that the spec should say,
as an open thread — not as a spec edit. Ideas you have that are neither ruled nor scheduled
go in that open-threads list too; the next Fable pass files them in `docs/backlog.md`.
