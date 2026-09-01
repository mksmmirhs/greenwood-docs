# Repository map

## Applications

- `apps/web` — Next.js 15 shell, Privy UI, Unity host, marketplace, mint, swap, and lifecycle interfaces.
- `apps/unity` — Unity 6 voxel renderer, movement, animation, camera, combat presentation, and JavaScript bridge.
- `apps/api` — NestJS auth, wallet binding, world gateway, chain roster, economy boundary, vitals, social, and admin APIs.
- `apps/indexer` — confirmed Robinhood event ingestion, cursors, and reorg rewind.
- `apps/job-worker` — territory upkeep and season rollover tasks.

## Contracts

- `contracts/src/tokens` — `$WOOL`, Wolf, Sheep, Land, and resource tokens.
- `contracts/src/game` — animal lifecycle and signed settlement.
- `contracts/src/market` — V5 genesis mint, liquidity pool, marketplace, shop, and legacy summoner.
- `contracts/src/governance` — treasury.
- `contracts/test` — unit, fuzz, integration, and invariant tests.
- `contracts/script` — deployment and administration scripts.
- `contracts/deployments` — machine-readable chain manifests.

## Shared packages

- `packages/contracts-abi` — generated ABIs and addresses.
- `packages/game-core` — deterministic server rules.
- `packages/protocol` — network schemas.
- `packages/config` — common configuration.
- `packages/types` — shared TypeScript data types.

## Content and operations

- `art`, `metadata`, and `scripts` — deterministic NFT generation and validation.
- `simulation` — economic scenarios and invariant tests.
- `infra/compose` — local PostgreSQL/PostGIS and Redis.
- `docs` — engineering plans, ADRs, threat model, and runbooks.
- `gitbook` — player/developer documentation backed by the current source.
