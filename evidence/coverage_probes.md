# COVERAGE PROBES - the contract surface is fully covered (2026-10-14)

The soak receipt originally carried 3 uncovered coverage rows (9 of 12
checkable). All three are now CLOSED with ABI-verified on-chain exit codes
against live v7 (auth-probes.ts, committed to the PR branch):

- Negative auth: SetMinMasterBalance from a NON-owner wallet ->
  exit 63399 "Only DAO". The gate holds for strangers.
- Floor minimum: owner sets floor=0.1 TON (below the 0.5 require) ->
  exit 29560 "Floor below relay reserve". The setter enforces its own bound.
- Replay guard: SettleTask replaying settled taskId 53001282225 ->
  exit 54291 "Task already settled". Double-pay is dead.

State untouched through all three probes: taskCount 30, floor 1.0 TON,
verdicts read from the chain's transaction list, never from the client.

Coverage on the contract surface: 12 of 12 checkable, all LINKED.
Remaining evidence work is the linked-logs class (council dispute-set run,
fleet cost comparison, hostile-corpus lamp run) - named in the index.
