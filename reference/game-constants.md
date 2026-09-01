# Game constants

These values describe the current Rules V2 Solidity source and V5 deployment. Contract constants are authoritative where a value is enforced on-chain.

## Token and sale

- `$WOOL` hard cap: 1,000,000,000.
- Gameplay emission budget: 900,000,000 `$WOOL`.
- Genesis liquidity budget: 100,000,000 `$WOOL`.
- Genesis price: `0.0001 ETH` per NFT.
- Sale ETH split: 20% treasury, 80% liquidity.
- Liquidity issuance: 10,000 `$WOOL` per mint.
- V5 cap: 10,000 NFTs — 1,000 Wolves, 1,000 Lands, and 8,000 Sheep.

## Lifecycle

- Initial feed window: 3 days.
- Maximum feed-ahead horizon: 28 days.
- Sheep feed: 2 Grass/day.
- Wolf feed: 3 Meat/day.
- Auto-feed: 10 `$WOOL` burned per 7 days.
- Sheep production: 3 `$WOOL`/day.
- Claim theft probability: 20% of the complete claim.
- Hungry Wolf theft eligibility: none.
- Hunger death: disabled; hunger suspends economic actions.
- Randomness delay: 3 blocks.
- Randomness expiry: 200 blocks.

## Breeding source rules

- Base fee: 10 `$WOOL` transferred to treasury.
- Standard duration: 7 days.
- Speedup: burn 100 `$WOOL` per day removed.
- Maximum reduction: 6 days; minimum duration 1 day.
- Cooldown: 7 days after ready time.
- Parent maximum: 20 breeds each.
- Lamb theft probability: 10%.
- Secondary Sheep cap: 10,000.
- Current deployed end-to-end status: **blocked by entropy expiry**.

## Runtime

- Authoritative world target: 5 ticks/second.
- Marketplace default UI listing duration: 7 days; bounded to 1–30 days by the client.
- Indexer defaults: 5 confirmations, 3-second polling, 50-block reorg rewind.
