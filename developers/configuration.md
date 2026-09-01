# Configuration

Use the application `.env.example` files as the schema. Never commit populated secrets.

## Web

Public browser values include:

- `NEXT_PUBLIC_PRIVY_APP_ID`;
- game server/API URLs;
- `$WOOL`, Wolf, Sheep, Land, marketplace, treasury, shop, V5 minter, pool, resources, settlement, and lifecycle addresses.

Only public identifiers and contract addresses may use `NEXT_PUBLIC_`. `PRIVY_APP_SECRET` and private keys must never be exposed in the browser bundle.

The web falls back to addresses from `@wolf-game/contracts-abi` when public overrides are absent. Verify the built bundle's actual addresses after every deployment.

## API

Required for the full shared flow:

- `PORT`, `WEB_ORIGIN`;
- `DATABASE_URL`, `REDIS_URL`;
- `PRIVY_APP_ID`, `PRIVY_PUBLIC_KEY`;
- `GREENWOOD_RESOURCES_ADDRESS`, `GREENWOOD_SETTLEMENT_ADDRESS`;
- `SETTLEMENT_SIGNER_PRIVATE_KEY`;
- `WORLD_RNG_SECRET` with at least 32 random bytes in shared environments.

Optional/controlled:

- `ROBINHOOD_RPC_URL`;
- `SHOP_REQUIRED_CONFIRMATIONS`;
- comma-separated `ADMIN_USER_IDS`;
- `AUTH_BYPASS`, local automation only.

The API does not need the Privy app secret to verify access tokens because it uses the public verification key. If another server-side Privy operation needs the app secret, keep it in that server environment only.

## Indexer

Configure:

- `DATABASE_URL`, `ROBINHOOD_RPC_URL`;
- confirmation, poll, chunk, rewind, and start-block values;
- all 11 watched contract addresses;
- `RUN_ONCE` for bounded synchronization.

The default start block is just before the first configured deployment. A lifecycle/minter redeploy requires a reviewed start point and watched-address update. The indexer does not discover deployments automatically.

## Job worker

The worker needs `DATABASE_URL`, poll interval, and optional `RUN_ONCE`. Run only one effective tick at a time; the advisory transaction lock coordinates concurrent instances.

## Unity

Unity runtime endpoints are supplied through the Next.js bridge; no wallet private key belongs in Unity. Generated WebGL files live under `apps/web/public/unity` and should be rebuilt from source rather than hand-edited.

## Contract configuration

Use `contracts/deployments/46630.json` as the repository manifest. Treat on-chain immutable dependencies and role reads as final verification. A JSON file can be stale or manually edited.

Animal economics are lifecycle Solidity constants. Release gather/craft/hunt allowlists are in `apps/api/src/world/world-state.service.ts`. Do not let the TypeScript economics object, generic recipe catalog, simulation, UI strings, or GitBook silently override those runtime authorities.

## Secret incident rule

If a Privy secret, settlement key, deployer key, or treasury key appears in chat, logs, screenshots, shell history, or source control, rotate it and invalidate the old credential. Redacting documentation afterward does not make the leaked value safe.
