# The raise profile

The profile is the only thing sent to Pulse for matching. It is short on purpose. Extract it, mark what was inferred, show it back, and confirm before calling `match_investors`.

## Fields

| Field | Where it usually comes from | Reliability | How to handle it |
|---|---|---|---|
| `company` | Title slide | High | Exact name as written |
| `one_liner` | Problem or solution slide, one sentence | High | Write it yourself if the deck has none; mark as inferred. The tool matches the specific words in it against each firm's thesis, so "payroll for construction subcontractors" beats "fintech platform" |
| `sector` | Customer and pricing slides, not the buzzwords on the title slide | High | One to three tags from the list below |
| `business_model` | Pricing slide | High | `saas`, `marketplace`, `transactional`, `hardware`, `consumer_subscription`, `advertising`, `services`, `other` |
| `stage` | The deck's own label when it names the round; otherwise inferred from raise size plus traction | Low | Mark inferred unless the deck says it; see the table below |
| `raise_amount_usd` | Ask slide | Medium | `{ min, max }` in whole US dollars, not millions; a single number becomes min = max. Confirm the currency. For non-USD raises, confirm a dated conversion and USD amount with the founder, or leave null rather than invent an exchange rate |
| `hq_country`, `hq_region`, `hq_city` | Team or contact slide, footer, website | Medium | HQ only. Send ISO two-letter country codes (`US`), US two-letter state codes (`TX`), otherwise region names. Display readable names to the founder |
| `traction` | Traction or metrics slide | Low | Confirm for stage understanding and requested drafts only. Omit from the public matching payload |
| `competitors` | Competition slide | Medium | Names only; feeds the portfolio conflict check |
| `preferences` | Asked | n/a | Optional: `lead_only`, `exclude_corporate`, `local_only`, `exclude` (firm names) |
| `notes` | Anything that matters for fit and fits no field | n/a | Keep local context; omit from the public matching payload |

## Sector tags

Pick the closest one to three: `ai`, `b2b_saas`, `fintech`, `healthtech`, `biotech`, `climate`, `energy`, `consumer`, `marketplace`, `developer_tools`, `infrastructure`, `cybersecurity`, `hardware`, `robotics`, `deeptech`, `edtech`, `proptech`, `insurtech`, `legaltech`, `hrtech`, `logistics`, `mobility`, `media`, `gaming`, `food`, `agtech`, `space`, `defense`, `web3`, `other`.

## Inferring stage

Use this only when the deck does not name the round. A deck that says "seed" is seed, even at 5M.

| Raise amount (USD) | Traction | Stage |
|---|---|---|
| Under 1.5M | Idea, prototype, first users | `pre_seed` |
| 1.5M to 5M | Early revenue or strong usage | `seed` |
| 5M to 8M | Repeatable revenue, early team | `seed` (note "large seed") |
| 8M to 20M | Clear revenue growth, 10 to 40 people | `series_a` |
| Over 20M | Scaling | `series_b_plus` (the directory is thin here; say so) |

Treat these ranges as rough prompts for clarification, not firm definitions. When raise and traction disagree or this is a bridge round, ask the founder which round label to match.

## The confirmation message

One compact block, then one question. Example:

> Here is what I read from the deck. Stage is inferred from the raise size.
>
> Acme, "payroll for construction subcontractors". Fintech, B2B SaaS. Raising 2 to 3M, seed (inferred). HQ Austin, Texas, United States (wire location US/TX). Traction: 180k ARR, 40 customers. Competitors named: Gusto, Rippling.
>
> Anything to change before I match?

Do not ask field-by-field questions. One message, one confirmation.

## Wire payload

Send `profile` with company (200 characters max), one_liner (300 max), one to three supported sector tags, stage, and only confirmed business_model (60 max), raise_amount_usd, HQ fields and competitor names needed for matching. Keep preferences in the separate supported `filters` object, never a profile field. Unknown optional fields can be omitted or null. Do not copy deck passages, traction, personal details or arbitrary notes into the call.

Example call (live discovered schemas take precedence):

```json
{"profile":{"company":"Acme","one_liner":"Payroll for construction subcontractors","sector":["fintech","b2b_saas"],"business_model":"saas","stage":"seed","raise_amount_usd":{"min":2000000,"max":3000000},"hq_country":"US","hq_region":"TX","hq_city":"Austin"},"filters":{"lead_only":true}}
```

`lead_only` currently retains records with unknown lead roles. If the founder requests only confirmed leads, present only returned candidates whose `leadRoles` contains `LEAD` or `CO_LEAD`, disclose omitted unknowns, and do not invent replacements. `local_only` retains unknown state and defaults missing country to US: confirm HQ before calling it and describe location proximity, never investment eligibility. If the founder needs a strict known-location list, omit unverifiable locations and explain the shorter list.
