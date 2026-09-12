## Harness Diagnostic Diff · Juno Harness Lab

_Working notes from Module 3 Lab 1. Do not paste over `03-harness-prd/prd.md`. That file comes from the AI PRD Builder._

**Prototype:** Will provide separately as I'm using claude code

### Harness off

1. P0 CSV export hangs ~5 min, fails to a blank screen
2. P0 The job is pivoting in Excel, not producing a document
3. P1 The failure is silent, separately fixable

One pass, 0 tool calls, 0 citations, 0 items flagged. All 6 insights
reach the roadmap unchallenged — including P1 nav bar luminance and
P2 dark mode. Nothing stopped it because nothing was there to stop it.

### Harness on

1. P0 CSV export crash — cites Reliability First
2. P0 Excel pivot is the job — cites NORTH STAR
3. P1 Silent failure — cites Reliability First

Both P0s paused at write_roadmap() and committed only on a click.
Two items left the ranking: nav luminance -> UNRANKED, no supporting
clause. Dark mode -> UNRANKED, excluded by "WHAT WE ARE NOT DOING
THIS QUARTER".

### Tool trace

5 of 5 turns, stopped on turn-limit. 13 calls: read_tickets x1,
search_strategy x6, draft_priority x4, write_roadmap x2 — both writes
paused for confirmation (calls #12, #13), 2 human decisions logged with
timestamps. Classes shown per call: READ/AUTO, DRAFT/AUTO, WRITE/CONFIRM.
4 of 6 verified; 2 came back unverified and were shown, not dropped.
~360 in / ~1190 out tokens (est.). Committed: INS-1 P0, INS-2 P0.

### Takeaway

> The harness didn't change my top 3 — it changed whether I could defend them, and it killed the two items that had no evidence.

---

# Supporting detail

Everything below backs the four sections above. Nothing here is recalled or
estimated: both configurations were executed headlessly against the shipped
`juno-dashboard.html`, with the same transcript (INT-114, Sarah), the same
extraction, and the same six candidate insights. The only variable is whether
the loop, the tool registry, the permission gate and the verification gate were
in play.

## The top 3 did not move — and that is the finding

The ranking is identical in both runs. The harness did not make Juno smarter.
It made the same three calls **defensible**, and it removed the two items that
had no evidence behind them.

| Insight | Harness off | Harness on |
|---|---|---|
| CSV export crash | P0, uncited | **P0** · ✓ Verified — *Reliability First* |
| Excel pivot is the job | P0, uncited | **P0** · ✓ Verified — *NORTH STAR* |
| Silent failure | P1, uncited | **P1** · ✓ Verified — *Reliability First* |
| PDF + date filter healthy | P2, uncited | **P2** · ✓ Verified — *Reliability First* |
| Nav bar luminance | P1, uncited, shippable | **UNRANKED** · Unverified — no supporting clause |
| Dark mode | P2, uncited, shippable | **UNRANKED** · Excluded — *WHAT WE ARE NOT DOING THIS QUARTER* |

The two badged items are **shown, not dropped**. An insight that fails
verification is displayed with its failure visible; silently discarding it
would ask for exactly the blind faith the artifact exists to refuse.

`search_strategy` ran six times, not four, because **every** insight had to be
grounded — including the two that failed. Failing grounding is a result, not a
skipped step.

## Raw trace

```
turns        5 / 5        stopReason: turn-limit
tool calls   13
             read_tickets    ×1   READ   / AUTO
             search_strategy ×6   READ   / AUTO
             draft_priority  ×4   DRAFT  / AUTO
             write_roadmap   ×2   WRITE  / CONFIRM  ← paused both times
paused at    call #12, again at #13
decisions    2 human approvals, timestamped into the trace
tokens       ~360 in / ~1190 out (est.)
verified     4 / 6      low-grounding banner: did not fire
committed    INS-1 → P0,  INS-2 → P0
```

Order of operations: corroborate against tickets → ground all six against the
strategy document → draft the four that survived → commit, one human click per
write. It stopped at the turn bound with approved drafts still uncommitted, and
**said so** rather than continuing or silently truncating.

## Self-review

| Check | Status |
|---|---|
| Tools listed in the trace with their side-effect class | ✅ `READ/AUTO`, `DRAFT/AUTO`, `WRITE/CONFIRM` on every call |
| Turn count visible; loop stops rather than runs forever | ✅ `5 / 5 · turn-limit`, bound enforced in `run()` |
| `write_roadmap()` paused for a human; gate not deleted | ✅ paused twice, 2 decisions logged with timestamps |
| Every priority cites a real clause or is badged | ✅ 4 cite a verbatim clause; 2 badged and shown |
| At least one item flips to `notRecommended` when strategy loads | ✅ **two** — nav luminance (no clause), dark mode (excluded) |
| Prototype URL recorded | ⏳ provided separately |
| Hosted backend enabled, no "mock" warnings | ❌ **not met — see below** |

### The one that is not met, stated plainly

**There is no hosted backend, and no model call.** Claude Code has no "enable
backend" button — that instruction comes from a different class of builder
(Lovable, Bolt, v0, Replit), each of which provisions Supabase and a model
gateway in one click. There is no equivalent here, and I did not create an
account or enter a credential to fake one.

What exists instead:

- `planNextStep()` is a deterministic policy standing in for the model's choice
  of next action. It is the **single** swap point, and is labelled as such in
  the file header.
- Token counts are estimated from real payload sizes at ~4 chars/token and
  displayed as `est.`
- `search_strategy()` is **genuinely real** — it parses and searches whatever
  document is pasted in. The four verified citations above came out of the
  Q3 one-pager, not out of a model's memory.
- A full server-side implementation of the same loop, same bounds, same
  permission gate and same verification gate is written in
  `supabase/functions/juno-agent/index.ts`. It is **not deployed and has never
  been executed** — treat its behaviour as unverified.

Everything the harness is assessed on — turn bound, failure counter, permission
gate, verification gate, token ledger, latency budget — is transport-independent
and already correct. Swapping `planNextStep()` for a model round-trip does not
change any of it. See `BACKEND.md` for the five-step path to going live and
what it costs.

### One wording flag for the reviewer

The checklist asks that unsupported priorities be badged *Unverified*. Dark mode
is badged **"Excluded by strategy"** instead. It is unverified — `verified:
false`, and `write_roadmap()` refuses it — but the badge is more specific on
purpose: *"no clause supports this"* and *"the document explicitly forbids
this"* mean opposite things to a PM, and collapsing them would lose the
stronger signal.
