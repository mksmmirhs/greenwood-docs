# Current status

Source and Robinhood Testnet audit: **2026-09-01**, around block `111,186,961`.

## Deployed economy

The configured V5/Rules V2 contracts responded on chain during this audit. The lifecycle reported Rules Version 2, three frozen eligibility sources, and the V5 minter reported a frozen lifecycle address. The shop reported all five items available with 100% burn configuration. The marketplace fee was 3%.

The chain snapshot is evidence that the addresses exist, not a promise that the counters below remain current. At the audit point the V5 cumulative migration counters reported 26 paid mints: 1 Wolf, 3 Lands, and 22 Sheep. `$WOOL` total supply was about `710,003.054745`, of which about `450,003.054745` was recorded as gameplay issuance.

## Current feature matrix

| System | Current source path | Status | Important boundary |
|---|---|---:|---|
| Privy login | Next.js and API token verification | Wired | Requires correct app ID/public key; the app secret remains server-only. |
| Wallet binding | EIP-191 five-minute challenge | Wired | The bound wallet, not a request-body address, controls server settlement rewards. |
| Unity world | `/world` + Unity 6 WebGL | Wired | World join currently does not enforce the NFT access pass server-side. |
| Movement | Unity → Socket.IO → 5 Hz server snapshots | Wired | Server position, not chain state. |
| Gather | Grass/Timber/Stone/Herbs nodes | Wired | Each accepted action returns a signed +3 ERC-1155 settlement that requires a wallet transaction. |
| Craft | Bandage and Plank settlement recipes | Partly wired | Bandage is exposed in the active HUD; Plank is accepted by the server but has no active Unity HUD button. |
| Hunt | Four server tiers | Partly wired | The active HUD exposes Safe only. Rewarded hunts settle Meat/Hide on-chain; health/energy change server-side before reward confirmation. |
| Round combat | rules, service, protocol, Unity presentation | Source-only | Nothing calls `startEncounter`, so no reachable encounter begins in the active route. Combat loot is also server inventory, not a durable settlement. |
| Vitals | health/energy regen, Bandage, shop refills | Wired | Vitals are server state. Medicine requires a confirmed Bandage burn. |
| Genesis V5 | single/batch 1–20 random mint | Deployed and wired | `0.0001 ETH` each; cumulative V3–V5 distribution caps are 1k Wolf, 1k Land, 8k Sheep. |
| Protocol pool | custom discrete-bin `$WOOL`/ETH pool | Deployed and wired | Custom unaudited pool. The web selects the most ETH-funded V3/V4/V5 pool for swaps, while new mints fund V5. |
| Animal setup | activate legacy asset, assign to Land | Deployed and wired | V5 animals auto-activate but still need Land assignment for Sheep production. |
| Feeding | Grass/Meat burn and `$WOOL` auto-feed | Deployed and wired | Hunger suspends economics; it does not kill the NFT. |
| Sheep claim | request future block, then resolve | Deployed and wired | Up to five Sheep are processed sequentially, not in one batch contract call. A full claim may be stolen 20% of the time. |
| Sheep breeding | lifecycle V2 + Breeding House UI | Deployed and wired | 10 $WOOL treasury fee, 7-day base duration with up to 6 days speedup (100 $WOOL/day burned), 10% lamb theft chance to fed Wolf. |
| Marketplace | approval-based ETH/`$WOOL` listings | Deployed and wired | The NFT stays in the seller wallet until purchase; a revoked approval or transfer makes the listing stale. |
| Shop | on-chain burn + server fulfilment | Deployed and wired | Every current item burns 100%; stale UI text still says 50% for the two vital items. |
| Guild/chat/territory | HTTP API + PostgreSQL | Backend-only | The active Unity page does not mount `CommunityPanel`. WebSocket territory/vault handlers only return success text. |
| Seasons/jobs | PostgreSQL worker | Backend-only | Territory expiry and 90-day rollover exist; reward distribution is not implemented. |
| Indexer | confirmed log journal | Backend-only | Stores decoded logs/cursor and rewinds on reorg; it does not build full materialized ownership/balance projections. |
| NFT assets | local generation/validation | Generated locally | 14,500 images/metadata exist, but deployed URIs point to `api.wolf.game` and all three base URIs remain unfrozen. |

## Animal Lifecycle V2

The V2 animal lifecycle contract (`0xF7578ADf443c6aF0b571731D9E69aa0C4a35cC2E`) handles:
- **Activation & Multi-Land Assignment:** Animals can be assigned or re-assigned across any owned Land parcels.
- **Feeding & Auto-Feed:** Burn Grass (Sheep) / Meat (Wolves) or burn 10 `$WOOL` per 7 days of auto-feed.
- **Wool Accrual:** 3 `$WOOL` per fed, assigned Sheep per day with 20% theft probability to fed Wolves.
- **Breeding House:** 2 eligible, fed parent Sheep on the same Land can breed for 10 `$WOOL` treasury fee with configurable 0–6 days duration speedup (100 `$WOOL`/day burned). 7-day cooldown applies after completion. Lamb has a 10% theft probability to a fed Wolf owner.

## Other release gaps

- `GenesisMinterV5.testnetMakeEligible` is public and can mark arbitrary existing token IDs eligible.
- V5 random selection uses block variables and is unsuitable for mainnet-value randomness.
- The active NFT gate is client-side presentation; a signed-in user without NFTs can still join and move in the server world.
- The root dashboard and old `world-game.tsx` contain showcase or alternative-client behavior that is not active at `/world`.
- Local fallback secrets and in-memory persistence are allowed in development and must fail closed in a shared economy.
- Contracts, custom pool math, settlement authority, and media delivery have not been externally audited.
