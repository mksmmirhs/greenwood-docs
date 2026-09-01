# Core loop

## 1. Sign in and bind a wallet

Privy authenticates the account. Economic server actions also require a bound EVM wallet. Binding is a separate five-minute message-signing challenge for chain `46630`; it sends no transaction and grants no token approval.

The wallet binding matters because the server signs durable rewards only for that verified address.

## 2. Join the Unity world

Open `/world`, wait for the Unity build, and select **Enter the world**. Unity sends movement intent over Socket.IO. The server advances accepted positions at five ticks per second, applies map bounds and fixed obstacles, and sends interest-filtered snapshots back to Unity.

The current socket join does not enforce NFT ownership. The HUD separately reports whether the wallet has the intended access set: at least one eligible Land and one eligible Wolf or Sheep.

## 3. Earn a durable resource

Walk within eight tiles of a node and gather. The release nodes produce:

- Grass/Fodder (`3`) from pasture nodes;
- Timber (`0`) from wood nodes;
- Stone (`1`) from quarry nodes;
- Herbs (`6`) from herb nodes.

An accepted gather does not directly update an off-chain reward balance. It returns a signed `+3` ERC-1155 settlement. Confirm the wallet transaction before calling the resource owned.

## 4. Hunt and craft

The active hotbar exposes a Safe hunt and the Bandage recipe.

- A rewarded hunt changes server health, energy, and XP, then offers a settlement for Meat (`4`) and Hide (`5`).
- The live Bandage settlement burns 1 Timber + 1 Stone + 1 Grass and mints 1 Bandage (`9`).
- The server also accepts a Plank recipe that burns 3 Timber and mints 1 Plank (`8`), but the active Unity HUD does not expose it.

The 20 recipes in `packages/game-core` are a design/rules library. They are not all enabled settlement recipes, and its Bandage ingredients differ from the live `WorldStateService` recipe.

## 5. Acquire and prepare a homestead

The active V5 minter returns a random NFT for `0.0001 ETH`. It does not guarantee a complete access set in a single mint. Players can also acquire supported assets through the marketplace.

For animal production:

1. own a lifecycle-eligible animal and eligible Land;
2. activate a legacy animal if it was not registered by V5;
3. assign the animal to owner-held Land;
4. keep it fed.

V5 Wolf and Sheep mints are activated during minting but are not automatically assigned to Land.

## 6. Feed and claim

Feed Sheep by burning on-chain Grass and Wolves by burning on-chain Meat. A player can instead approve and burn `$WOOL` for seven-day auto-feed periods.

A fed, activated, non-breeding Sheep on a valid Land assignment accrues ERC-20 `$WOOL`. Claiming is a request transaction followed by a resolve transaction after a future block. The UI can process up to five Sheep sequentially.

## 7. Spend and trade

- swap test ETH and `$WOOL` in the protocol's custom pool;
- buy shop items by burning `$WOOL`, then let the server verify the purchase and apply the effect;
- list supported NFTs for ETH or `$WOOL` and buy them through the marketplace;
- burn a Bandage settlement and have the server verify it for `+40` health.

Breeding is not part of the current playable loop until the lifecycle contract is repaired and redeployed.
