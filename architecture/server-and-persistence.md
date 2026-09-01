# Server and persistence

## Authentication and wallet binding

The API verifies Privy access tokens. Wallet binding uses a server-created challenge and a wallet signature, preventing a caller from claiming another address by sending it in a request body.

Privy secrets and the settlement signer key are server-only. Browser code receives only the public Privy app ID and public contract addresses.

## World gateway

The Socket.IO gateway handles join, movement, gather, craft, hunt, combat, territory, and vault messages. Protocol version and request IDs support compatibility and idempotency. The server owns cooldown and range validation.

## Database

PostgreSQL/PostGIS stores players, bound wallets, positions, inventory, assets, buildings, transactions, settlement nonces, guilds, territories, seasons, chat, moderation, and operational state. Migrations live under the API application.

On-chain roster rows are projections. They include contract and token IDs and are refreshed from the bound wallet. The server economy controller deliberately rejects its retired mock `feed`, `produce`, and `breed` mutations.

## Redis and jobs

Redis supports transient presence, shared cooldown/rate data, and coordination. The job worker performs scheduled territory upkeep and season rollover work. Production still needs durable queues, distributed leases, retry/dead-letter policy, and recovery drills.

## Settlement safety

Signed settlements use a dedicated signer, domain separation, nonces, deadlines, and receipt verification. The signer must not be the deployer or treasury key. A database outage must fail closed rather than issue unverifiable rewards.
