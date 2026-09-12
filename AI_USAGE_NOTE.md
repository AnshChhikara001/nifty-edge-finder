# AI Usage Note

> Fill this in honestly based on what you actually did. Don't submit claims about
> decisions or edits you didn't really make — this note is itself being evaluated for
> how clearly you can describe your own process, and reviewers may ask you to defend
> any of it. Before submitting, make at least a couple of real, small changes to the
> code or copy yourself (a default value, a color, a wording choice, an extra example
> question) so this note describes something true.

**1. Which AI tools did you use?**
Claude (Anthropic), via [claude.ai chat / Claude Code — say which]. Used within a tight
time limit, so most of the first draft came from AI and I focused my own time on review,
verification, and decisions.

**2. What did you use them for?**
- Drafting the initial HTML/CSS/JS for the ASK→CLARIFY→DEFINE→TEST→LEARN flow
- Drafting the rule-based extraction patterns and the synthetic backtest logic
- Drafting this README and the Thinking Note structure
- [add/remove anything that doesn't match what you actually did]

**3. Which important decisions did you make yourself?**
[This is the part to make genuinely yours. Examples of the kind of decision to name —
only keep the ones you actually made:]
- Choosing Option 2 over Option 1, and why
- Choosing to use simulated random-walk data with *no* built-in mean-reversion, so the
  tool can't produce a falsely convincing "it works" result
- The specific clarifying questions to ask (threshold, holding period, exit, test period,
  cost assumption, volatility filter) and which ones to default vs. force the user to set
- Splitting the result into "what the data shows" vs. "what we conclude" vs. "what to
  investigate next" as three distinct blocks
- [your own additions/edits to the code, copy, or design]

**4. Did you reject or modify any AI-generated suggestions? Why?**
[Be specific and honest. If you didn't reject anything, say what you changed instead —
wording, a default value, a color, a piece of logic you checked and adjusted.]

**5. What part of the solution are you most proud of?**
[Answer for real — e.g. the honesty of the Learn step's conclusion text, the streak-
detection warning, the fact that the backtest is seeded and reproducible, etc.]
