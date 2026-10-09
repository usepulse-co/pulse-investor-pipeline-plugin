# Presenting the list

The list is read on a phone as often as on a laptop. Short entries, consistent order, nothing the founder has to decode.

## Shape

```
Profile: Acme, fintech B2B SaaS, seed, raising 2 to 3M, Austin. (Change anything? Just say.)
The directory is built from US SEC filings and investors' own sites; 183 of 1,589 firms are outside the US.
Left out 217 firms: 189 do not invest at seed, 18 show no activity in two years, 10 back a competitor you named (Gusto, backed by Firm Z and others). Ask to see any group.

Likely leads (firms that say they lead rounds)

1. Firm A, Washington, DC
   Backs fintech and AI-native software, their words (site, read Sep 2026). Pre-seed and seed (site). Form ADV updated Mar 2026.
   Reach them: email. The address is on their site: <website>

2. Firm B, Office withheld
   B2B software, sector agnostic within that (site, read Sep 2026). Pre-seed through Series A, mostly seed. Form ADV updated Mar 2026.
   Reach them: application form: <programme URL>

3. Firm C, Baltimore, MD
   SaaS, healthtech and fintech (site, read Sep 2026). Form D filed May 2026; largest fund 1.6M.
   Conflict: backs Gusto (their portfolio page).
   Reach them: no published route found; check their site: <website>
   (stage read as "early stage companies", no round named)

Strong followers and co-investors (firms that follow, or do not say)
...

Stretch picks (large funds, 500M and up; lower odds, worth one email)
...

Next: I can draft the outreach one investor at a time, or refine this list. Which?
```

## Rules of the layout

- One blank line between entries. No tables for the list itself; tables break on phones.
- Firm and location only. Matching candidates have no partner recommendation; full records may contain legal-role people whose names this skill does not display. Do not add a partner, a founder or an "attention" line from memory.
- "Office withheld" is printed as the tool gives it. It means the firm's SEC filing marks its office as a private residence, so the directory shows no city. Say that once, the first time it appears, in a short parenthesis.
- Two or three reasons per firm, never more, strongest first. The tool often returns four: a sector line, a "thesis mentions" line, an activity line and a stage line. Fold the first two into one sentence, keep the stage, and give the activity as a date.
- Dates in the form "Mar 2026". For a filing, that is the filing date. For the firm's site, say "read Sep 2026", because the fact is what their page said on that day.
- Source names in plain words: "Form D", "Form ADV", "their site". Link the firm's directory page on its name and each reason to its returned source URL. A directory link alone is not a citation for a site claim. If a source date is absent, label "source date unavailable" instead of supplying a date.
- Fund size rounded: "120M", not "$118,400,000". Only when the tool has it. No "Fund III" style names; the tool does not return them.
- Do not print the score. The scores in a list sit within a few points of each other and a number next to each firm implies a ranking the data does not support. Give it if asked.
- The reach line uses exactly one of:
  - "application form: <programme URL>" when the route is FORM
  - "email. The address is on their site: <website>" when the route is EMAIL. The tool does not return the address or say whether it is a pitch inbox or a general one; the founder finds it on the page.
  - "intro only. They say they take referred deals" when the route is INTRO_ONLY
  - "no published route found; check their site: <website>" when the route is null
  Nothing about whether a form takes a deck link or a PDF; form fields are not recorded yet.
- Confidence flags go in a short parenthesis on their own line, in the tool's words: "(stage unknown)", "(thesis text thin)", "(sector focus unknown)", "(no dated activity on file)", or the longer "(stage read as "Early stage AI companies", no round named)". A firm whose lead role is unknown needs no flag; its group says so.
- Conflict line only when the tool found a conflict, or when the founder named competitors and wants to see the check ran. Then "no competing portfolio company found" is one line, not one per firm.

## The three groups, in one line each

The group names come from the tool and a founder should know what they mean before they read the list. Say it once, in the group header or just under it:

- Likely leads: the best-scoring firms that say on their own site they lead or co-lead rounds. Up to eight.
- Strong followers and co-investors: good fits that say they follow, or do not say what they do.
- Stretch picks: large funds (500M and up, or very large assets under management) that fit on stage and sector. Lower odds, worth one well-aimed email, not a first wave.

## Exclusions

Group by reason, count each, and name a firm only when the reason is specific to it (a portfolio conflict, a cheque size larger than the round). The tool returns every conflict and cheque-size exclusion by name and a sample of the stage and dormancy ones, so "show me the firms left out for stage" gets the sample plus the count. For any one firm with a known slug, `get_investor` has its published record, not the reason it was omitted. Do not infer exclusions from absence. Never hide that firms were left out.

The two exclusion reasons that never fire today (a stated geography rule and "paused or not investing") are not printed as zero. Leave them out.

## Non-US founders

The tool's coverage note says no record carries a geography rule, so firms are not filtered by where the founder is. Print the note. Then, when any firm in the list is in the founder's country or region, say which ones in one line before the groups, so a London founder sees "Firms in the UK on this list: SV Health Investors" before 19 US firms. The founder can ask for `local_only` to see only those.

## Refinements

After a refinement, show only what changed: "Added Firm K and Firm L (New York, lead seed). Dropped Firm C and Firm D (corporate)." Then offer the full list again if they want it.
