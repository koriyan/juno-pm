# Juno PM — AI Copilot for RocketShip’s Product Org

> An AI Associate PM that turns Slack/Notion/Jira chaos into a prioritised top-3 risk list every morning.

_Kori Yan · AI PM Cohort · Sep 2026_

Repo: https://github.com/koriyan/juno-pm

This repo is my final project for the AI Product Management Certification — **Juno PM**. Each module’s artefact lives in its own folder; this README is the dashboard and the pitch.

---

## Module artefacts

### M1 · Prompting
- **System prompt** — [`01-prompting/system-prompt.md`](01-prompting/system-prompt.md)
- **Prototype** — https://juno-kori.replit.app

### M2 · Strategy
- **Decision matrix** — [`02-strategy/decision-matrix.md`](02-strategy/decision-matrix.md)
- **AI Strategy one-pager** — [`02-strategy/strategy-one-pager.md`](02-strategy/strategy-one-pager.md)

### M3 · RAG / AI PRD
- **AI PRD** — [`03-harness-prd/prd.md`](03-harness-prd/prd.md)

### M4 · AI-Native UX
- **AI user flow** — [`04-ai-ux/user-flow.md`](04-ai-ux/user-flow.md)
- **Trust-gap mitigations** — [`04-ai-ux/trust-gaps.md`](04-ai-ux/trust-gaps.md)

### M5 · Agentic Workflows
- **Agent Workflow Spec (AWSpec)** — [`05-agentic-workflows/awspec.md`](05-agentic-workflows/awspec.md)
- **Agent Control Panel** — [`05-agentic-workflows/agent-control-panel.md`](05-agentic-workflows/agent-control-panel.md)

### M6 · Evals &amp; Guardrails
- **Eval stack** — [`06-evals/eval-stack.md`](06-evals/eval-stack.md)
- **Human evaluation rubric** — [`06-evals/human-rubric.md`](06-evals/human-rubric.md)

---

## PM Execution Plan

### Where Juno is today
- M1–M6 specced and committed.
- The prototype validates the M1 flow with the team.
- Automated evals: 200-item golden set drafted, judge prompt validated against 30 items; not yet wired to CI.
- Human rubric drafted; 2 grader candidates lined up; no calibration round yet.

### What ships next (next 2 sprints)
- Sprint 1: wire the eval harness to CI; staff and calibrate 2 graders; ship the Slack triage tool.
- Sprint 2: open closed beta with 3 PMs (1 RocketShip, 2 customers); weekly rubric review; instrument abandon-rate.

### What I watch (dashboards)
- Daily: thumbs-down rate, regen rate, hand-off rate.
- Weekly: human-rubric mean per dimension; refusal hit-rate; cost per run.
- Per release: golden-set accuracy; format/citation/refusal pass rate.

### Red lines (what blocks shipping)
- Any critical-safety fail (any "1" on safety dimension in human eval).
- <90% golden-set accuracy on automated layer.
- Customer-name fabrication in last 30 days.
- Cost >$0.50 per run.
- P99 latency >5s on triage flow.

### Governance
- Compliance: PII scrubber pre-LLM; GDPR DSR handler in /docs/dsr-runbook.md.
- Safety: prompt-injection eval row in golden set; refusal on legal/contract content.
- Reliability: 99.5% SLO; cached top-3 fallback if model is down.
- Reputation: 2-hour incident-response playbook in /docs; canary deploys for every model swap.

---

## Build Insights

- **Friction point.** Not the model — the same judgement written in three places. "Is this insight ranked?" was decided independently by the insight cards, the PRD generator and the eval panel, and when retrieval failed they disagreed: the cards said UNRANKED, the PRD printed an authored P1/P2 fallback directly underneath a sentence saying no priority was proposed, and the eval layer failed all three for not citing anything. Most of my debugging was reconciling surfaces that each held a private opinion about state, not improving output quality.
- **Key learning.** The gaps are better output than the scores. Two rubric dimensions cannot be evidenced by this build — accuracy needs a golden answer it cannot load, actionability tops out at 3 because there is no owner or ETA field — and marking them un-scoreable surfaced the two most useful findings in the whole evaluation. The same rule runs upstream: the release gate blocks on PII not because PII was found, but because nothing measures it.
- **Aha moment.** Confidence stopped being a label and the autonomy dial fell out of it. Once the score was a real weighted function I could no longer set autonomy by hand — the band had to be derived from the number. The sharper half: evidence strength and sample size are different axes. "I am sure what this person meant" is not "I am sure anyone else means it," so every single-interview finding takes a haircut, and what rescues an n=1 is corroboration. One interview plus 65 support tickets clears the auto bar; the same interview alone does not.

---


_Certification submission — AI Product Management Certification._
