# pop-tape-anchors

Daily fingerprints of the [popularity.dev](https://popularity.dev) price tape,
committed here so that **rewriting price history is publicly detectable —
even by us**.

## How it works

Every market day runs 06:00→06:00 UTC. Shortly after each day completes, the
scheduled workflow in this repository fetches

```
GET https://popularity.dev/api/public/tape-digest?day=YYYY-MM-DD
```

and commits the response to `anchors/YYYY-MM-DD.json`. The digest is a sha256
over every price print of that day in a canonical order
(`person_id|captured_at_utc_ms|score` lines, sorted by person then time,
joined with `\n`).

The whole pipeline is public: no secrets, no privileged access — this repo's
workflow calls the same endpoint anyone can call. If a published digest ever
stops matching a recomputation from the public chart data, history was
tampered with. The git history of this repository timestamps every anchor.

## Verify an anchor

1. Pick a day file from `anchors/`.
2. Fetch the same day from the public endpoint and compare digests.
3. Deeper: rebuild the canonical lines yourself from the public chart-history
   API and hash them — the algorithm is stated in every anchor file and in
   `GET https://popularity.dev/api/public/methodology`.

## Related

- [pop-engine](https://github.com/milosraymondrosas-cyber/pop-engine) — the
  published price function itself.
- `GET https://popularity.dev/api/public/methodology` — formula, movement
  caps, covenant, engine change log.
