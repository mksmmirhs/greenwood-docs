# Greenwood MMO

Greenwood is a voxel-style WebGL game with an authoritative multiplayer server and a Robinhood Chain Testnet economy. The player explores the Greenwood, gathers and crafts through the server, and manages Land, Sheep, Wolves, resources, and `$WOOL` through on-chain transactions.

This book documents the code and deployment that exist in this repository. It does not treat design documents as proof of implementation.

## Status language

- **Live** — implemented and connected to the current Robinhood Testnet deployment.
- **Source-only** — implemented in source but not proven against the current deployment.
- **Prototype** — usable for development, but not production-complete.
- **Blocked** — a known defect prevents the promised end-to-end behavior.
- **Planned** — a product target without a complete implementation.

## Current release truth

The project is a playable testnet vertical slice, not yet a full production MMO.

- **Live:** Privy login, wallet binding, V5 paid genesis mint, NFT ownership reads, lifecycle activation and land assignment, animal feeding, Sheep `$WOOL` claims, protocol liquidity, swaps, marketplace transactions, server movement, gathering, crafting, hunts, combat, vitals, and Unity WebGL presentation.
- **Prototype:** guilds, territories, chat, seasons, admin/live operations, settlement receipts, indexing, NFT generation, and economic simulations.
- **Blocked:** the deployed breeding request stores randomness that expires hundreds of blocks before the 1–7 day breeding timer finishes. Breeding must not be advertised as working until the lifecycle contract is corrected and redeployed.
- **Planned:** production-grade sharding, gas sponsorship/session keys, audited mainnet contracts, immutable NFT media, load targets, anti-cheat, and complete world content.

Start with [Current status](overview/current-status.md), then read [The core loop](gameplay/core-loop.md).
