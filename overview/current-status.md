# Current status

Last source and deployment audit: **2026-09-01**.

## Live on Robinhood Testnet

- Chain ID `46630` and the official testnet RPC are configured.
- V5 genesis mint costs `0.0001 ETH` per random NFT.
- Each mint routes 20% of ETH to the team treasury and 80% to protocol liquidity.
- Each mint creates `10,000 WOOL` inside the protocol pool, up to the 100 million genesis-liquidity budget.
- Wolf, Sheep, Land, resource, `$WOOL`, marketplace, shop, settlement, treasury, liquidity, mint, and animal-lifecycle contracts are deployed.
- New V5 Wolf and Sheep mints are registered in the lifecycle contract.
- Animal activation, Land assignment, Grass/Meat feeding, `$WOOL` auto-feeding, and two-step Sheep yield claims are available.
- Hungry animals remain NFTs. Hunger suspends economic actions; it does not kill or burn the NFT under current Rules V2.
- The web client reads the wallet's eligible assets and current lifecycle state.

## Server-backed playable systems

- Privy token verification and wallet challenge binding.
- PostgreSQL persistence and Redis-backed presence/cooldown support.
- A 5 Hz authoritative world loop with join, movement, gathering, crafting, hunt, and combat messages.
- Health, stamina, medicine preparation, and chain-receipt-based claims.
- Guild, territory, chat, leaderboard, moderation, and season endpoints.
- A chain indexer with confirmation depth, cursor persistence, and reorg rewind.

These features are meaningful implementation, but the social, seasonal, moderation, indexing, and operational layers remain **prototype** quality.

## Known blockers

### Breeding randomness expires before breeding completes

The deployed lifecycle stores an entropy block only three blocks after `requestBreed`, but `resolveBreed` waits one to seven days. Entropy becomes invalid after 200 blocks. On this chain, the entropy expires before the breeding timer completes, so resolution reverts. Cancellation also fails to clear the parents' `isBreeding` state.

Required correction:

1. Choose the entropy block near `readyAt`, or request randomness after the timer completes.
2. Make cancellation restore both parents to a valid non-breeding state.
3. Remove the nested `nonReentrant` two-argument overload.
4. Add tests that advance both timestamp and block number realistically.
5. Redeploy and update the address manifest and clients.

### Testnet eligibility is publicly writable

`GenesisMinterV5.testnetMakeEligible` has no access control. Any account can mark arbitrary existing NFT IDs as lifecycle-eligible. This is unacceptable outside an isolated testnet rehearsal and should be owner-only or removed before another release.

### Breeding approval UX needs completion

Lifecycle auto-feed and breeding call `burnFrom`/`transferFrom`, so the player must approve the lifecycle contract to spend `$WOOL`. Auto-feed now checks allowance and requests approval when required. The corrected breeding flow must do the same before breeding is re-enabled.

## Not yet a production MMO

The repository does not yet demonstrate production sharding, 1,000 concurrent players, sponsored transactions, hardened anti-cheat, immutable art hosting, audited contracts, operational disaster recovery, or a fully populated four-biome world. Those remain release work, not current claims.
