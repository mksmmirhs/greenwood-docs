# Core loop

## 1. Authenticate and bind a wallet

Privy authenticates the player. A challenge-signature flow binds the authenticated user to an EVM wallet. Server roster synchronization accepts only assets owned by the bound wallet.

## 2. Acquire on-chain assets

The V5 genesis minter sells a random Wolf, Sheep, or Land NFT for `0.0001 ETH`. Players may also acquire supported NFTs from the escrow marketplace.

Genesis supply is capped at:

- 1,000 Wolves;
- 1,000 Lands;
- 8,000 Sheep;
- 10,000 total V5 mints.

## 3. Enter the world

The browser loads Unity WebGL and establishes the authenticated WebSocket session. Movement and world actions are accepted or rejected by the server. The Unity bridge renders server snapshots and action results.

## 4. Gather and craft

Gathering, hunting, crafting, health, and stamina are fast server actions. The server maintains authoritative cooldowns, range checks, resource changes, and combat results.

When server-earned Grass or Meat must be used for animal feeding, the resource first has to exist as the player's ERC-1155 balance through the signed settlement path. The lifecycle then burns that on-chain resource.

## 5. Manage the homestead

An animal must be lifecycle-eligible, activated, and assigned to a Land owned by the same wallet. Land capacity limits how many current assignments it can hold.

Keep Sheep fed to accrue `$WOOL`. Keep Wolves fed so they can participate in theft selection. Feed using on-chain Grass/Meat or burn `$WOOL` for a seven-day auto-feed period.

## 6. Claim, trade, and spend

Sheep yield is claimed through a delayed on-chain request and resolve flow. The player can trade NFTs in escrow, swap `$WOOL` and test ETH in the protocol pool, or spend `$WOOL` in contract-backed sinks.

The current breeding flow is disabled at the release level until the deployed timing defect described in [Current status](../overview/current-status.md) is fixed.
