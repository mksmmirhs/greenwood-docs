# Release strategy

The current code has moved to a **server-authorized, player-finalized** economy.

## 1. Keep the world responsive

Positions, movement, node range, cooldowns, health, energy, hunts, and social state remain server-side. The authoritative loop runs at five ticks per second. Unity sends intent and interpolates accepted snapshots; it does not calculate rewards.

## 2. Put durable rewards on-chain

Gathering, release crafting, rewarded hunts, shop resource crates, and Bandage consumption use `GreenwoodSettlement`.

The API signs the exact player, action hash, inputs, outputs, nonce, rules version, world tick, and expiry. The connected wallet must then finalize the receipt. The contract burns the stated ERC-1155 inputs and mints the outputs.

This means the server is still a trusted issuance authority, but it cannot change a player's on-chain balance without a player transaction. The browser cannot invent a reward because it lacks the settlement signer key.

## 3. Make animal care the resource sink

Animal care is fully contract-controlled:

- Sheep burn 2 Grass per fed day.
- Wolves burn 3 Meat per fed day.
- Either animal can burn 10 `$WOOL` for seven auto-fed days.
- Hungry Sheep stop producing and cannot breed.
- Hungry Wolves cannot receive stolen yield or lambs.
- Hunger does not kill or burn the NFT under Rules V2.

This is a suspension economy, not the starvation-permadeath economy described in older plans.

## 4. Separate liquidity issuance from gameplay issuance

The one-billion `$WOOL` hard cap is partitioned in the active contracts:

- 100 million maximum for genesis-sale-backed protocol liquidity;
- 900 million maximum for Sheep gameplay emissions.

Each paid mint contributes `0.00008 ETH` and `10,000 WOOL` to the protocol pool and sends `0.00002 ETH` to treasury. The player receives an NFT, not the liquidity `$WOOL`.

At the contract maximum of 20,000 productive Sheep and 3 `$WOOL` per Sheep-day, the 900 million allocation lasts at least about 41.1 years. Under the intended paid distribution of 8,000 genesis Sheep plus 10,000 bred Sheep, the ceiling is 18,000 productive Sheep and about 45.7 years. These are supply-exhaustion bounds, not proof of a healthy token price or player economy.

## 5. Use explicit sinks

Current `$WOOL` sinks are:

- 10 `$WOOL` burned for each seven-day auto-feed period;
- 100 `$WOOL` burned per breeding day removed, once breeding is repaired;
- 100% of every active `GameShop` item price burned.

The 10 `$WOOL` base breeding fee, marketplace fee, theft, and pool fee redistribute value; they do not burn supply.

A self-funded Sheep that uses auto-feed emits 21 `$WOOL` in seven days and burns 10, leaving 11 before theft and optional spending. Long-term balance therefore depends on voluntary shop/breeding expenditure, the active-player mix, and issuance controls—not only the hard cap.

## 6. Scale by batching, not by pretending transactions are free

The current player confirms one transaction for each gather/craft/hunt settlement. This proves the authority boundary but is not suitable for MMO-scale repetition. The next release architecture should batch entitlements or use carefully scoped session keys/paymasters while preserving server validation, nonce protection, caps, and player-visible reconciliation.

## Strategy constraints

- Do not reintroduce database-only feeding, breeding, or `$WOOL` counters.
- Do not call ERC-1155 Fleece `$WOOL`; `$WOOL` is the ERC-20 and Fleece is a separate, currently unissued resource ID.
- Do not use the generic `game-core` recipe catalog as release truth. Only actions wired in `WorldStateService` are live.
- Do not claim economic sustainability from the simulation alone. Measure daily issuance, burns, resource settlements, pool depth, concentration, and transaction abandonment from chain data.
