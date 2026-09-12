# AI Usage Note

**1. Which AI tools did you use?**

Claude (Anthropic) — both the chat interface and Claude Code. I worked under a tight time
budget, so I used AI to produce the first working draft and spent my own time on the
decisions, the review, and verifying that the thing actually behaves as described.

**2. What did you use them for?**

- The first full draft of `index.html` — the five-stage flow, the rule-based extraction
  patterns, the synthetic price series, and the summary statistics.
- Drafting the README and the structure of the Thinking Note, which I then edited.
- Acting as a reviewer on my own changes: when I changed one thing, asking what else that
  change invalidated.

**3. Which important decisions did you make yourself?**

- **Choosing Option 2 over Option 1.** The evaluation weights put 65% on critical thinking,
  original thinking, and problem solving, and only 15% on implementation. Option 2 rewards
  reasoning clearly about an ambiguous question, which is where I wanted my limited time to go.
- **Simulated data with no edge baked into it.** The price series is a random walk with
  volatility clustering and no engineered mean-reversion. A generator tuned to produce a
  satisfying "yes, it works" would have made a better-looking demo and a dishonest one. The
  consequence is visible in the output: the default question returns a −0.10% edge against a
  0.23% standard error, and the app says plainly that this is indistinguishable from noise.
- **Ask rather than assume, and make the assumptions editable.** Every parameter the parser
  can't find becomes a visible card with a suggested default the user can change, tagged so
  it's obvious which values came from the question and which are mine.
- **Separating "what the data shows" from "what we conclude."** Three distinct blocks, not
  one paragraph. Conflating the measurement with the interpretation is the specific failure
  mode this kind of tool invites.
- **Default holding period of 3 days, not 5.** If a bounce after a sharp fall exists, it
  should show up quickly; a longer window mostly adds unrelated drift and dilutes the effect
  I'm trying to measure.

**4. Did you reject or modify any AI-generated suggestions? Why?**

Yes — and one of them was caught by making a change that looked purely cosmetic. I recoloured
the accent from teal to blue, which silently falsified the Clarify screen's instruction
("Green = read directly from your question"). Rather than just swapping the word green for
blue, I rewrote the line to refer to the tag text on each card instead of its colour, so the
meaning no longer depends on colour at all — which is also better for anyone who can't
distinguish the two. I also changed the default holding period and the wording of the Learn
step's closing sentence, which had drifted toward reassuring the user rather than telling them
what the run actually established.

The thing I deliberately did not accept was the suggestion to add more features. The brief
says "build less, think more," so I kept the surface small and spent the time on whether the
output is honest.

**5. What part of the solution are you most proud of?**

The conclusion text branches on the evidence rather than on a script. If the sample is too
small it says so; if the measured edge is inside one standard error it calls it noise; only
past that does it describe the result as worth investigating — and even then it refuses to
call simulated data a finding. It would have been easier to print a number and let the user
draw their own flattering conclusion.
