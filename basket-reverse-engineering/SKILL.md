---
name: basket-reverse-engineering
description: Reverse-engineers a stock basket (pasted text, screenshot or table). Infers what the names are trading (themes, ideas, strategies), how exposed each name is to each theme, how the names were picked and weighted, which extra filters the author applied beyond thematic exposure, and the investment thesis. Use when the user sends a basket and asks what it is, what it is betting on, or to guess the theme, selection rule or strategy behind it. Also triggers on Chinese requests such as 这个篮子在炒什么, 这个篮子是什么主题, 帮我看看这个篮子, 猜一下他们的选股逻辑, 反推篮子, 拆解篮子。
compatibility: Needs web search, access to company fundamentals and daily price data (e.g. Yahoo Finance), and vision for screenshot input.
metadata:
  author: Hogan Tong
  version: "1.0.1"
---

# Basket Reverse-Engineering

## Purpose
Baskets arrive from clients, colleagues and competitor products, in any format. Work
backwards: what are these names trading, how strongly is each name tied to each idea, how
were the names chosen and weighted, what selection filters sit on top of the theme, and
what is the thesis. The result is a reasoned inference, not a verdict. Grade how firm each
conclusion is (Step 6h).

This is the reverse of building a basket. Do not rebuild the basket unless asked. Do not
comment on whether the theme is already priced in. Do not list missing names for their own
sake: only those that help identify a filter (Step 6).

## Reading the lists in this skill
Every list of metrics, themes, drivers or examples here is illustrative, not exhaustive.
Treat each as a starting point: add any other dimension the data suggests, and do not
limit yourself to what is written.

## Input
Plain text, a screenshot (read it with vision; transcribe titles, tickers, weights and
labels exactly), or a table or CSV. Transcribe first and state what you read in one line,
so a misread is caught early. Titles, weights, group labels and accompanying text are
evidence, but not every input has a title, and some come with only a sentence or a short
description of the idea. Use whatever text exists as a lead, never as the answer: titles
and descriptions can be marketing, loose or wrong, and the stocks decide. A typo in a
ticker is possible: check the obvious alternatives before proceeding and say which one you
assumed.

Ask for these if missing, because they change the quality of the answer: the date the
basket was created or last changed, other baskets from the same author, and any ETF or
list the author says they followed.

## How the work is ordered
Phase 1 (Steps 2-3): decide which theme or themes the basket is built on.
Phase 2 (Steps 4-6): given those themes, work out how the stocks were chosen and weighted
within each, and what filters sit on top. Do not start Phase 2 before Phase 1 has a
defensible theme list.
Steps 7-9 add evidence and checks. The output (last section) has a fixed shape.

## Step 1. Identify every name properly
- Search each name: main business, segment revenue mix, market cap, listing date and
  venue. Never work from memory. Unrecognised names may be recent IPOs, renamed companies
  or spin-offs: say so, do not guess what they do, and if a name cannot be identified, say
  so.
- Pull a fundamentals snapshot for the set. It feeds the filter test in Step 6 and the
  weight test in Step 5. Cover, as far as the data allows:
  (a) Size and tradability: market cap, free float, average daily traded value (ADV),
  listing date and venue, country and currency, index membership.
  (b) Growth: revenue and EPS growth (trailing and forward), estimate revisions.
  (c) Profitability and quality: gross, operating and net margin, ROE, ROIC, FCF margin,
  earnings stability, R&D and capex intensity.
  (d) Balance sheet: net debt/EBITDA, interest cover, cash runway for loss-makers,
  dilution.
  (e) Valuation across several multiples, not just P/E: forward and trailing P/E, EV/Sales,
  EV/EBITDA, EV/EBIT, P/B, P/FCF or FCF yield, PEG, dividend yield, and sector-specific
  ones where they fit (P/NAV, EV/subscriber, EV/reserves, etc.). Use the multiple that the
  market actually uses for that business.
  (f) Analyst view: consensus rating (buy/hold/sell split), target-price upside, number of
  analysts covering, direction of recent rating and EPS revisions, dispersion of
  estimates.
  (g) Price behaviour and positioning: returns over several windows (1m, 3m, 6m, 12m,
  12-1m), distance from 52-week high, beta and volatility, drawdown, short interest,
  institutional and insider ownership.
  (h) Anything else the basket points to, etc.
  Use values as of the basket's creation date where known, and say where you had to use
  current data.

## Step 2. Brainstorm the themes: what could these names be trading?
A theme does not have to be a sector. It can be a macro or policy driver, a demand cycle,
a business-model trait (pricing power, installed-base annuity, serial M&A), a style
(quality, momentum, low leverage) or a trade structure (barbell, hedge, diversifier).
Diverge first, then prune. Do not start from your own prior about what the companies do.

Generate candidates from outside evidence:
1. Segment and end-market data from filings. Good for measurable sector themes, blind to
   narratives and styles.
2. Sell-side research and earnings-call language: what story is each name covered under?
   Rank sources: serious, named, accountable research first, then reputable newsletters,
   then general news, company websites last. Promotional research is not evidence.
3. News: what recent events repriced these names together (contract awards, budgets,
   policy, a competitor's guidance, a supply shock)? Note which names moved most. One event
   is a small, confounded sample, but the market's reaction shows which ideas it ties to
   which names. Use excess returns over a stated window and real price data.
4. Thematic ETFs and published baskets that hold several of these names. Pre-screening
   only: they suggest the label the market uses and never prove a theme.
5. Market-implied clusters: residual co-movement among the names and loadings on sector,
   style or theme ETFs.
6. The basket's own title, group labels and any accompanying text or short description,
   if present. A lead only: many inputs have none, and text can mislead. Never build the
   theme list on it alone.
7. Other directions to try (a starting list only): geography or regional policy; regulation
   or subsidy; commodity or input-cost exposure; rates, FX or other macro sensitivity; a
   supply-chain bottleneck; a technology adoption curve; a capital-spending cycle; corporate
   actions (spin-offs, takeover targets, activism); ownership structure (state-owned,
   family-controlled, insider-held); shareholder returns (buybacks, dividends);
   balance-sheet themes (net cash, deleveraging); positioning (crowding, short squeeze,
   retail attention); index or ETF flow events (inclusion, rebalance, lock-up expiry);
   demographics; climate or transition; and pair or hedge structures, etc.

How fine should the sector be? If the natural reading is sector-based, go much finer than
a classification code. A theme is often a single product, technology, end-market, customer
group or value-chain link (a component such as MLCC, a memory type, a packaging step, a
drug class), which is finer than even the lowest GICS sub-industry. Classification codes
group unrelated businesses under one label, so use segment and product-line revenue data,
filings and how the companies describe themselves, and finer vendor taxonomies where they
exist (revenue-based industry classifications from data vendors are deeper than GICS). Name
the theme at the level where the companies really share one driver.

Merge into a long candidate list, then prune with Step 3, keeping the ideas that explain
the most names. Test one broad theme against several narrow ones and prefer the simpler
reading only if it explains the names about as well. Keep the final list short (usually 2-4
themes). Mark outliers: a name that defines the edge of a theme, a hedge or diversifier, a
label-justifier (the one foreign name behind a "global" title), or noise. If no coherent
theme explains the names, say so: it may be a style or factor basket, or names picked for a
reason unrelated to theme. Many names are strong on more than one theme at once. Do not
force a name into a single bucket: keep every theme it is genuinely exposed to.

## Step 3. Exposure of each name to each theme
Give a degree for every name and theme, not yes or no. Use the four plain-text grades below, and
state the legend in the output. No emoji:
- High: main driver: the theme drives more than roughly half of revenue or profit, or is the
  main driver of the stock.
- Medium: second driver: roughly 20-50%, or a clear second driver.
- Low: minor: under roughly 20%, or present but minor.
- None: no exposure.
A name can score High on several themes at once. Grade each theme independently and keep all
of them. Do not force a primary theme.
Quantify with the revenue share where disclosed (segment, end-market or product-line data)
and say so. Where the theme cannot be measured from financials (a style, a narrative),
grade it on the relevant metrics and mark it as judgment. Say which figures are sourced
and which are estimates. Never present an estimate as disclosed data.

## Step 4. Picking style within each theme
Take each theme in turn and describe each name on the dimensions a builder would use to
choose it, then see which dimensions the basket appears to favour:
(a) chain position (direct play, enabler, downstream);
(b) how much of current revenue comes from the theme;
(c) disclosed customer and supplier links to the theme;
(d) potential future exposure and how firm the evidence for it is (named contracts >
committed capex > company guidance > consensus > talk);
(e) how strongly the stock price should respond if the theme accelerates.
- Are the names the leaders (purest, largest) or a spread across sub-themes and chain links?
- Is there visible tiering, for example a high-torque core plus diluted large caps?
- Which obvious names in the same theme were not picked? Only those that help expose a
  rule (Step 6).
The answer is the picking style: pure-play, leaders, chain spread, or something else.

## Step 5. Read the structure and the weights
- Count the names and read the group labels and title wording ("core", "satellite",
  "global"). Note size and liquidity, survivorship and IPO age.
- Weights are evidence about how the author thinks. Analyse them, do not just describe
  them. First name the pattern: equal, cap-weighted, liquidity-capped, inverse-vol,
  score-tilted, theme-bucket budgets (weights sum by theme), tiered, or hand-set with a
  residual. Check the step size (multiples of 5% point to hand-set tiers) and whether the
  weights were edited after publication.
- Then test why any name is over or underweight. Put each candidate driver next to the
  weights. Examples, not a closed list: expected price response to the theme, market cap,
  free float, ADV, quality and pricing power (margin, ROIC, etc.), growth, valuation on
  several multiples, analyst rating or target upside, estimate revisions, volatility or
  beta, momentum, theme purity, number of themes the name is exposed to, chain position,
  dividend yield, short interest, ownership, index membership, country, and theme-bucket
  sums. Add any other driver the data points to. A driver is supported only if the over-
  and underweights line up with it AND the equally weighted names do not spread widely on
  it. If a driver varies a lot across equal-weight names, it is not what sets weights. Note
  which drivers cannot be tested because too few names deviate.
- If nothing explains the deviation, say the weights carry no information beyond roughly
  equal (or a residual) and do not invent a reason. A basket equal-weighted across names of
  very different size and volatility is not cap-, liquidity- or risk-weighted.
- What the structure implies about the product: client sleeve, model portfolio, trend-test
  sample, event basket, factor exposure, structured-product underlying.

## Step 6. Filters on top of the theme
Question: beyond thematic exposure, what rule or judgment selected these names over similar
ones? Use a two-sided test. Never just list the absentees. Work through 6a to 6h in order.

6a. Build two probe sets before pulling any data.
- Should be in but absent: for every link of the theme, the chain leaders, the top ten
  holdings of the closest ETFs, listed peers, and names this author used in other baskets.
  Write one line per name on why it should belong, then pull data. There is no target
  count: include every name that genuinely belongs, and do not pad. Some themes have very
  few candidates (a component such as MLCC has only a handful of listed makers). A short
  list is fine, but say so, because a small absent set makes the test weak (Step 6h).
- Looks out of place but present: the basket names furthest from the theme.

6b. One table, both sides together. Score every present and absent name on market cap, ADV,
listing history, venue, growth, profitability, valuation (several multiples), analyst
rating, momentum, ownership, theme purity, whether it sits in a sibling basket, and
compliance status, etc. (any Step 1 metric). A quantitative filter is established only
when there is a gap: the worst present name clears the line AND the best-fitting absent
names fall on the wrong side. Present names all passing is not enough. If an absent name
sits inside every band, that metric is not the filter.

6c. Near-twin control. Purpose: hold theme exposure constant, so that whatever differs
must be the filter, not the theme. Take an absent name that looks almost identical to a
present one on theme, chain position and business (for example two GPU clouds), and list
what separates them: size, liquidity, listing age, profitability, valuation, rating,
ownership, etc. That separating dimension is a candidate rule. It is the case-level
version of the 6b table, and it is only useful when a real near-twin exists. Do not pair
names that are merely in the same sector. One pair proves nothing: trust a dimension only
if several independent pairs, or the 6b table, point to it as well. If no real twin
exists, skip 6c.

6d. Outliers. For each looks-out-of-place name, see on which dimensions it is an outlier.
One dimension (same IPO window, same catalyst, same sibling basket) is a clue to a rule.
Many dimensions means a subjective addition (narrative, client holding): say "subjective,
cannot be reverse-engineered" and do not force a rule.

6e. Order of hypotheses: mechanical threshold, then rank-based (top N on a metric), then
theme-purity judgment, then subjective. Stop at the first level that explains the data.

6f. Point in time. Check each absent name's state on the creation or update date: not yet
listed, too small, short history, different price trend. Current data is a limitation, so
say what the conclusion would change to if the state was different.

6g. Cross-basket. If several baskets come from the same author, see how names are
allocated between them. A name in one basket and never in another, or a name repeated on
purpose, often exposes the partition rule. It also shows whether a filter seen in one
basket is applied author-wide: a filter contradicted by a sibling basket is not a general
rule.

6h. Grade every conclusion on four levels and say what data is missing for each:
confirmed (both sides fit), fits (one side only), suggested (a clue, not tested), not
found. With fewer than five or six absentees the test is weak by construction: say so.

Subjective filters (inferred): sole-source or certified positions, installed-base
aftermarket, serial-acquirer record, management quality, customer concentration, regulatory
or reputational overhang, the author's own narrative. Mark each as inferred, give the
evidence, and never invent a reason for an absence. A feature shared by the absent names is
probably the real criterion and outranks the title.

## Step 7. Co-movement as evidence, not a criterion
Run a light co-movement check (market-adjusted daily returns, stated window, real price
data). Low overall co-movement does not undermine a multi-theme basket. Look at co-movement
within each theme cluster and at names that sit apart. One or two lines. If there is no
price data, say the check was not run.

## Step 8. Thesis and strategy (working notes)
- Thesis: the mechanism. Who spends, how it reaches these companies, what must hold.
- Strategy: what the basket is for.
Decide how firm each is and whether a credible alternative reading survives the tests.
These are inputs to the Verdict and are not written out as a separate section.

## Step 9. Usability check: could a builder reproduce the basket from the brief alone?
Design principle: hand the Verdict paragraph to an independent builder who picks stocks by
theme (for example, one following the companion thematic-basket-construction skill), and
they should land on roughly the same names and weights. Do not run a builder; keep that
mindset and test the brief before writing the output. Such a builder
thinks like this: a theme has a driver ("if this accelerates, which stocks rally most?");
the theme is split into sub-themes and value-chain links; names are ranked by expected
price elasticity to the theme, not by quality or size. Check the brief against that:
- Themes are phrased as a theme or event with a driver, with sub-themes and chain links
  named, and whether it is structural or a recurring macro event. A style or quality trait
  (compounder, pricing power, low leverage) is not an elasticity theme: write it as a
  selection overlay, separate from the themes, or the builder will ignore it.
- The brief carries everything a builder would otherwise decide differently: chain
  positions to include and exclude, any liquidity or size bar and where the data put it
  (only if Step 6 found one), overlays, and whether delisted names were in scope. Do not
  assume a bar that the data did not show.
- Given only the brief, which names would a builder add that the basket lacks, and which
  basket names would they drop? Each mismatch means the brief is missing a rule or the
  basket holds a pick no rule explains. Fix the brief or list the name as unexplained. Do
  not hide it.
- A builder does not set weights, so the brief must state the weight pattern from Step 5.
  Test weights too.
Report the result in one or two lines at the very end of the last section of the output.

## Output
Punctuation in the output (hard rule). Write like a human typing on a keyboard. No em dashes or en dashes: use a comma, colon, period or a plain hyphen "-" instead. Use straight quotes (" and ') only, never curly quotes. No ellipsis character (use three periods), no emoji, no non-breaking hyphens or special spaces. In Chinese, use the full-width punctuation a Chinese keyboard produces (，。：；？！“”（）、), never the English em dash. Before sending, scan the draft for these characters and fix them.

Plain text, no tables, short enough to read in a minute or two, even on a phone. Keep the
reasoning short: findings and conclusion, not the whole argument. Exactly four sections
(skip section IV if no weights were given). No separate sections for concerns, usability or
verification: Steps 7 to 9 still run and their findings go inside these sections. Write in
the language the user writes in, using the matching label set below. Mark facts as
disclosed, estimated or inferred where they appear. Cut explanation of method; keep results.

Hierarchy, three levels, each with its own symbol. Never reuse one numbering style at two
levels. The two sets below are worked examples. For any other language, translate the
section titles and grade words (keeping the English title in brackets after the translated
one), use that language's own numbering and punctuation conventions, and keep the three
levels visibly different.
- English: sections I. II. III. IV. (bold line), items 1. 2. 3. (restart in every section),
  sub-points (a) (b).
- Chinese: sections 一、二、三、四、 (bold line), items 1、2、3、 (restart in every section),
  sub-points （1）（2）. Use full-width Chinese punctuation throughout.

Section titles. English: Verdict, Theme Identification, Selection Method Inference,
Weighting Inference. Chinese: 结论（Verdict）, 主题识别（Theme Identification）,
选股方法推测（Selection Method Inference）, 加权方法推测（Weighting Inference）.

I. Verdict
One clean paragraph, not a list, that describes these stocks in a form the user can forward
as-is to someone who would then build something similar. Write about the stocks, not about
an index: do not say "this index" and do not assume an index is the goal. Cover the theme
and sub-themes and the economic logic (what the stocks are trading and why), the kind of
company picked (region, size, liquidity, chain position, revenue drivers, business
quality), and how the weights look. Plain declarative sentences.
Include only rules you believe are in force and leave out anything you could not support.
No confidence levels, no "probably", no tested-versus-inferred labels, no mention of this
analysis, no ticker list. No risks or concerns anywhere: the user built or picked the
basket and has already done that thinking. After the paragraph add at most one line, and
only if a credible alternative reading of the theme survived your tests: that reading and
what would separate it. A reading your tests rejected is not mentioned.

II. Theme Identification
Each theme in a line with the main evidence it came from (filings, report, news event, ETF
label, co-movement cluster). Mark style overlays separately from themes. Then the exposure
grades, legend stated once, basis in brackets. Choose the layout that reads best: for a
small basket (up to about 15 names) one line per name with its grade on each theme; for a larger basket
group the names under each theme by grade (High names, then Medium, then Low), repeating a name
under every theme it belongs to. Skip the ones graded None.
Say which figures are disclosed and which are estimates. Then the co-movement result in one
or two lines, or say it was not run.

III. Selection Method Inference
Picking style within themes (pure-play, leaders, chain spread, tiering). Then the filters
on top, one item each, each opening with a grade word and a colon. English: Confirmed /
Fits / Suggested / Subjective (cannot be reverse-engineered) / Not found. Chinese: 证实 /
吻合 / 推测 / 主观（无法反推）/ 找不到. Grades mean what Step 6h says. Content is told apart
by the grade word, not by the numbers. State what the two-sided test showed for each
quantitative filter. A filter you are confident is not used is left out entirely, not
listed as ruled out. Include the sibling-basket partition if several baskets share a source.
State the point-in-time date used and the data source once.

IV. Weighting Inference
Only if weights were given. The pattern, then the driver-by-driver test from Step 5: only
the drivers that fit or that cannot be separated from each other, plus any that cannot be
tested. A driver you are confident is not used is left out. If nothing explains the
deviation, say so in one line.
Close the last section (III if there are no weights) with the usability result from Step 9:
one or two lines on whether the Verdict alone would reproduce the names and the weights,
what a builder would add, drop or weight differently, and what rule is missing.
