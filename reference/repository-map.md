# Repository map

## Active applications

- `apps/web` — Next.js 15. `/world` hosts Unity, Privy, chain reads/writes, lifecycle, mint/pool, marketplace, shop, and HUD actions. `/` is a showcase/simulation dashboard.
- `apps/unity` — Unity 6 runtime-built voxel farm, ranger animation, camera, network bridge, node interaction, and presentation helpers.
- `apps/api` — NestJS authentication, wallet binding, world loop, signed settlement issuance, roster reads, vitals/shop receipt verification, social, and admin APIs.
- `apps/indexer` — confirmed-log journal with persisted cursor and reorg rewind.
- `apps/job-worker` — territory expiry and season rollover.

## Alternative/source-only client code

- `apps/web/src/components/world/world-game.tsx` — older alternative client, not routed at `/world`.
- `apps/web/src/components/world/community-panel.tsx` — functional social HTTP UI mounted only by that inactive client.

This distinction explains why finding a component in source does not mean it appears in the current game.

## Contracts

- `contracts/src/tokens` — `$WOOL`, Wolf, Sheep, Land, and ERC-1155 resources.
- `contracts/src/game` — Rules V2 animal lifecycle and signed settlement.
- `contracts/src/market` — V5 minter, legacy summoner, pool, marketplace, and shop.
- `contracts/src/governance` — treasury.
- `contracts/script` — deployments and migrations.
- `contracts/deployments` — address manifests.
- `contracts/test` — 51 current Foundry tests across unit, fuzz, and invariant suites.

## Shared packages

- `packages/contracts-abi` — generated ABIs/address defaults.
- `packages/protocol` — Zod schemas and shared wire types.
- `packages/game-core` — deterministic rules library; not the release feature allowlist.
- `packages/config` — network, world, and strategy constants.
- `packages/types` — shared types.

## Persistence

- `apps/api/migrations` — players, chain events, vitals, social/seasons, playable assets, operations, combat/territory, wallet integrity, and consumable fulfilment tables.
- `infra/compose` — local PostgreSQL/PostGIS and Redis.

## NFT media

- `metadata` — source JSON for full collection namespaces.
- `art/source` — master art inputs.
- `art/generated` — 14,500 generated images and metadata.
- `art/manifests` — collection provenance output.
- `scripts/generate_nft_collection.py` — deterministic composer.
- `scripts/validate_generated_collection.py` — supply/hash/path/coordinate/provenance validator.

## Documentation and analysis

- `gitbook` — maintained product/developer truth from current code.
- `docs` — plans, ADRs, risk registers, threat model, and runbooks; some are historical.
- `simulation` — supply-conservation and scenario analysis, not a price or adoption proof.
