# Authority model

## On-chain authority

Robinhood Testnet is canonical for:

- Wolf, Sheep, and Land ownership;
- NFT mint caps, traits, lineage, and Land coordinates;
- `$WOOL` balances, supply cap, minting, transfers, approvals, and burns;
- ERC-1155 durable resource balances;
- animal lifecycle activation, feeding horizon, Land assignment, production checkpoints, and breeding state;
- genesis sale revenue and liquidity creation;
- escrow marketplace settlement;
- shop purchases, treasury custody, and signed resource settlement.

## Server authority

The NestJS service is canonical for:

- position, movement validation, presence, and world ticks;
- gathering nodes and cooldowns;
- inventory used by server gameplay;
- crafting, health, stamina, medicine, hunts, and combat;
- guilds, chat, territories, leaderboards, moderation, and season jobs;
- deciding whether an off-chain action earned a signed settlement entitlement.

## Projection-only data

The database contains wallet-bound projections of on-chain NFTs for game UI and world access. It must never create ownership. Synchronization re-reads the configured contracts and deactivates rows no longer owned by the bound wallet.

The indexer is also a projection. It waits for confirmation depth and rewinds on reorg. A rebuild from chain events must be safe.

## Client trust

The Next.js/Unity client is untrusted for reward calculation. It may predict movement and format transaction calls, but both the server and contracts validate state transitions independently.
