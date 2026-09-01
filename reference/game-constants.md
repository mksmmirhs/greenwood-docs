# Game constants

These values are extracted from the active Solidity, API, web, and shared configuration. Where sources disagree, the row names the active authority.

## `$WOOL` and V5 sale

| Constant | Value | Authority |
|---|---:|---|
| `$WOOL` decimals | 18 | `WoolToken` |
| Hard cap | 1,000,000,000 | `WoolToken` |
| Genesis liquidity budget | 100,000,000 | `GenesisMinterV5` |
| Gameplay emission budget | 900,000,000 | Lifecycle |
| Mint price | 0.0001 ETH | V5 |
| Team share | 20% | V5 |
| Liquidity share | 80% | V5 |
| Pool `$WOOL` per mint | 10,000 | V5 |
| Batch size | 1–20 | V5 |
| Paid distribution | 1,000 Wolf / 1,000 Land / 8,000 Sheep | cumulative V3–V5 counters |

## Collection caps

| Collection | Genesis cap | Secondary cap | Maximum |
|---|---:|---:|---:|
| Wolf | 2,500 | contract range beyond 2,500 | 50,000 |
| Sheep | 10,000 | 10,000 | 20,000 |
| Land | 2,000 | none | 2,000 |

V5 quotas are smaller than collection caps. Current V5 Land is Grassland, quality 10,000 bps, capacity 20. Land contract maximum capacity is 100.

## Animal lifecycle Rules V2

| Constant | Value |
|---|---:|
| Initial feed | 3 days |
| Maximum feed ahead | 28 days |
| Sheep feed | 2 Grass/day |
| Wolf feed | 3 Meat/day |
| Auto-feed | 10 `$WOOL` burned / 7 days |
| Sheep yield | 3 `$WOOL` / eligible day |
| Claim theft | 20% of full claim |
| Randomness delay | 3 blocks |
| Randomness expiry | 200 blocks |
| Wolf selection probes | max 32 |
| Hunger death | disabled/reverts |

Breeding constants on Animal Lifecycle V2:

| Constant | Value |
|---|---:|
| Base fee | 10 `$WOOL` to treasury |
| Standard duration | 7 days |
| Minimum duration | 1 day |
| Speedup | 100 `$WOOL` burn/day removed |
| Maximum reduction | 6 days |
| Parent cooldown | 7 days after ready time |
| Maximum breed count | 20 per parent |
| Lamb theft | 10% |
| Secondary lifecycle cap | 10,000 Sheep |

## ERC-1155 resources

Every resource has a one-billion whole-unit cap.

| ID | Contract name | Current UI/release meaning |
|---:|---|---|
| 0 | Timber | Timber/Wood |
| 1 | Stone | Stone |
| 2 | Ore | Ore; no current world issuer |
| 3 | Food | Grass/Fodder |
| 4 | Meat | Wolf feed/hunt output |
| 5 | Hide | hunt output |
| 6 | Herbs | gather output |
| 7 | Fleece | separate resource; not `$WOOL`, no current issuer |
| 8 | Planks | Plank craft output |
| 9 | Bandages | Bandage craft/medicine input |
| 10 | Building Kits | shop bundle output; building use locked |

## Release action amounts

- gather: +3 selected resource;
- Bandage: burn 1 Timber + 1 Stone + 1 Grass → mint 1 Bandage;
- Plank: burn 3 Timber → mint 1 Plank;
- medicine: burn 1 Bandage → +40 server health after receipt verification;
- Timber Crate: +50 Timber;
- Stone Crate: +50 Stone;
- Master Bundle: +100 Timber, +50 Stone, +1 Building Kit.

## Shop

All active items are configured as 100% burn:

- Health Elixir: 5 `$WOOL`, server health to 100;
- Energy Flask: 3 `$WOOL`, server energy to 100;
- Timber Crate: 10 `$WOOL`;
- Stone Crate: 10 `$WOOL`;
- Master Bundle: 25 `$WOOL`.

## Marketplace and pool

- marketplace fee: 3% current/default, owner maximum 10%;
- listing maximum duration: 30 days;
- active UI listing duration: 7 days;
- pool center bin: 8,388,608;
- pool initial price: 8,000,000,000 wei per `$WOOL`;
- bin step: 0.25%;
- base fee: 0.30%;
- dynamic fee cap: 2%.

## World

- protocol version: 1;
- tick: 5 Hz;
- map: 32 × 32;
- movement: 4 tiles/second;
- gather range: 8 tiles;
- gather cooldown: 1.5 seconds;
- node respawn: 5 seconds in `WorldStateService`;
- interest radius: 12 tiles;
- configured capacity: 100;
- energy regeneration: +1 / 15 seconds;
- health regeneration: +1 / 60 seconds.

Hunt constants are listed in [World resources, hunts, crafting, and vitals](../gameplay/server-gameplay.md).
