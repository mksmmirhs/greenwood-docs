# Greenwood MMO

Greenwood is a hybrid voxel MMO prototype: Unity renders the shared world, a NestJS server validates real-time play, and Robinhood Chain Testnet owns NFTs, `$WOOL`, durable resources, animal care, and economic settlement.

This book follows the code that is active in the repository on **2026-09-01**. Older plans, dashboard copy, simulations, and comments are not treated as implementation evidence.

## The current playable loop

1. Sign in with Privy and bind the wallet by signing a challenge.
2. Open `/world`, load Unity WebGL, and join the five-Hz server world.
3. Move with WASD and request a gather, craft, or hunt action.
4. The server validates the action and, when a durable reward is due, signs a short-lived settlement.
5. The wallet submits that settlement to Robinhood Testnet. The ERC-1155 resource exists only after this transaction confirms.
6. Use on-chain Grass to feed Sheep and Meat to feed Wolves.
7. Assign an eligible Sheep to eligible Land and request/resolve its ERC-20 `$WOOL` yield.
8. Mint, swap, shop, and trade through the contracts exposed in the `/world` HUD.

Movement is not on-chain. Durable resource rewards are on-chain. Health, energy, positions, hunts, guilds, and chat are server state. NFT ownership, animal feed clocks, Land assignments, `$WOOL`, and ERC-1155 resources are contract state.

## Release language

- **Deployed** — a configured contract exists on Robinhood Testnet and was read during this audit.
- **Wired** — the current `/world` route calls the implementation.
- **Backend-only** — the API or worker exists, but the active Unity route does not expose the complete feature.
- **Source-only** — rules or presentation code exists without a reachable end-to-end start path.
- **Blocked** — code deliberately disables the feature or a known defect prevents safe use.
- **Planned** — a design target, not current behavior.

## Release truth

- **Wired:** Privy login, wallet challenge binding, Unity movement, server-validated gathering, two settlement recipes, server hunts, ERC-1155 settlement, V5 minting, the custom `$WOOL` pool, lifecycle activation & multi-land assignment, animal feeding & auto-feed, Sheep `$WOOL` yield claims, Breeding House UI with 1–7 day sheep breeding and speedup burn options, NFT listings and purchases, shop purchase fulfilment, and Bandage medicine.
- **Backend-only:** guild creation/joining, HTTP territory claims, global/trade/guild chat, leaderboards, reports, moderation, season rollover, and confirmed-log journaling.
- **Source-only:** round-based combat presentation/rules and most of the 20 recipe library.
- **Not implemented as current rules:** animal starvation death, Wolf breeding, overcrowding yield multipliers, on-chain health/stamina, on-chain movement, building settlement, guild vault economics, and siege gameplay.

Read [Current status](overview/current-status.md) before treating any feature as released. The intended product direction is in [Release strategy](overview/release-strategy.md).
