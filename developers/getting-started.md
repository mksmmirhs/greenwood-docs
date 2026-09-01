# Getting started

## Requirements

- Node.js matching `.nvmrc`;
- pnpm `11.5.2` through Corepack;
- Docker with Compose;
- Foundry for Solidity tests and deployment;
- Python matching `.python-version` for simulation and NFT scripts;
- Unity 6 plus the WebGL Build Support module for the 3D player.

## Install and run

```bash
corepack enable
pnpm install
pnpm dev:infra
pnpm migrate
pnpm dev
```

The default local endpoints are:

- web: `http://localhost:3000`;
- API: `http://localhost:4000`;
- PostgreSQL: `localhost:5433`;
- Redis: `localhost:6381`.

The root `dev` command starts the web and API packages. Run the indexer and jobs separately when exercising projection or scheduled behavior:

```bash
pnpm dev:indexer
pnpm dev:jobs
```

## First playable session

1. Configure Privy and the API signer using local environment files.
2. Start infrastructure and apply migrations.
3. Start API, web, indexer, and jobs.
4. Open `/world`, authenticate, and bind a Robinhood Testnet wallet.
5. Acquire or mint eligible assets.
6. Synchronize the chain roster.
7. Enter the Unity world.

Never commit private keys, Privy secrets, or production RPC credentials.
