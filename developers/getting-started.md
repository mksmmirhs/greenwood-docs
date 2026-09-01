# Getting started

## Requirements

- Node.js matching `.nvmrc`;
- Corepack and pnpm `11.5.2`;
- Docker/Compose for PostgreSQL/PostGIS and Redis;
- Foundry for contracts;
- Python matching `.python-version` for simulations/NFT generation;
- Unity 6 with WebGL Build Support for rebuilding the game client.

The root `package.json` declares `packageManager: pnpm@11.5.2`, which Turbo requires to resolve this workspace.

## Install and start infrastructure

```bash
corepack enable
pnpm install
pnpm dev:infra
pnpm migrate
```

Local defaults:

- web `http://localhost:3000`;
- API `http://localhost:4000`;
- PostgreSQL `localhost:5433`;
- Redis `localhost:6381`.

## Start applications

```bash
pnpm dev
```

This starts web and API only. Start the other processes separately when testing their behavior:

```bash
pnpm dev:indexer
pnpm dev:jobs
```

## Build Unity

The repository ignores generated Unity caches/build output. Build it before expecting `/world` to load the player:

```bash
pnpm build:unity
```

If the build fails, use `pnpm unity:hub` only to open/locate Hub; it is not the build command. Follow `docs/runbooks/UNITY_WEBGL_BUILD.md` for license/module checks.

## First economic session

1. Configure browser-safe Privy and contract values.
2. Configure the API's Privy public verification key, database, Redis, settlement contract, dedicated settlement signer, and RNG secret.
3. Confirm the signer has `SETTLEMENT_SIGNER_ROLE` on the configured contract.
4. Apply all migrations and start API/web.
5. Open `/world`, sign in, and bind the connected wallet.
6. Build/load Unity and enter the world.
7. Gather a node and finalize the signed ERC-1155 settlement.
8. Synchronize an eligible wallet roster, activate/assign as needed, and test feeding/claiming.

A server response saying `PENDING_SETTLEMENT` is expected. The wallet transaction is the second half of the action.

## Local versus shared behavior

The API deliberately falls back to in-memory world state when PostgreSQL is absent, and authentication can be bypassed for local automation. Do not use these fallbacks to validate a shared testnet economy. Social features, durable wallet identity, medicine, shop idempotency, and reliable restarts require PostgreSQL.
