# How matching works

Matching runs on the Pulse server, not in the conversation. This reference explains the current method. Scoring is deterministic: the same profile, filters, index snapshot and scoring date return the same list, and the result carries a `method` line naming the rubric version it ran.

## Data

The Pulse investor directory is built from SEC Form D filings, Form ADV records and investors' own websites (thesis, stages, portfolio, how to pitch). These sources do not cover every investor or investment. Nothing comes from LinkedIn, data brokers or scraped contact lists. Matching candidates return no partner recommendation or email address. Full `get_investor` records may contain filing names with legal roles (director, promoter); never expose these as outreach targets. Sources may have missing dates; label those explicitly.

## Hard filters

A firm is excluded, and counted in "left out and why", when:

1. Its stated stages do not include the founder's stage. A firm whose stage text names no round is kept and flagged instead.
2. Its smallest stated cheque is larger than the whole round. A missing cheque size never excludes.
3. It holds a direct portfolio conflict with a competitor the founder named, on its portfolio page or a board-seat filing.
4. It is dormant: the latest of its Form D, Form ADV and announced fund is older than 24 months. A firm with no date of any kind is kept and flagged.
5. The founder's own filters: lead only, no corporates, local only, investor types, named exclusions.

Two rules in the published method cannot fire yet because no record carries the field: a stated geography restriction, and "paused or not investing". The result says so in its coverage note for a founder outside the US.

## Soft score, maximum 95 points

| Signal | Weight | Source |
|---|---|---|
| Sector and thesis fit | 30 | The firm's industry tags (18) and the words of its thesis (12). Lexical in this version |
| Stage and cheque-size fit | 20 | Named stage 14 (unknown 6), plus stated cheque fit 6 (unknown 2) |
| Recent activity | 20 | Latest Form D, Form ADV update or announced fund: 20 within 6 months, 16 through 12, 10 through 24, zero beyond; no dated activity scores 5 |
| Leads rounds | 10 | The firm's own site; co-lead scores 7, follow 3, unknown 4 |
| People on file | 5 | Presence only: 5 when filings name anyone at the firm, else 0. Never a reason, never a name |
| Geography proximity | 10 | Directory location: same state 10, same US region 6, same country 2 |

## Groups

- Likely leads: up to eight firms among the shortlist that say they lead or co-lead
- Stretch picks: up to four of the rest with a fund of 500M or more, or 2B in assets under management
- Strong followers and co-investors: everything else

About 20 firms by default, selected before grouping. Sorting is descending score, then descending disclosed scale (`fundSizeUsd`, else `grossAssetsUsd`, else zero), then ascending slug. Stretch grouping applies before Series B, not to `series_b_plus`. The weights total 95; code clamps at 100 but does not normalize to 100. Some server descriptions still say 0 to 100; do not invent normalization. Preserve returned method/version and flag that discrepancy when explaining scores.

## What the assistant may and may not do with the result

- May rephrase a reason, keeping the fact and the source
- May reorder within a group when the founder asks
- May not add a firm the tool did not return, name a person, or state a thesis the tool did not cite
- May not show any contact that is not a firm-level published route (a form URL, or "email" with the firm's site)
