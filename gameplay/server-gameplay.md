# Resources, crafting, combat, and vitals

The multiplayer server owns fast-changing game state. These systems are not EVM transactions for every click.

## World actions

The authenticated WebSocket gateway accepts:

- world join and movement;
- gathering;
- crafting;
- hunts and combat actions;
- territory and vault actions.

The world state service validates sequence, range, cooldown, inventory, and authoritative outcomes. Clients render accepted snapshots and display rejection reasons; they do not decide rewards.

## Health and stamina

Health and stamina live on the server. Actions consume stamina, time regenerates it, and combat changes health. Medicine preparation is a server action. Where a contract settlement is required, the server issues a signed receipt and verifies the submitted transaction before applying the claim.

## Resource bridge

Timber, Stone, Wool-material, Grass, Meat, Hide, Herbs, and crafted supplies may exist in server inventory for the real-time loop. Durable ERC-1155 resources are issued through `GreenwoodSettlement` using a signed authorization and nonce/deadline protection.

Animal feeding is the important boundary:

1. A player earns Grass or Meat through server-authoritative gameplay.
2. The server signs the permitted resource settlement.
3. The player settles it on-chain and receives ERC-1155 resources.
4. The player calls lifecycle feeding, which burns those resources on-chain.

This avoids letting the browser mint food while keeping gathering responsive.

## Prototype scope

Guilds, chat, territories, seasons, moderation, and job-worker transitions exist in the codebase, but they need integration, abuse testing, recovery tests, and full client UX before they should be called production-complete MMO systems.
