# Security and release risks

This is the release gate derived from current source, not a generic roadmap.

## Lifecycle V2 & Breeding release notes

1. `GreenwoodAnimalLifecycle` V2 is deployed on Robinhood Testnet (`0xF7578ADf443c6aF0b571731D9E69aa0C4a35cC2E`).
2. Breeding is wired in the in-game HUD (`AnimalLifecyclePanel`), featuring 7-day base duration, 100 `$WOOL`/day speedup burn (up to 6 days for 1-day min), 10 `$WOOL` treasury fee, parent cooldowns, and 10% lamb theft chance.
3. Next production milestones: replace testnet blockhash entropy with Chainlink VRF or dedicated validator beacon before Mainnet deployment.

## P0 — remove uncontrolled eligibility

`GenesisMinterV5.testnetMakeEligible` has no access control. Any account can mark arbitrary existing IDs eligible in that source. Remove it or restrict it to a time-bounded migration role, and deploy/retire the source so frozen lifecycle eligibility cannot be expanded publicly.

## P0 — protect economic signing

The settlement signer can authorize any resource ID 0–10 and amount up to the one-billion per-resource cap. Before shared economic use:

- isolate it from deployer/treasury keys;
- require PostgreSQL/Redis and fail closed;
- enforce per-action amount allowlists and daily/player/global issuance limits before signing;
- record every signed entitlement, including abandoned ones;
- monitor signer-versus-chain reconciliation;
- prepare tested rotation/revocation and incident pause procedures;
- consider threshold or policy-controlled signing for production.

`WORLD_RNG_SECRET` must also be non-default and rotated with a versioned fairness/audit policy.

## P1 — align code and UI truth

- Shop Health/Energy cards say 50% burn while the deployed configuration burns 100%.
- Some HUD text still advertises breeding even though the action is blocked.
- The root dashboard uses hand-authored NFT/sample economy claims that are not contract state.
- `game-core` Bandage inputs differ from the signed release recipe.
- `apps/unity/README.md` still describes older resource authority language.
- The active HUD exposes only Safe hunt and Bandage, while the server supports more actions.
- The swap client dynamically chooses the most ETH-funded V3/V4/V5 pool instead of using one explicitly governed active pool.

Either wire and test a feature or label it source-only. Never let a design library or sample card become product documentation.

## P1 — enforce access and durable boundaries

- The NFT “game gate” is only a client/HUD check; Socket.IO world join does not require `canPlay`.
- Round combat has no encounter start path and its loot mutates server inventory rather than using settlement.
- WebSocket territory and guild-vault handlers return success without durable mutations.
- Building is presentation-only/locked, not an NFT or settlement flow.
- The old community panel is not mounted on `/world`.

Server authorization must be the enforcement point for any gate or durable MMO action.

## P1 — NFT media and randomness

- All collection base URIs remain mutable.
- Live URIs were not proven to serve the 14,500 generated assets.
- Generated Land coordinates/traits must be reconciled with on-chain Land data.
- V5 selection and DNA use miner/sequencer-influenceable block values.
- Bred Sheep need a media/metadata policy.

Publish content-addressed assets, provenance, redundancy, and a freeze/governance plan. Replace sale/lifecycle randomness before meaningful value is at stake.

## P1 — contract review

Externally review:

- `$WOOL` partition accounting and legacy migration counters;
- custom pool bins, reserve accounting, loop bounds, fees, pricing, and slippage;
- marketplace stale approvals, ETH pull payments, fee changes, and transfer edge cases;
- shop purchase/fulfilment split and replay handling;
- settlement EIP-712 domain, nonces, signer roles, caps, and pause/version migration;
- lifecycle accrual checkpoints, transfers, Land epochs, Wolf selection gas, and emission exhaustion;
- role/admin delays, multisig ownership, and emergency pause scope.

No current test count is a substitute for an independent audit.

## Operational readiness

Before calling Greenwood a production MMO, demonstrate:

- load and soak tests at declared concurrent-player targets;
- multi-instance world/Redis behavior and shard ownership;
- database backup restore and point-in-time recovery;
- indexer rebuild and reorg drills;
- signer/admin/treasury rotation;
- transaction abandonment/retry UX;
- contract pause/unpause and replacement migration;
- moderation, appeals, and audit retention;
- on-chain daily supply/resource reconciliation;
- multi-day public playtests without manual database or contract repair.

## Secret hygiene

Never paste or commit a Privy app secret, settlement private key, deployer key, or treasury key. Any credential already exposed in chat or logs must be rotated; do not reuse it because the repository itself looks clean.
