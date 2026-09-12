# Thinking Note
**Question: "Does buying NIFTY after a sharp fall work?"**

## 1. Interpreting the question

"Sharp fall" is doing a lot of work in this sentence and isn't defined. It could mean:
- A single day's close-to-close decline past some threshold (e.g. ≥1%)
- A multi-day decline (e.g. down 3 days in a row, or down 3% over a week)
- A move relative to normal volatility, rather than a fixed percentage (e.g. a 2-standard-deviation day is "sharp" in a calm market but ordinary in a volatile one)

I'd default to the simplest, most common reading — a single day's close-to-close fall past a fixed percentage threshold — because it's the easiest to test and explain, and it matches how most retail traders use the phrase. But I'd treat that as *my* assumption, not the user's intent, and say so.

Before this becomes a real experiment I'd need: the exact fall threshold, whether it's daily or multi-day, the entry timing (same close vs. next open), an exit rule, a holding period, the test period, and what "work" means — a positive average return, a positive *risk-adjusted* return, or just a high win rate. Those aren't the same question.

## 2. Assumptions

**What the user actually said:** NIFTY, a "sharp fall," and whether buying afterward "works."

**What I assumed (and why):**
- Fall = daily close-to-close decline ≥ 1% — the most common convention, chosen for testability, not because it's obviously correct.
- Entry = next day, since you can't act on the same close that defined the signal.
- Holding period = 3 trading days, as a reasonable short-term default when none is given — short enough that a bounce, if one exists, hasn't been diluted by unrelated drift.
- "Works" = a positive average forward return, net of costs, that's meaningfully different from a random day — not just a positive raw return.

**What the system should ask instead of assuming:** the fall threshold, the holding period, any exit rule beyond time, the test period, and whether to filter by conditions like volatility regime. These change the answer enough that guessing isn't a fair substitute for asking.

## 3. What I'd ask the user

- What size move counts as "sharp" — a percentage, or something relative to recent volatility?
- How long do you hold before exiting?
- Is there an exit rule besides time (stop-loss, target)?
- What period should we test — the last 5 years, 10 years, a specific regime?
- Should we control for anything else, like only testing during high-volatility periods?

These five cover the parameters that most change the conclusion. Anything narrower (exact commission rate, specific data vendor) can default sensibly without changing what the experiment is testing.

## 4. What the experiment looks like

```
MARKET            NIFTY
CONDITION         Daily close-to-close fall ≥ threshold%
ENTRY             Next trading day
EXIT              Time-based (holding-period days), unless a
                  stop-loss/target is specified
HOLDING PERIOD    N trading days (user-set, default 3)
TEST PERIOD       Last N years of daily data
FILTERS           Optional — e.g. high-volatility days only
COST ASSUMPTIONS  Round-trip cost (brokerage + STT + slippage
                  estimate), applied to every trade
HYPOTHESIS        The forward return after this condition,
                  net of costs, differs meaningfully from an
                  average day's forward return.
```

## 5. What could go wrong

- **Threshold sensitivity** — a "1% fall" and a "2% fall" can tell completely different stories from the same data; the conclusion shouldn't hinge on one arbitrary cutoff.
- **Small sample size** — sharp falls are rare events; a handful of occurrences can look like an edge or a loss purely by chance.
- **Overfitting** — tuning the threshold, holding period, or filters until something "works" is finding noise, not an edge, especially with one dataset.
- **Look-ahead bias** — using price data that's been adjusted with information not available on the entry date overstates results.
- **Ignoring costs and slippage** — a short holding period strategy that looks profitable gross of costs can be a loser net of them.
- **Regime dependence** — an edge measured in one 5-year window (say, a recovery period) may not hold in a different market regime.
- **Mistaking one backtest for proof** — a single historical run is one sample from a noisy process, not a verified law; it needs out-of-sample or cross-market validation before anyone should act on it.
- **Conflating "the data shows X" with "the system concludes X"** — these are different claims, and blurring them is the fastest way to mislead a user who trusts the output.
