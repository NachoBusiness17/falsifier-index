# BUILD SET AUDIT - per-contract grades (2026-10-14, sitting 2 of the wormhole-class pass)

Scope note: this sitting covered GstdBridgeRouter (full, previous verdict),
the jetton mint surface (full), TreasuryGold + SettlementRouter (guard
surfaces), and the cutover. DAOVoting/AgentRegistry/SovereignVault/
EcosystemTreasury/escrows remain READ-PENDING (~900 lines, next sittings).

## Per-contract grades

| Contract | Grade | Notes |
|---|---|---|
| SettlementMaster (v8) | PASS, coverage 12/12 | probes + soak + band + two-tier; the audited core |
| GstdBridgeRouter | **FAIL - EXCLUDED from build set** | 3 criticals (WORMHOLE_RED_VERDICT.md); source preserved, never deployed |
| GSTDJetton | PASS with notes | MintWorkerReward: 4 guard layers (authority, mintable flag, maxSupply, workerPoolMax 400M cap); bounced(InternalTransfer) restores supply on failed worker-JW deploy; MintInitialAllocation is one-time, owner-gated, locked by SetMintAuthority. NOTE: until authority locks, owner can mint to workerPoolMax - bounded, disclosed, acceptable at testnet; the mainnet checklist must include SetMintAuthority -> Settlement + FreezeMint |
| TreasuryGold | PASS with notes | owner-gated ops; convert ratio bounded 50-95%; WithdrawOperational bounded to balance - 1 TON reserve; receives the Settlement "deposit" flow (compatible) |
| SettlementRouter | READ-PENDING | guard surfaces look owner-centric; full pass owed |
| SovereignVault / EcosystemTreasury / DAOVoting / AgentRegistry / escrows | READ-PENDING | ~900 lines; next sittings |

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
