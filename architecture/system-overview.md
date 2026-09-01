# System overview

```text
Privy + Next.js shell
        |
        +-- viem wallet transactions ---------------- Robinhood Testnet
        |
        +-- Unity WebGL via WolfBridge.jslib
                    |
              authenticated WebSocket
                    |
                 NestJS API
                    |
       +------------+-------------+
       |            |             |
  PostgreSQL      Redis       job worker
       ^                            |
       +--------- chain indexer ----+
```

## Browser application

The Next.js app owns authentication, wallet selection, chain configuration, marketplace, minting, swaps, animal lifecycle HUD, and the container for Unity. Contract calls use ABIs and deployment defaults from the shared contracts package.

## Unity presentation

Unity owns voxel world rendering, ranger animation, camera, local input, visible players, resource nodes, homestead state, biome presentation, equipment, and combat effects. It communicates with JavaScript through `WolfBridge.jslib` and `NetworkBridge.cs`.

## API and world loop

NestJS provides HTTP identity/economy/social endpoints and a Socket.IO world gateway. A five-Hz loop validates and broadcasts world snapshots. PostgreSQL stores durable player, inventory, social, and projection state; Redis supports ephemeral shared state.

## Chain integration

The web submits player transactions. The API verifies settlement receipts and synchronizes the bound wallet's roster. The indexer consumes contract logs after confirmation depth and handles reorg rewind. Chain data remains authoritative even when a projection is temporarily stale.

## Shared packages

- `contracts-abi` — ABI and Robinhood deployment addresses.
- `protocol` — versioned request and event schemas.
- `game-core` — deterministic gameplay rules and tests.
- `config` — shared network/economy configuration.
- `types` — common TypeScript types.
