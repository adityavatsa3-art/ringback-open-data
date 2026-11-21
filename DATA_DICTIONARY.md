# Data dictionary — RingBack outreach funnel (2026-09)

All files are UTF-8 CSV with a header row. "Count" fields are integers. There
are no missing-person records: every row is an aggregate.

## prospecting_runs.csv

One row per retained run of the OpenStreetMap prospect finder.

| Column | Type | Meaning |
|---|---|---|
| `ts` | epoch ms | When the run happened (source clock). |
| `date_ist` | string | Source's local-time rendering (`YYYY-MM-DD HH:MM IST`). |
| `city` | string | One of Hyderabad, Bengaluru, Pune, Chennai, Delhi, Ahmedabad. |
| `niche` | string | Vertical slot: gym, salon, clinic, dentist, car, dental, etc. |
| `found` | count | Businesses the Overpass query returned. |
| `verified` | count | Candidates that passed verification. |
| `queued` | count | Candidates added to the founder's review queue. |
| `skipped` | count | Candidates not queued (duplicates, low score, etc.). |
| `dry_run` | boolean | `True` = the run did not commit its queue writes. |
| `zero_queue_streak` | count | Consecutive runs that queued nobody (a stall signal). |
| `notes` | string | Free-text operator/API notes, `\|`-joined; e.g. `overpass 504`. |

## outreach_experiments.csv

One row per readout snapshot. Five snapshots of the **same** 8-message batch —
these are not 40 independent observations.

| Column | Type | Meaning |
|---|---|---|
| `ts` | epoch ms | Snapshot time (source clock). |
| `datetime_utc` | ISO 8601 | `ts` converted to UTC for convenience. |
| `dental_short_sent` / `_replied` / `_rate` | count / count / ratio | Dental cohort, SHORT message arm. |
| `dental_long_sent` / `_replied` / `_rate` | count / count / ratio | Dental cohort, LONG message arm. |
| `gym_short_sent` / `_replied` / `_rate` | count / count / ratio | Gym cohort, SHORT message arm. |
| `gym_long_sent` / `_replied` / `_rate` | count / count / ratio | Gym cohort, LONG message arm. |
| `dental_rate` | ratio | Combined dental reply rate. All rows are 0. |
| `gym_rate` | ratio | Combined gym reply rate. All rows are 0. |
| `h2_ratio`, `h4_ratio` | — | Empty in the source: a planned hypothesis-2/4 ratio that was never computed. Left empty, not filled with zero. |
| `batch_sent` | count | Total messages sent by this snapshot (8 in every row). |
| `batch_replied` | count | Total replies by this snapshot (0 in every row). |

## funnel_stages.csv

| Column | Type | Meaning |
|---|---|---|
| `key` | string | Stage id: `queued`, `sent`, `replied`, `demo`, `paid`. |
| `label` | string | Human label. "Pitched" is the label for `sent`. |
| `reached` | count | Leads that reached the stage. |
| `pct` | number | Percent of all leads (`reached / 161 * 100`). |
| `step_pct` | number | Percent of the previous stage. |

## funnel_totals.csv

Two columns: `metric`, `value`. Metrics: `leads`, `replies`, `demos`, `paid`,
`replyRate`, `demoRate`, `closeRate`, `pitched`, `pitchReplyRate`, and
`outcome_replied`, `outcome_ghosted`, `outcome_bounced`, `outcome_unknown`
(the last four are outcome classifications; all 8 pitched leads are `unknown`).

## reply_by_segment.csv

| Column | Type | Meaning |
|---|---|---|
| `city` | string | City of the business. |
| `niche` | string | Vertical label as recorded by the operator. |
| `pitched` | count | Messages sent in this segment. Sum = 8. |
| `replied` | count | Replies. All 0. |
| `ghosted` | count | Marked as no-response. All 0. |
| `bounced` | count | Delivery failures. All 0. |
| `reply_rate` | ratio | `replied / pitched`. All 0. |
| `dead_rate` | ratio | `(ghosted + bounced) / pitched`. All 0. |

## Reading the zeros correctly

`replied=0` means "no reply was observed by the time of the snapshot". It does
not mean "the message was never delivered", and `ghosted=0` means the operator
never made that classification — it is not an assertion that nobody ignored the
message. `outcome_unknown=8` is the honest summary of those eight leads.
