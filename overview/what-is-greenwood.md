# What is Greenwood?

Greenwood is a persistent-homestead and expedition game built across three authorities.

- **Unity WebGL** renders the voxel farm, ranger, other connected players, and resource nodes.
- **The game server** owns movement validation, positions, health, energy, hunt outcomes, social data, and permission to issue durable resource settlements.
- **Robinhood Testnet** owns NFTs, `$WOOL`, ERC-1155 resources, animal feed clocks, Land assignments, Sheep production, mint revenue, swaps, shop payments, and NFT sales.

The Next.js `/world` page connects these systems. It hosts Unity, handles Privy and wallet transactions, displays the homestead HUD, and submits server-signed settlements to the chain.

## Player fantasy

The player is a ranger maintaining a farm and venturing into Greenwood:

1. walk through a shared voxel homestead;
2. gather Grass, Timber, Stone, and Herbs;
3. hunt to earn Meat and Hide while spending server energy and risking server health;
4. craft Planks or Bandages from on-chain resource inputs;
5. own Land, Sheep, and Wolves;
6. feed animals and assign them to Land;
7. claim Sheep-produced `$WOOL`;
8. spend `$WOOL`, swap against protocol liquidity, and trade NFTs.

## What the current build is not

It is not a fully on-chain game. Sending every movement tick to the EVM would make the world unusable. It is also not yet a full MMO release: the active route has a single small world, manual wallet confirmation for each durable settlement, no reachable round-combat entry, no social panel, disabled breeding, and no building transaction flow.

The large NFT and simulation dashboard at `/` is a showcase/development surface. Its sample names, rarity statistics, scenario claims, and cards are not contract truth. The playable client is `/world`.
