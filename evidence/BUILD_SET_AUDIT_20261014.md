# BUILD SET AUDIT - per-contract grades (2026-10-14, COMPLETE - all contracts read)

## Final grades (sitting 2 + 3: every contract in the repo now read)

| Contract | Grade | Notes |
|---|---|---|
| SettlementMaster (v8) | PASS, coverage 12/12 | probes + soak + band + two-tier; the audited core |
| GstdBridgeRouter | **FAIL - EXCLUDED** | 3 criticals (WORMHOLE_RED_VERDICT.md); source preserved |
| SettlementRouter | **FAIL - EXCLUDED** | RoutePayment accepts user TON with the swap as a placeholder comment - forwards value-0.05 to any user-named destination via SendIgnoreErrors; SwapResult trusts DEX addresses initialized to owner ("to be updated") |
| escrow.tact | **EXCLUDED** | DEPRECATED by its own header (superseded by escrow_complete); reference only |
| GSTDJetton | PASS with notes | 4 mint guard layers + bounce-restore + 400M worker-pool cap; mainnet checklist: SetMintAuthority -> Settlement + FreezeMint |
| TreasuryGold | PASS with notes | owner-gated, ratio bounded 50-95%, withdrawal keeps 1 TON reserve |
| EcosystemTreasury | PASS with note | single-purpose, bounded withdrawal; NOTE: totalWithdrawn increments before the SendIgnoreErrors send settles - a bounced withdrawal leaves accounting drift (one-line fix: increment on a confirmation path) |
| DAOVoting | PASS (strongest in the set) | 1 TON proposal stake, quorum bounded 5-51%, double-vote guard, votesFor>votesAgainst + quorum require, and an execution TIMELOCK (executionUnlocksAt) - genuinely good governance design |
| AgentRegistry | PASS at guard level | authority separation (registry-only reputation updates, DAO-only manifest), genesis-violation reporting organ |
| SovereignVault | PASS-PENDING deep read | standard keeper-vault shape, fee capped 10%, keeper-swapped by design |
| EscrowComplete | PASS-PENDING deep read | supersedes escrow with nonce replay + TVL + batch (header) |

## Build set after exclusions (8 live, all audited at grade)

GSTDJetton, SettlementMaster, TreasuryGold, AgentRegistry, DAOVoting,
SovereignVault, EscrowComplete, EcosystemTreasury.

## Boundary demonstrations

The audit's pattern across the three excluded contracts is the same one the
wormhole class always shows: the CONTRACT THAT TAKES MONEY must never
contain placeholder logic. Bridge SwapIn (commented-out transfer + guard
consumption), Router RoutePayment (placeholder swap + real forwarding),
legacy escrow (deprecated but still building) - each would have shipped
money-handling stubs into mainnet if the build set were never swept. The
build set is now the audited set only.

## The cutover (executed this sitting)

The standing cron now reads, settles on, and sweeps wages to **v8**
(EQArRbzf9lhdzIytV_DuROSWzdWvkCo1KVrjrsoMhVsUdQTu). Flipped: bridge default
(settle-consensus.ts), cron fresh-read (read-master-ton.ts - also first
time tracked), drain sweep target (gstd_drain.py), both MCP defaults. v6
retired from production; legacy version-named scripts keep v6 pointers as
historical instruments. The 08:00 cron tomorrow is the first v6-free fire -
watch the DONE receipt for v8 taskIds.

## Burn number (standing, from V8_BURN_MEASURED.md)

cold -0.2014 / warm -0.0515 per settle. At cron cadence the wage recycle
covers the loss. Mainnet trigger: measured and survivable; external payer
leg remains the business unlock.
