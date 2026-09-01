# World resources, hunts, crafting, and vitals

The game server decides whether a real-time action is valid. Durable resource changes then pass through a signed on-chain settlement.

## World movement

- map: 32 × 32 server tiles;
- speed: 4 tiles per second;
- tick: 5 Hz;
- visible-player interest radius: 12 tiles;
- world capacity configuration: 100 players;
- fixed rectangular obstacles and map bounds are enforced by `stepPosition`.

Unity sends direction changes rather than an on-chain transaction stream. The server sequence-checks movement, and Unity interpolates the snapshots.

## Gathering

Gathering checks authentication, duplicate request ID, node existence, node availability, eight-tile range, a 1.5-second player cooldown, and a five-second node respawn.

The reward is always a signed settlement for three units of the node's ERC-1155 resource. Rejecting the wallet transaction means no durable balance is minted. The node is still depleted temporarily because depletion happens when the server validates the action.

## Release crafting

`WorldStateService` enables exactly two signed-settlement recipes:

| Recipe ID | On-chain inputs burned | On-chain output minted | Active `/world` control |
|---|---|---|---|
| `bandage` | 1 Timber + 1 Stone + 1 Grass | 1 Bandage | Yes |
| `wood_plank` | 3 Timber | 1 Plank | No direct HUD button |

The generic `RECIPES` catalog contains 20 recipes and 10 professions, but it is not the active settlement allowlist. In particular, its older Bandage recipe says 2 Wool-material + 1 Herb, which is not what the release server signs.

Building is explicitly rejected by the economy controller until a building-NFT settlement exists. The Unity farm blueprint can change presentation state, but that is not a durable building asset.

## Hunts

The server supports Safe, Medium, Dangerous, and Legendary tiers. It derives the outcome from an HMAC of user ID, request ID, tier, and `WORLD_RNG_SECRET`.

| Tier | Energy | Injury | Fatal roll | Reward range | XP on success |
|---|---:|---:|---:|---:|---:|
| Safe | 10 | 5% | 0% | 1–3 | 5 |
| Medium | 20 | 15% | 0.5% | 3–7 | 12 |
| Dangerous | 35 | 30% | 3% | 7–14 | 25 |
| Legendary | 60 | 50% | 10% | 15–30 | 60 |

A successful hunt signs Meat and Hide outputs. The reward number becomes Meat; Hide is at least one and otherwise approximately one third of Meat. A failed hunt can still spend energy and damage health.

“Fatal” means the server player reaches 0 health. It does not burn an NFT and is not permanent death. The player cannot hunt again while incapacitated but can regenerate or restore health.

The active HUD exposes only Safe hunt. The other tiers are accepted by the protocol/server but are not all presented in the current Unity interface.

## Health and energy

- Health and energy each cap at 100.
- Energy regenerates 1 every 15 seconds.
- Health regenerates 1 every 60 seconds.
- A rewarded or failed hunt commits health, energy, and XP before the player confirms any reward settlement.

### Bandage medicine

1. The API signs a five-minute settlement burning 1 Bandage.
2. The player finalizes the burn on-chain.
3. The API verifies the exact `SettlementFinalized` event, player, action, rules version, input, and transaction destination.
4. A serializable database transaction records the one-time fulfilment and adds 40 health, capped at 100.

This flow requires PostgreSQL. The same transaction hash cannot restore health twice.

### Shop vital refills

Health Elixir and Energy Flask first burn `$WOOL` in `GameShop`. The API then verifies the `ItemPurchased` event for the bound wallet and item. It sets the selected vital to 100 and records an idempotent fulfilment.

The vital effect is server-side even though the payment is on-chain.

## Round combat status

Four enemy configurations, combat rounds, persistence, protocol messages, and Unity presentation exist. However, no current controller or gateway path calls `CombatService.startEncounter`. Sending a combat action without an active encounter returns a rejection. Round combat is therefore source-only, distinct from the wired one-click hunt system.
