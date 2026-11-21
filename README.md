# RingBack outreach funnel and message experiment readout (September 2026)

Published by [RingBack](https://ringback-4sb.pages.dev), a missed-call follow-up
service for Indian service businesses. A PII-free, aggregate, **negative-result**
dataset: 8 messages pitched, 0 replies. It is a mirror of the CC0-1.0 dataset at
[Kaggle](https://www.kaggle.com/datasets/adityavatsa3/ringback-outreach-funnel-2026).

# RingBack outreach funnel and message experiment readout (September 2026)

**An honest, privacy-safe record of a very small cold-outreach experiment run by a
one-person bootstrapped SaaS in India. It is a negative-result dataset: nothing
converted. It is published for transparency and reproducibility, not as evidence
that any message works.**

Read that again before using the numbers: **8 messages were pitched, 0 replies,
0 demos, 0 paid customers.** With n=8 the data has essentially no statistical
power. Any "effect" you compute from it is noise. The useful contribution is the
*shape* of a real early-stage funnel and the exact counts, with no success story
invented around them.

## What this is

RingBack is a missed-call auto-reply service for Indian service businesses
(salons, gyms, clinics, car detailers). Between 2026-09-17 and 2026-09-21 the
founder ran an automated prospecting loop and a small message-length experiment:

- `prospecting_runs.csv` — 12 retained runs of an OpenStreetMap-based prospect
  finder across 6 Indian cities and several verticals. Aggregate counts only:
  found, verified, queued, skipped, dry-run flag, and any error notes.
- `outreach_experiments.csv` — 5 readout rows. Each row is one snapshot after a
  batch of 8 founder-sent WhatsApp messages split across two cohorts (dental,
  gym) and two message arms (SHORT, LONG). Every cell is a count, not a person.
- `funnel_stages.csv` / `funnel_totals.csv` — funnel counts at a point in time:
  161 leads queued, 8 pitched, 0 replies, 0 demos, 0 paid.
- `reply_by_segment.csv` — 6 city×niche segments, all with `pitched` counts and
  `replied=0`.

## Privacy and provenance

- **No personally identifying information is present.** The source records
  (`research/data/batch*_assignment.json`, `batch*_sendlist.txt`,
  `docs/outreach-batch-*.csv`) contain business names, phone numbers and WhatsApp
  deep links. They are deliberately **not** in this dataset. Only aggregate
  counts, cities, vertical labels, and timestamps were exported.
- A regex leak check for phone-like digit strings, e-mail addresses, and every
  business name in the source assignment files ran over every CSV here and
  reported **CLEAN** before publication. Epoch-millisecond timestamps were
  masked for that check (a 13-digit number is not a phone number).
- Source of truth: `research/data/outcome_readout.csv` and the `state`, `funnel`,
  and `replyStats` objects in `research/data/_probe.json` in the
  `adityavatsa3-art/ringback-site` repository, as of 2026-09-21. These files are
  in a gitignored `research/` directory; this dataset is the PII-free public
  projection of them.
- Collection method: the prospecting loop queried the Overpass API
  (`overpass-api.de`) for businesses by city and vertical; a founder reviewed and
  queued candidates; a proportion of queued leads were messaged by hand from the
  founder's own WhatsApp, one message per business, with no automation of the
  send itself. The experiment's assignment protocol was pre-registered in
  `research/to_human/batch1_outcome_review.md` (score-matched pairs). That file
  is not published here because it is part of the private research record.

## Known limitations (stated, not hidden)

1. **n=8 pitched, 0 replies.** There is no measurable arm difference. `h2_ratio`
   and `h4_ratio` are empty in the source readout and stay empty here.
2. **Not a randomised controlled trial.** Assignment was score-matched pairs,
   not random; the operator was the founder; the cohorts are small and uneven.
3. **Snapshot, not a longitudinal panel.** `outcome_readout.csv` has 5 timestamped
   snapshots of the same 8-message batch, not 40 independent sends.
4. **The funnel is a point-in-time snapshot.** "161 leads" is the count of
   businesses discovered and queued by the scraper, not a count of interested
   buyers. "Queued" is an internal pipeline state.
5. **History is capped.** The probe retains only the most recent 12 runs even
   though the counter reports 74 total; only retained rows are published.
6. **Timestamps are epoch milliseconds** in the source. A UTC ISO string is
   included alongside for `outreach_experiments.csv`; `date_ist` strings in
   `prospecting_runs.csv` are copied verbatim from the source.
7. **No customer outcomes.** No revenue, retention, or satisfaction data exists
   in this dataset. Do not infer any.

## Files

| File | Rows | Grain |
|---|---|---|
| `prospecting_runs.csv` | 12 | one prospecting run |
| `outreach_experiments.csv` | 5 | one snapshot of the 8-message batch |
| `funnel_stages.csv` | 5 | one funnel stage |
| `funnel_totals.csv` | 13 | one summary metric |
| `reply_by_segment.csv` | 6 | one city × niche segment |
| `DATA_DICTIONARY.md` | — | every column explained |
| `LICENSE` | — | CC0 1.0 |

## Suggested (modest) uses

- Teaching: a worked example of a real, tiny B2B funnel with honest zeros.
- Demonstrating that an early-stage outreach pipeline can look busy (587
  businesses found, 142 verified) without producing a single reply.
- Comparing prospecting yield across Indian cities/verticals as *descriptive*
  counts — with the small-n caveat always attached.

## Citation

If you use this dataset, cite it as:

> RingBack outreach funnel and message experiment readout (September 2026).
> adityavatsa3-art/ringback-site, 2026. CC0-1.0.

And please repeat the limitation: 8 messages, 0 replies, no significant result.
