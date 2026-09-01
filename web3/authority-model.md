# Authority model

Greenwood is not “everything on-chain.” It places durable ownership and economic state on Robinhood Testnet while keeping real-time play on the server.

## Contract authority

Robinhood Testnet is canonical for:

- Wolf, Sheep, and Land token ownership;
- collection-level token IDs, traits, lineage, Land coordinates, and caps;
- the V3–V5 paid-mint eligibility records used by Rules V2;
- `$WOOL` balances, approvals, burns, supply, and issuance budgets;
- ERC-1155 resource balances and per-resource caps;
- animal activation, feed horizon, Land assignment, breeding flags, and Sheep claim checkpoints;
- genesis ETH routing and protocol-liquidity issuance;
- custom pool reserves, bins, quotes, and swaps;
- marketplace listings, purchases, fees, and ETH withdrawal credits;
- shop payments and burn amounts;
- settlement nonces and exact resource inputs/outputs finalized from a valid signer.

## Server authority

NestJS is canonical for:

- authenticated world sessions and current positions;
- movement sequence acceptance, node range, cooldowns, and respawns;
- server health, energy, XP, and regeneration;
- hunt randomness/outcomes and whether a resource settlement may be issued;
- shop and medicine fulfilment after receipt verification;
- guilds, membership, chat, territories, leaderboard scores, reports, moderation, and seasons;
- settlement signatures for allowed release actions.

The settlement signer is a centralized economic authority. Resource caps and player confirmation constrain it, but a compromised signer can authorize arbitrary allowed ERC-1155 IDs and amounts up to those caps. Key isolation, rate limits, monitoring, rotation, and reconciliation are mandatory.

## Client responsibility

Next.js and Unity are untrusted presentation/input clients. They may:

- request an action;
- display predicted or fetched state;
- build a contract transaction;
- ask the wallet to sign or submit;
- wait for a receipt.

They may not decide a reward, claim ownership, or treat a submitted transaction as successful before confirmation.

## Projection data

The chain roster service reads ERC-721Enumerable ownership and lifecycle eligibility directly from RPC, then stores wallet-bound rows for the HUD. Those rows are projections; a later synchronization deactivates assets no longer owned.

The separate indexer journals confirmed logs into `chain_events`, persists a cursor, and rewinds on a detected reorg. It currently does not build complete ownership, balance, or lifecycle materialized views. Contract reads remain necessary.

## Important split-state effects

- A hunt may spend server energy even if the player declines the on-chain reward transaction.
- A shop vital item burns `$WOOL` on-chain, but the refill is server-side and requires API fulfilment.
- A resource crate purchase needs a second signed-settlement transaction after the purchase transaction.
- An NFT transfer changes chain ownership immediately; the HUD database may remain stale until roster synchronization.
- Hunger can stop chain production while server player health remains 100, or vice versa.
