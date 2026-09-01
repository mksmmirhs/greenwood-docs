# System overview

```text
                    Robinhood Testnet
                 /        |          \
        direct reads   wallet txs   confirmed logs
              /            |              \
       Next.js HUD ---- settlement ---- event indexer
          |   |          finalization          |
       Privy  |                               PostgreSQL
          |   |
      Unity WebGL
          |
  authenticated Socket.IO
          |
       NestJS API ---- PostgreSQL
          |  \
          |   Redis
          |
 signed resource entitlement
```

## Active browser route

`apps/web/src/app/world/page.tsx` renders the authentication button and `UnityWorld`. `UnityWorld` loads the compiled WebGL manifest/build, opens the Socket.IO connection, forwards snapshots into Unity, receives Unity input, and serializes resource-settlement wallet prompts.

`HomesteadHud` reads chain balances and assets and exposes minting, lifecycle, marketplace, pool, shop, medicine, gather, craft, and hunt actions.

The older `components/world/world-game.tsx` Phaser-style client and its `CommunityPanel` are not mounted by `/world`.

## API

The API provides:

- Privy token verification and account-ban checks;
- wallet challenge binding;
- roster synchronization from chain;
- the authoritative movement/world loop;
- signed ERC-1155 settlements;
- shop and medicine receipt verification;
- social and administrative HTTP endpoints.

## Shared packages

- `@wolf-game/contracts-abi` — ABIs and default Robinhood addresses.
- `@wolf-game/protocol` — schemas for HTTP/socket payloads and signed settlement serialization.
- `@wolf-game/game-core` — deterministic movement, hunt, combat, recipe, building, and profession rules. Not every library rule is enabled in the release server.
- `@wolf-game/config` — chain/world/economic constants.
- `@wolf-game/types` — shared domain types.

## Separate processes

`pnpm dev` starts only web and API. The confirmed-log indexer and scheduled job worker are separate processes. A complete shared testnet environment also needs PostgreSQL, Redis, migrations, a configured settlement signer with the correct on-chain role, and the compiled Unity WebGL files.
