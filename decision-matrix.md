# AI Solution Decision Matrix · Juno

## The decision

Whether RocketShip builds Automated Prioritization in Juno as a Hybrid (RAG + Agentic) Copilot, vs buying a generic LLM API or fine-tuning a model on our corpus.

Why now: roadmap discussions are driven by the loudest voice in Slack rather than customer evidence. Priorities reverse weekly, and the PM cannot defend the call to leadership.

## Scoring method

All criteria scored **1–5, higher is better**. Risk is scored as exposure-adjusted confidence, so **5 = lowest risk**.

Weights reflect what this particular decision turns on. Juno's output is a ranked backlog a PM must defend to leadership, so the ability to control and cite the ranking outweighs how fast or cheaply we can stand it up.

| Criterion | Weight | Why this weight |
|---|---|---|
| Control | 30% | The ranking must be inspectable and correctable, or the PM cannot defend it |
| Moat | 25% | Prioritization tied to our corpus is the defensible part; the model is not |
| Risk | 20% | A wrong ranking that looks authoritative is worse than no ranking |
| Speed | 15% | Matters, but the loudest-voice problem is structural, not urgent-this-sprint |
| Cost | 10% | Lowest weight — this is a core capability, not a peripheral feature |

## Options scored

| Option | Cost (10%) | Speed (15%) | Control (30%) | Moat (25%) | Risk (20%) | Weighted | Unweighted |
|---|---|---|---|---|---|---|---|
| **Build** | 2 | 2 | 5 | 5 | 4 | **4.05** | 3.60 |
| Buy / API | 5 | 5 | 2 | 1 | 2 | 2.50 | 3.00 |
| Fine-tune | 3 | 2 | 4 | 4 | 3 | 3.40 | 3.20 |

## Sensitivity

Build ranks first under **both** weighted and equal weighting, so the recommendation does not depend on the weights chosen. Weighting changes the margin, not the winner:

- Equal weights: Build leads the runner-up by 0.40
- Weighted: Build leads by 0.65, and leads Buy / API by 1.55

Weighting also reorders second place. Under equal weights Buy / API and Fine-tune are near-tied; once Control and Moat carry their real weight, Fine-tune separates clearly as the better fallback. If Build is rejected on cost or timeline, Fine-tune is the alternative — not Buy / API.

## Recommendation

**Build.** Control and Moat are the axes this decision turns on, and Build leads both outright.

A generic Buy / API is cheaper and faster, but it cannot cite RocketShip sources. An uncited ranking is just a confident opinion, which recreates the loudest-voice problem in a new interface — the failure mode we are trying to remove. Fine-tune is slower than we can wait and still needs the corpus Juno would retrieve live, so it pays the data cost without escaping the freshness problem.

Autonomy stays **Copilot**: Juno drafts the ranked backlog with citations; the PM approves before publish.
