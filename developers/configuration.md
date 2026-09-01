# Configuration

Copy each application's `.env.example` to its local environment file and supply secrets outside version control.

## Web

Browser-safe values use `NEXT_PUBLIC_`:

- `NEXT_PUBLIC_PRIVY_APP_ID`;
- NFT, `$WOOL`, marketplace, shop, settlement, lifecycle, genesis, resource, treasury, and pool addresses.

`PRIVY_APP_SECRET` and private keys must never use the public prefix.

## API

Configure:

- `PORT` and `WEB_ORIGIN`;
- `DATABASE_URL` and `REDIS_URL`;
- `PRIVY_APP_ID` and `PRIVY_PUBLIC_KEY`;
- `SETTLEMENT_SIGNER_PRIVATE_KEY` for a dedicated testnet signer;
- `WORLD_RNG_SECRET` with at least 32 random bytes;
- optional comma-separated `ADMIN_USER_IDS`.

`AUTH_BYPASS` is only for local automated clients and must remain false in shared environments.

## Indexer

Configure the Robinhood RPC, confirmation count, poll interval, chunk size, reorg rewind, start block, database URL, and every watched contract address.

When a contract is redeployed, the indexer address and start block must change with it. Leaving an older lifecycle or minter address in the indexer creates a split-brain UI even when the web points at the new contract.

## Source of truth

Use `contracts/deployments/46630.json` as the deployment manifest and generate shared exports from it. Do not manually maintain competing address lists in documentation.

The current lifecycle Solidity constants are runtime truth for animal economics. Shared TypeScript/simulation configuration must be kept synchronized and must not override contract behavior.
