---
name: thematic-basket-construction
description: Builds custom stock baskets around a theme, event or narrative (client thematic baskets, theme screens, beneficiary or victim baskets, or samples for statistical testing), ranking each name by expected price elasticity to the theme and delivering a Bloomberg-ticker table with tiers and reasoning. Use when the user asks to build, expand, screen or review a stock basket. Also triggers on Chinese requests such as 做篮子、主题篮子、选股、找标的、成分股、扩充/补充篮子、A股/H股主题、产业链标的、退市标的。
compatibility: Needs web search and access to daily price data (e.g. Yahoo Finance). Writing .xlsx output needs Python with openpyxl.
metadata:
  author: Hogan Tong
  version: "1.2.0"
---

# Thematic Basket Construction (Custom Stock Baskets)

## Purpose
Build custom stock baskets for a given theme or event: for statistical testing, client-facing
thematic baskets, or general screening. Use whenever the user asks to build/expand a basket
around a theme, event, or narrative, whether they want the stocks that benefit or the stocks
that get hurt.

## The Master Question
For every candidate stock, everything comes down to a single test:

**If this theme plays out / accelerates, is this stock's price expected to move materially in the direction the user cares about (up for beneficiaries, down for victims)?**

Not "does it have exposure." Not "what % of revenue." Not "where does it sit in the supply
chain." Those are EVIDENCE toward the master question, not independent scoring dimensions.
The deliverable is a judgment with reasoning - not a multi-label scorecard.

Elasticity, used throughout this skill, means how far the stock's price is expected to move
per unit of theme news or acceleration. A high-elasticity name moves a lot, a low-elasticity
name barely moves.

## Direction: Beneficiaries or Victims
Every basket has a direction. A beneficiary basket holds the names expected to rally if the
theme plays out. A victim basket holds the names vulnerable to the theme (hurt by AI
disruption, a rate hike, tariffs, a commodity spike and so on), where the expected move is a
sharp fall.

- Settle the direction before researching. If the user has not said and the wording does not
  make it obvious (e.g. "vulnerable", "hurt by", "losers", "受损", "被颠覆" = victims;
  "benefit", "受益" = beneficiaries), ask in one line.
- The rest of this skill is written from the beneficiary side. For a victim basket, flip the
  sign: the master question becomes whether the stock falls materially, "priced in" means the
  selloff has already happened, "optionality" means potential damage if the theme scales, and
  tiers rank the size of the expected fall.
- In a victim basket, `comment` must state the damage mechanism (lost revenue, margin
  squeeze, higher costs, funding stress), the same way a beneficiary comment states the
  exposure.
- Do not mix directions in one basket. A name that wins from the theme in one way and loses
  in another is a mechanism mismatch: flag it.

## Don't Force a Basket That Isn't There
Not every theme has a clean basket behind it. If, after real research, the candidate set is
all weak/diluted/mechanism-mismatched names — nothing genuinely pure or reasonably pure to
the theme — say so plainly and do not deliver a padded table anyway. A thin or empty
longlist is a legitimate research finding, not a failure to try harder.

- Do not lower the bar on relevance just to reach a "respectable" basket size. A 3-name
  basket of real exposure beats a 12-name basket where 9 are stretches.
- If NO name clears even a defensible low-elasticity bar, tell the user the theme doesn't
  support a basket right now and explain why (too early-stage/private, too diffuse across
  giant diversified companies, no way to isolate the exposure, etc.) — don't ship one out of
  obligation.
- This is a judgment call, not a strict numeric cutoff — flag genuine borderline cases to the
  user rather than silently deciding either way.

## Two Theme Types — different research approach

### Type 1: Structural / emerging themes (no historical precedent)
Examples: AI storage chips, biofuel supply chain, custom ASIC, SMR, GLP-1 chain — anything
novel enough that there's no repeated historical playbook to test against.
Approach: qualitative reasoning using the internal reasoning framework (below). No backtest
possible because the event hasn't happened before in comparable form.

### Type 2: Recurring macro/cyclical events (rate cuts/hikes, recessions, inflation prints, etc.)
Approach: economic logic AND a simple historical backtest — look at how each candidate
actually reacted in past similar events (e.g. the last 3-4 rate cut cycles). Empirical
evidence supplements, and can override, pure logic. Follow the Backtest Rules below.

**If logic and backtest disagree, do not resolve it unilaterally.** Present the conflict
explicitly and reason through it together with the user — no fixed rule for which one wins.

### A quasi-backtest even for Type 1 themes
Type 1 themes have no repeated historical cycle to test, but they often still have at least
one sharp, sudden news event where the market re-priced the theme in real time (e.g. a
sudden headline about an AI storage chip shortage). Look at which stocks actually spiked
hardest on that news. This is not a rigorous backtest — one event, small sample, easily
confounded — but the market's reaction is itself distilled, aggregated information about
which names professional investors judged to be genuinely exposed. Treat it as a useful data
point feeding the framework reasoning below, not as a substitute for it. Follow the Backtest
Rules below.

## Backtest Rules (Type 2 backtests and Type 1 quasi-backtests)
- **Measure excess returns, not raw returns.** Compare each stock with its market index,
  and with its sector where a clean sector benchmark exists. Raw returns just reward high
  beta: in a risk-on move every high-beta name "reacts" whether or not it is exposed.
- **You choose the event window, not the user.** Pick it by judgment for this theme: how
  fast the news was absorbed, whether there was a leak or a build-up, and whether other
  news landed at the same time. State the window and a one-line reason in the deliverable.
  Where it matters, check that the ranking holds up under a slightly shorter or longer
  window. A name that only looks exposed under one exact window is weak evidence.
- **Be honest about sample size.** 3-4 past cycles, or one news event, is a small sample.
  Treat the result as evidence, not proof, and say so.
- **Check the company is the same business.** A stock's reaction in 2008 means little if
  its business mix has since changed materially. Discount or drop those observations.
- **Watch for confounders.** Earnings, index events or company-specific news inside the
  window can make a stock look like it reacted to the theme when it did not.
- **Data:** use real price data you can pull (free sources such as Yahoo Finance, a local
  price database, or other market data tools available to you). Never estimate a
  historical reaction from memory. If the data isn't available, say so and fall back to
  logic.

## Co-movement Sanity Check (does the market trade these names together?)
This follows the spirit, not the letter, of Candès, Hastie, Kahn et al., "Thematic
Investing: A Risk-Based Perspective" (Financial Analysts Journal, 2025). Their finding: a
basket whose names genuinely move together, beyond what the broad market explains, tends
to trend. A basket whose names don't move together tends not to. So if the market
doesn't trade the names as a group, the basket is unlikely to show the
trend/acceleration that statistical testing is looking for.

The paper used a commercial risk model with style factors. You won't have that, and you
don't need it. This is a quick, rough check with free price data, not a pass/fail test.

**How to run it (keep it light):**
- Take daily returns over a window when the theme was live (your judgment; state it).
  Remove the market's move from each stock, using its local market index, so that you
  are looking at what's left over. Add a sector index too if one is freely available.
- Look at how correlated the leftover returns are across the basket, and at each name's
  average correlation with the rest.
- Rough bootstrap, in the spirit of the paper: compute the basket's average pairwise
  correlation of leftover returns, then do the same for a few hundred random baskets of the
  same size drawn from the same market. Where the real basket ranks among them is a rough
  p-value (e.g. "higher than about 95% of random baskets"). Approximate is fine: no need to
  match liquidity or sector for the random draws, and no formal test statistics.
- Do not swap names in or out because they correlate. The names come from fundamentals;
  picking them for their correlation makes the number meaningless.
- Optional, often more telling for event-driven themes: did the names move together on
  the theme's key news days?

**How to use the result:**
- It is a sanity check, not a filter. Fundamental judgment decides the basket.
- Its main use is per-name: a name that clearly doesn't move with the rest deserves a
  second look. It is either early (the market hasn't noticed yet, which could mean
  optionality) or doesn't belong. Say which in its comment.
- If almost nothing moves together, mention it. It may mean the theme is too early or too
  diffuse, but weak co-movement alone is not a reason to scrap a basket.
- Short price histories (recent IPOs) and thin trading give noisy numbers. Don't read
  much into them.
- Report it in one line above the table, e.g. "Names co-move moderately beyond the
  market over [window] (above roughly 90% of random baskets); X and Y don't." Don't add a
  column.
- Real price data only. If you can't get it, say the check wasn't run.

## Map the Theme Before Building the Longlist
Before searching for names, split the theme into its sub-themes and value-chain links.
For AI, that would be compute, memory, networking, power, cooling and so on. For a
rate-cut theme, it would be the separate channels the cut works through. Then search each
branch. This stops you from missing a whole branch just because the obvious names sit in
one corner. It also shows where elasticity clusters, which should shape the tier boundaries.
The map is a thinking tool. Include it in the deliverable only as a short line, if it
helps explain the tiers.

## The Internal Reasoning Framework (thinking tool, NOT the deliverable)
Used to keep the reasoning toward the master question systematic rather than gut feel.
Never output these as separate labels or scores in the final deliverable — they get
distilled into the comment/reasoning for each name.

- **Industry chain position** — direct play / enabler / downstream beneficiary. Purely
  explanatory: it helps explain WHY a stock would or wouldn't move, it is not itself a
  pass/fail filter.
- **Current revenue exposure to the theme.**
- **Disclosed customer and supplier links.** Trace where the money spent on the theme
  actually goes. US 10-Ks name every customer above 10% of revenue, and many other markets
  have similar disclosures. Supplier lists, named contracts and procurement announcements
  also count. If a supplier gets a large share of its revenue from the company driving the
  theme, that is hard evidence of high elasticity: cite the percentage in the comment. These links
  can also surface second-order names that no thematic list includes.
- **Potential revenue exposure if the theme scales** — this can matter MORE than current
  exposure. A name with low current exposure but high optionality can be a better
  re-rating candidate than one already priced for its exposure (market has already
  discounted a high current-exposure name; a low-exposure name with real optionality has
  more room to re-rate on the surprise). **But potential exposure needs evidence, or it is
  just a story.** Rank the evidence, strongest first:
  1. Named contracts or order backlog tied to the theme.
  2. Capacity or capital spending committed to the theme (plants, fabs, product lines).
  3. Specific management guidance (numbers, timelines).
  4. Consensus estimates for the relevant segment.
  5. Management merely talking about the theme. This is the weakest: companies namecheck
     hot themes freely.

  Say in the comment which level of evidence the optionality case rests on. A case built
  only on level 5 belongs in the weakest tier, or gets flagged as speculative.
- **(Type 2 only) historical price reaction in past analogous events.**

This list is not necessarily exhaustive — other thinking angles may surface theme by theme.
Check with the user rather than assuming this is the complete set.

## Elasticity Tiers — standard output for EVERY basket, tier count is judgment-based
Every candidate gets sorted into a tier by overall expected price elasticity to the theme.
This is the master question expressed as a ranking, not a separate fourth question — assign
it for every name, in every basket, Type 1 or Type 2, by default.

**The number of tiers is not fixed at three.** Use however many the basket actually needs to
describe real, distinct clusters of elasticity — two if the names genuinely split into only two
groups, four or five if there's a meaningfully different rung (e.g. a "mechanism confirmed
by real market reaction" rung above ordinary Tier 1, or a "confirmed but pre-revenue/private"
rung that needs separating from listed Tier 3 names). Do not force names into three buckets
if the honest picture has more or fewer gradations, and do not manufacture extra tiers just
to look thorough.

**Whatever tier scheme is used, write out what each tier means, in the deliverable itself,
before or alongside the table** — the reasoning for where a cutline sits matters more than
the label. A reader should be able to understand the ranking logic from the tier definitions
alone, without needing this skill file.

Evidence used to assign tiers depends on theme type:
- **Type 2 (backtestable):** lead with measured historical beta/reaction of the stock across
  past analogous events, refined by framework reasoning (is that reaction likely diluted this
  time by size, by being multi-driver, or by already being priced in).
- **Type 1 (structural, no precedent):** framework reasoning, plus the quasi-backtest above where
  a real news event exists — chain position, current vs potential exposure, whether the
  market has already priced the exposure in, and whether the theme's driver spending more
  money actually converts into this company's revenue/earnings or into some other mechanism
  entirely.

A reasonable default, if nothing about the basket demands otherwise, is a three-way split by
elasticity:
- **High elasticity.** Expect the largest price response per unit of theme acceleration.
  Concentrated or pure-play exposure, not yet fully priced in, real optionality, or (Type 2) a
  strong measured historical beta.
- **Moderate elasticity.** Real, defensible exposure, but diluted — by company size, by
  being one driver among several, or by already having re-rated hard on known information.
- **Low/structurally capped elasticity.** Exposure is real but something dampens the
  payoff: regulation capping how revenue converts to earnings (e.g. a rate-of-return
  utility), extreme diversification, an indirect/mechanical link one or two steps removed
  from the theme's driver, or a genuinely unconfirmed/speculative connection.

Treat this three-way split as a starting point to adapt, not a rule to default back to
without thinking.

**Mechanism mismatches are flagged separately, not forced into a tier.** If a name's
connection to the theme runs on an entirely different economic logic than the rest of the
basket — e.g. an equity/pre-IPO stakeholder benefiting from a valuation re-rating, sitting
in a basket otherwise made of revenue-linked suppliers — say so explicitly and let the user
decide whether it belongs in this basket or a separate one. Don't silently drop it and don't
silently blend it into a tier where it doesn't actually fit.

A useful sub-lens when the theme's driver spending more money is specifically a revenue
question (as opposed to general "will it move"): direct named contract > indirect but
mechanically near-certain (e.g. the foundry that must fab the chips, the supplier riding a
market-wide shortage the theme drives) > real revenue with a structurally capped payoff. This
maps onto the three tiers above but gives sharper reasoning for supply-chain-style themes.

## Information Sourcing & Priority
- Earnings reports/filings are the top source, but serious, high-quality sell-side research
  can rank slightly ABOVE them for a live basket build: filings only get a major refresh
  every six months to a year, while credible sell-side research gets updated any time the
  company or its industry actually changes. Query both, in parallel.
- "Serious sell-side research" means a real, established research house or named analyst
  covering the name/sector professionally — not just anyone publishing a "report." Hold this
  to a high bar. Low-quality, promotional, or unaccountable research is worse than no
  research at all — don't launder it through the word "report."
- Reputable financial blogs/newsletters can also be used as supplementary color, again under
  strict quality control — a track record, named authorship, and reasoning that can be
  checked, not anonymous hot takes.
- General news ranks below filings/sell-side research.
- Company websites rank lowest.
- **Thematic ETF holdings: only for pre-screening.** They are a cheap way to seed the
  longlist and to check for obvious omissions. They are NOT evidence that a name belongs
  in the basket. Most ETF issuers are not good at research, and their holdings are often
  shaped by index rules and marketing rather than real exposure. Every name that came from
  an ETF must still pass the master question on its own evidence.
- Query sources in PARALLEL, not sequentially waiting on one before trying the next.
- If sources genuinely conflict, present the conflict to the user rather than silently
  picking a side.

## Liquidity Filter
- **The bar depends on the market.** Use the trailing average daily traded value (ADV),
  converted to USD, and compare it with the bar for the name's listing market. A name below
  the bar does not go into the basket automatically.

  | Market | ADV bar (USD) |
  |---|---|
  | US | 100m |
  | Mainland China A-shares (CH) | 50m |
  | Japan (JP) | 50m |
  | Hong Kong (HK), Korea (KS), Taiwan (TT) | 30m |
  | Large European markets (UK, Germany, France, Switzerland, Netherlands) | 30m |
  | India (IN), Canada (CN), Australia (AU), other developed markets | 20m |
  | Other emerging markets | 10m |

  These bars are judgment, not a formula. They are anchored on how deep each market is (2025
  traded value per trading day, roughly: US exchanges USD 320bn, mainland China 240bn,
  Japan 35bn, Hong Kong 24bn, Korea 13bn, Euronext 12bn) and rounded up, because a bar
  scaled strictly by turnover would be too loose to trade. If a market is not in the table,
  pick the nearest comparable row and say so.
- **Exception: genuine exposure but lower liquidity.** If a name has real exposure but
  falls below the bar, don't silently drop it and don't silently include it. Flag it to
  the user with its ADV and the exposure case, and let them decide.
- **Some themes are illiquid by nature.** Some themes are made up mostly of small or
  illiquid names (e.g. early-stage or small-cap niches). If the genuinely exposed universe
  is mostly below the bar, it is fine to build a basket of illiquid names. Don't force in
  liquid but weakly exposed large caps to meet the bar. State plainly in the deliverable
  that this basket is illiquid, and why.

## China A-share and Hong Kong Specifics
Apply this section whenever the basket contains A-shares ("CH"), Hong Kong ("HK") names or
A+H dual listings. The general rules in this skill still apply. These are extra checks, because
mainland and HK names carry mechanics and information gaps the US-style workflow misses.
For any rule below that can change (price limits, Connect lists, delisting rules), check the
current version on the exchange site before relying on it, and never state it from memory
as fact.

**Sourcing in Chinese.** Much of the best evidence exists only in Chinese, so search and read
in Chinese, then write the comment in English.
- Annual and interim reports (年报/半年报), prospectuses (招股说明书), and exchange
  announcements (cninfo for A-shares, HKEXnews for HK) are the top source. Named contracts,
  capex projects (在建工程, 募投项目) and top-five customer/supplier tables (前五大客户/供应商) are
  the disclosed customer and supplier links to use. Chinese filings often name the
  customer and give its share of revenue.
- Mainland broker research (研报) is serious sell-side research if it comes from a real house
  with a named analyst. Research communities such as 知识星球 can be a source for it.
- Investor Q&A platforms (互动易, 上证e互动) are management talking. Treat replies there as
  level 5 evidence (the weakest) in the optionality ranking, never higher, however specific
  they sound.
- Chinese-language retail and media theme lists (概念股 lists) are marketing, not evidence.
  Treat them like thematic ETF holdings: pre-screening only.

**Tradability and status screens.** Run these for every A-share name and state the result in
the comment when it matters.
- ST / *ST status (risk-warning). Flag it. It signals delisting risk and a tighter daily
  price limit, so a name can look exposed and still not be a sensible basket member.
- Suspension (停牌). A name that has been suspended for a long stretch has meaningless ADV and
  a stale price. Flag it, and exclude it from the co-movement check.
- Board and daily price limit. Main board, ChiNext (300xxx), STAR Market (688xxx) and the
  Beijing exchange have different limits. Limit-up/limit-down days truncate returns and
  distort both the backtest and the co-movement check, so mention it if the event window
  contains limit days for a name. Bloomberg's code for Beijing-exchange names should be
  verified, never guessed.
- Stock Connect eligibility (northbound for A-shares, southbound for HK). State whether each
  name is eligible. For a client or desk that trades through swaps or Connect, a name outside
  Connect is much harder to access, which affects whether it belongs in the basket.
- Free float, not total market cap. State-owned or founder-controlled A-shares can have a
  small free float relative to total market cap. Use free-float market cap when judging
  size and liquidity.
- ADV must be converted to USD (CNY and HKD turnover) before applying the liquidity bar.
  A-share turnover is heavily retail-driven, so check that ADV over the event window is not
  inflated by a one-off spike.

**Hong Kong specifics.**
- Chapter 18A biotechs (pre-revenue, marked "-B") and WVR structures (marked "-W") are
  common in thematic baskets. Pre-revenue names cannot support a revenue-exposure case.
  Their optionality rests on pipeline evidence, so grade it by the evidence ranking and
  do not put it above level 3 unless there is a named contract.
- Chapter 18C specialist technology names are similar: check for actual revenue.

**A+H dual listings.**
- Decide deliberately which line goes in the basket, and say why (liquidity, Connect access,
  which line the market actually trades on the theme). Do not silently include only one, and
  do not include both unless both are tradeable and you want double weight on the name.
- Mention the A/H premium in the comment if it is large enough to matter.

**Survivorship.** Mainland delistings have become more frequent since the stricter delisting
rules. Search for delisted and long-suspended names in the theme specifically, and apply the
same include-delisted rule as in the Output Format section below. Bloomberg recodes
the ticker on delisting, so verify it or write "n/a".

**Dates.** `ipo_date` for an A-share is the exchange listing date (上市日期), not the prospectus or
approval date. For HK, use the listing date on HKEXnews.

## Output Format
Single table, columns exactly:

**bbg_ticker | name | ipo_date | delist_date | tier | comment**

**File format: .xlsx, not CSV** (a CSV lets Excel mangle tickers and dates). If you can't
write files, give the table inline instead. Ask the user what the file should be called
and where to save it, rather than choosing yourself.
Main sheet = the basket, headers as above (bold white text on a dark blue fill, frozen top row), every cell stored as text
so Excel does not reformat tickers or dates, `comment` column wide with wrap text. If names
were screened out, put them on a second sheet "Excluded_not_in_basket" in the same columns
with the reason in `tier`/`comment`.

- **Composite ticker, always.** Use the Bloomberg composite ticker for every market, never an
  exchange-level ticker. The one exception is Germany, which uses "GY" (Xetra), never "GR"
  (composite) or "GF". Examples: US "US" (never UN, UW, UQ), Japan "JP" (never JT), Canada
  "CN" (never CT), Australia "AU" (never AT), Switzerland "SW" (never SE, VX), India "IN"
  (never IB, IS). For any other market, use the composite code.
- `bbg_ticker` must be real Bloomberg format, always with the "Equity" suffix (e.g.
  "AAPL US Equity", "386 HK Equity", "000001 CH Equity"). Three quirks:
  - **ADR vs home line (non-US, non-A+H names):** one row per company. If the company has an
    ADR on an exchange (NYSE or Nasdaq, NOT pink sheets / OTC), compare median 60-day USD
    ADV of the ADR against the home line and use whichever is more liquid. A pink-sheet or
    OTC ADR is never used, and neither is an ADR that has been delisted from NYSE/Nasdaq.
    Say in `comment` which line was used and the two ADV figures, even when the home line wins.
    This does not apply to A+H dual listings, which have their own rule.
  - **HK-listed:** Bloomberg drops the leading zero from the exchange code. The exchange
    lists "0386.HK"; Bloomberg is "386 HK Equity" — never "0386 HK Equity".
  - **A-shares (Shanghai/Shenzhen, "CH") and Korea ("KS"):** leading zeros are KEPT. Ping
    An Bank is "000001 CH Equity", Samsung Electronics is "005930 KS Equity" — do not strip
    these. The no-leading-zero rule is HK-specific, not a general Bloomberg convention.
  - If a candidate isn't tradeable yet (pre-IPO) or the ticker is unconfirmed, write "n/a"
    rather than guessing a ticker.
- Include delisted names — do NOT survivorship-bias the basket by only keeping names still
  trading. Matters for any historical backtest built on the basket later.
- `delist_date`: actual date if delisted; leave BLANK if still active. Do not write "N/A" or
  "active" — blank is the convention.
- **Never invent dates.** `ipo_date` and `delist_date` must come from a source you actually
  checked (filings, exchange data, Bloomberg/market data, reliable news). If you could not
  verify a date, write "unverified". Never write a plausible-looking guess. For a delisted
  name this is the only exception to the blank convention: "unverified" means you know it
  delisted but couldn't confirm when. Bloomberg often re-codes a ticker after a delisting,
  merger or name change, so check that the ticker for a delisted name is the Bloomberg one,
  or write "n/a".
- `tier`: write the number/label PLUS a short relevance tag, so the tier is self-explanatory
  without cross-referencing this skill — e.g. "1 - high elasticity, direct", "2 - moderate,
  diluted by size", "3 - weak, structurally capped" (adapt the number of tiers and their
  tags to what this specific basket actually needs — see Elasticity Tiers above). Never a
  bare digit. If a name is a mechanism mismatch (see above), write that instead of forcing a
  tier, e.g. "equity stake, not tiered."
- **Delisted names are tiered like any other name.** A delisting is recorded only by filling
  `delist_date`. It is NOT a tier and NOT a reason to skip tiering: the user needs to know which
  tier the name belonged to while it traded. Never write "delisted, record only" or any similar
  label in `tier`. Assign the tier the name would have had at its last trading date, from its
  business at that time. The only exception is the usual one: a genuine mechanism mismatch is
  flagged as such, delisted or not. If the delisting is itself relevant (take-private, merger),
  say so in `comment`.
- **Language of the table: always English**, whatever language the user writes in or the
  sources are in. This covers `comment` and the `tier` tags. `name` is the official English
  name; if a mainland name has no standard English name, use the company's own English name
  or a transliteration, followed by the Chinese name in brackets. Keep a Chinese term in a
  comment only in brackets after the English, where the English is not standard (e.g.
  "risk-warning status (*ST)"). Text outside the table (tier definitions, the co-movement
  line, notes to the user) follows the language the user is writing in.
- `comment`: the reasoning that answers the master question and justifies the tier. Cite the
  framework factors (chain position, exposure now/potential, historical reaction for Type 2) as
  supporting evidence inside the prose — this is where the judgment gets justified, not a
  separate label dump.

## Workflow Checklist
1. Settle the direction (beneficiaries or victims), then classify the theme: Type 1
   (structural/emerging) or Type 2 (recurring macro/cyclical).
2. Map the theme into sub-themes and value-chain links, then generate a longlist for each
   branch via the sourcing priority above, including disclosed customer and supplier links
   (thematic ETFs may seed it, never justify it). If nothing clears even a defensible
   low-elasticity bar, stop here and report that this theme has no clean basket — see "Don't
   Force a Basket That Isn't There" above.
3. For each candidate, reason through the framework privately to answer the master question.
   Rate optionality claims by the evidence ranking under "Potential revenue exposure".
4. Type 2: pull a historical backtest across past analogous events for the candidate set.
   Type 1: run the quasi-backtest if a real news event exists. Both follow the Backtest
   Rules (excess returns, a window you choose and justify, real data only).
5. Type 2 only, if logic and backtest conflict: flag it to the user, don't decide solo.
6. If the basket contains A-shares, HK names or A+H pairs, run the "China A-share and Hong
   Kong Specifics" checks above now (and apply them again when
   writing comments).
   Then apply the liquidity filter: the ADV bar for each name's market (see the table in
   Liquidity Filter); flag exposed names below it to the user; if the theme is illiquid by nature, an illiquid basket is fine, but say so.
7. Run the co-movement sanity check on the draft basket. Take a second look at any name
   that clearly doesn't move with the rest, and say in its comment whether it is early or
   doesn't belong.
8. Decide how many Elasticity Tiers this basket actually needs, define what each one means,
   then assign each candidate to one — or flag it as a mechanism mismatch.
9. Deliver the tier definitions, the co-movement line, plus the single table (ticker /
   name / ipo_date / delist_date / tier / comment), in English whatever the user's
   language. Never the raw framework as a separate scorecard; the tier is the one exception
   already built in.
10. Delisted names included and tiered like the rest, delist_date filled where applicable. Every date is sourced or
    marked "unverified". Nothing is guessed.
