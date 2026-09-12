# Edge Finder — NIFTY Strategy Research Assistant (Prototype)

A small prototype of the ASK → CLARIFY → DEFINE → TEST → LEARN research workflow described
in the assignment. Given a plain-language trading question, it extracts a structured
experiment, flags what's missing instead of guessing, and runs it against a simulated
market so you can see the full reasoning loop end to end.

**Live demo:** [add your deployed link here]
**Repo:** [add your GitHub link here]

## Architecture

Single self-contained `index.html` (HTML/CSS/vanilla JS, no build step, no dependencies).
Everything runs client-side. The app is organized as a small state machine over five
stages that mirror the five sections of the assignment brief:

```
ASK ──► CLARIFY ──► DEFINE ──► TEST ──► LEARN
 │         │            │         │         │
 parse    fill in      show      run       show data vs.
 question missing      the       the       conclusion vs.
          fields       exact     backtest  next steps,
          w/ defaults  spec                separately
```

- **Extraction (`parseQuestion`)** — a small rule-based parser (regex over known patterns:
  instrument, % threshold, direction, volatility filter, holding period, exit type, test
  period). Anything it can't find is *not* silently defaulted — it's routed to the Clarify
  step and shown with a suggested default the user must confirm or change.
- **Clarify step** — every field is tagged either "from your question" or "suggested
  default — confirm", so the user always sees which parts were assumed. The tags carry the
  meaning in words, with colour as reinforcement rather than the only signal.
- **Backtest (`runBacktest`)** — generates a seeded synthetic daily price series (regime-
  switching volatility, small positive drift) and scans it for the entry condition,
  computing sample size, win rate, average return, a baseline (unconditional) average
  return for comparison, and the standard error of the sample. The seed is derived from
  the question + experiment parameters, so the same input always reproduces the same
  result.
- **Learn step** — results are split into three explicitly separate sections: *what the
  data shows* (objective numbers), *what we can reasonably conclude* (hedged, sample-size-
  aware interpretation), and *what to investigate next*. This split is the direct answer
  to the brief's instruction to distinguish what the data shows from what the system
  concludes from it.

## Why simulated data, and why that's labeled everywhere

The prototype does not use real NIFTY history. It generates a random-walk-style series
with realistic volatility clustering but **no engineered mean-reversion effect** — so a
"true" edge is not expected to show up by construction. This was a deliberate choice:
it means the Learn step's conclusion text is honest by design rather than a scripted
"yes it works" — the numbers actually vary run to run, and the interpretation text
branches based on sample size and whether the measured edge exceeds its own standard
error. Every screen that shows a result also shows a "SIMULATED DATA" flag so it's never
mistaken for a real market finding. Swapping in real historical data (see "What I'd
improve") would only require replacing the series generator — the rest of the pipeline
(extraction → clarify → stats → interpretation) is unchanged.

## Known limitation, surfaced rather than hidden

The entry model only recognizes single-day price moves. If a question describes a
multi-day pattern (e.g. "3 days in a row"), the app detects that language and shows an
explicit warning that it will test a single-day move instead of the streak, rather than
silently mis-modeling the question. This felt more honest than either crashing or quietly
answering a different question than the one asked.

## Technology choices

Plain HTML/CSS/JS, no framework, no build step.

**Why, given the brief suggests React/Next.js/TypeScript:** with a hard time limit, I
prioritized having a complete, correct, deployable ASK→LEARN loop over framework
scaffolding. A prototype this size doesn't need component state management — a single
state machine over five sections is easier to read as a reviewer, dependency-free to
deploy (drop the file anywhere), and has zero build-tooling risk. I see this as a
reasonable prototype-stage trade-off, not a claim that it's how I'd build the production
version — see "What I'd improve."

## Optional: LLM-based extraction

The Ask screen has a collapsed "Advanced" section to parse the question with an LLM
(OpenAI `gpt-4o-mini`) instead of the rule-based parser, if you paste in your own API key.
It's off by default, calls the provider directly from the browser (fine for a demo, not
how I'd ship this — see below), and falls back to the rule-based parser automatically if
the call fails for any reason, so the core demo never breaks. I kept the default path
rule-based rather than requiring a key, so the working prototype doesn't depend on any
external service or secret to run.

## Key decisions

- **Ask before assuming, everywhere.** Every field the parser can't find gets a visible
  clarify card with a suggested default, not a silent assumption. This was the single
  most important product requirement in the brief and it's the thing I optimized hardest
  for.
- **Separate "what the data shows" from "what we conclude."** These are rendered as
  physically separate blocks with different framing, on purpose — conflating them is
  exactly the kind of misleading-conclusion risk the brief calls out.
- **Reproducible, not theatrical, results.** The backtest is seeded from the actual
  question + parameters, so re-running the same question gives the same numbers, and
  different questions/thresholds give genuinely different (sometimes negative) edges —
  not a canned positive result.

## How to run

No install needed — open `index.html` in any browser.

To deploy: drag the file into [Netlify Drop](https://app.netlify.com/drop), or push it to
a GitHub repo and enable GitHub Pages (Settings → Pages → deploy from branch → root).

## What I'd improve with more time

- Swap the synthetic series for real NIFTY historical data (NSE Bhavcopy or a market data
  API), and re-validate that the Learn step's interpretation logic still reads sensibly
  on real, non-random data.
- Replace the standard-error heuristic with a proper significance test (bootstrap or
  Welch's t-test) and report a confidence interval, not just a point estimate.
- Rebuild in TypeScript + a component framework once the interaction model is proven —
  right now the state machine is simple enough that a framework would add ceremony
  without much benefit, but it would help once features grow (saved experiments,
  multiple instruments, comparing runs).
- Move any real LLM call server-side so an API key is never in the browser.
- Extend the extractor to handle multi-day/streak conditions properly instead of just
  flagging and falling back to a single-day model.
- Add persistence so a user can save past experiments and come back to them — the brief's
  larger vision explicitly includes "remember what it learned."

## AI tools used

See `AI_USAGE_NOTE.md`.
