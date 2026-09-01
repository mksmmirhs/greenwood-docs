# Player transaction flows

## Server-authorized resource settlement

This is the core hybrid flow for gather, craft, hunt rewards, resource shop items, and Bandage use.

1. The authenticated player sends an action with a request ID.
2. The server checks the bound wallet and validates the action.
3. The API signs a typed settlement containing the player, action hash, input/output hashes, deterministic nonce, Rules Version 2, world tick, and short deadline.
4. Next.js asks that wallet to call `GreenwoodSettlement.finalize`.
5. The contract checks caller, deadline, rules version, unused nonce, resource arrays, hashes, and signer role.
6. It burns inputs and mints outputs atomically, then emits `SettlementFinalized`.

The normal TTL is 120 seconds, bounded to 30–300 seconds. Medicine uses 300 seconds. A signed response is not an owned resource until finalization confirms.

## Gathering

Each release gather settlement has no input and mints 3 units:

- `GATHER_WOOD` → Timber `0`;
- `GATHER_STONE` → Stone `1`;
- `GATHER_FOOD` → Grass/Food `3`;
- `GATHER_HERBS` → Herbs `6`.

## Crafting

- `CRAFT_BANDAGE`: burn `1×0`, `1×1`, `1×3`; mint `1×9`.
- `CRAFT_PLANK`: burn `3×0`; mint `1×8`.

The settlement contract has the burner role, so the player does not grant it ERC-1155 operator approval for this path. The signature is the authorization.

## Hunt reward

The server resolves the HMAC-based hunt and commits server health, energy, and XP. If Meat or Hide was won, it signs `HUNT_<TIER>` with resource outputs `4` and `5`. The player then chooses whether to finalize the reward.

## Animal setup

1. Own an eligible Wolf/Sheep and Land.
2. If the animal is historical and inactive, call `activateLegacyAnimal`.
3. Call `assignAnimalToLand`.
4. Feed it before its three-day initial window expires.

V5 animals are registered/activated during the mint transaction. Land has no animal activation.

## Resource feeding

The lifecycle directly holds the ERC-1155 burner role:

- `feedSheep(id, days)` burns `2 × days` Grass/Food ID `3`;
- `feedWolf(id, days)` burns `3 × days` Meat ID `4`.

No ERC-1155 operator approval is required because `burnFrom` is role-gated and called by the lifecycle. The caller must own the animal. The result cannot exceed 28 days ahead of the current time.

## `$WOOL` auto-feed

1. Check the player's ERC-20 allowance to lifecycle.
2. If needed, approve lifecycle.
3. Call animal auto-feed for one to four seven-day periods.
4. Lifecycle calls `WoolToken.burnFrom` for `10 × periods` `$WOOL`.

The current UI performs the allowance check. Feed-ahead still cannot exceed 28 days.

## Sheep claim

1. Call `requestWoolClaim(sheepId)`.
2. Wait until the chain is past the recorded entropy block.
3. Call `resolveWoolClaim(sheepId)`.
4. Lifecycle mints the checkpointed fixed-rate yield to the Sheep owner or a qualifying fed Wolf owner after a 20% theft roll.

The UI's “up to 5” action loops over Sheep and performs this two-transaction flow for each. It is not a single atomic batch. If the entropy is over 200 blocks old, the helper cancels the expired request before opening another.

## V5 genesis mint

1. Call `summon(recipient)` or `summonBatch(recipient, 1..20)` with exactly `0.0001 ETH × count`.
2. V5 selects among the remaining cumulative quotas.
3. It mints Land, Wolf, or Sheep to the recipient.
4. A new Wolf/Sheep is registered and activated in lifecycle.
5. V5 mints `10,000 WOOL × count` to the pool and calls `addProtocolLiquidity` with 80% of ETH.
6. It sends 20% of ETH to treasury.

Selection uses current block variables and must be replaced for a value-bearing mainnet sale.

## Marketplace

### Listing

1. Seller approves the marketplace for the NFT.
2. Seller creates a supported-collection listing with nonzero price, ETH/`$WOOL` currency, and expiry no more than 30 days away.
3. The NFT stays in the seller's wallet.

### Purchase

- ETH: buyer pays the exact price; the contract credits fee and proceeds to `pendingETH`, transfers the NFT, and each recipient later withdraws.
- `$WOOL`: buyer approves the marketplace; fee transfers to treasury and net price transfers to seller during purchase; NFT transfers atomically.

The fee defaults to 3% and can be changed by owner up to 10%. A seller transfer or revoked approval makes the listing stale. The active UI reads only the latest 100 listing IDs.

## Shop

1. Approve `GameShop` for the item's `$WOOL` price.
2. Call `buyItem(itemId)`; the current contract burns 100% and emits `ItemPurchased`.
3. Send the confirmed purchase transaction hash to `/economy/shop/claim`.
4. The API verifies destination, confirmation depth, buyer, and item and records one-time fulfilment.
5. Health/Energy effects apply to server state. Resource items return a second signed settlement which the wallet finalizes for ERC-1155 outputs.

Resource outputs are +50 Timber, +50 Stone, or +100 Timber/+50 Stone/+1 Building Kit.

## Bandage medicine

Medicine is the reverse bridge: an on-chain Bandage is burned first, then the verified receipt adds 40 server health. The API verifies the exact action and allows each settlement event only once.

## Breeding

The intended flow is documented for contract review, but the current release must not execute it. See [Animals, hunger, and Sheep yield](../gameplay/animal-lifecycle.md).
