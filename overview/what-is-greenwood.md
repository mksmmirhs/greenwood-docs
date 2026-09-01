# What is Greenwood?

Greenwood combines a fast server-authoritative multiplayer loop with durable EVM ownership.

The Unity WebGL client renders a voxel homestead and world. A Next.js shell handles authentication, wallet transactions, marketplace and animal-management interfaces. A NestJS server validates movement and moment-to-moment game actions. Solidity contracts own scarce assets and economic settlement.

## Player fantasy

You are a ranger building a homestead in a shared wilderness:

1. Enter the world and explore with other players.
2. Gather Timber, Stone, Grass, Meat, Hide, Herbs, and other resources.
3. Craft tools and medicine, hunt creatures, and maintain health and stamina.
4. Acquire Land, Sheep, and Wolves as NFTs.
5. Assign animals to Land and keep them fed.
6. Produce and claim `$WOOL`, breed Sheep, trade assets, and participate in the economy.
7. Join guild and territory competition as those systems mature.

## Design boundary

Greenwood is deliberately hybrid. Putting every movement update or combat round on-chain would make the game slow and expensive. Leaving ownership and token issuance on a game server would make those assets unverifiable.

The system therefore uses:

- the **chain** for ownership, scarcity, token flows, lifecycle rules, and value settlement;
- the **server** for real-time world actions, anti-cheat validation, health, stamina, combat, social state, and settlement authorization;
- the **client** for presentation, input, prediction, transaction prompts, and synchronized UI.

See [Authority model](../web3/authority-model.md) for the exact split.
