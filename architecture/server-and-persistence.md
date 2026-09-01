# Server and persistence

## Authentication

HTTP and Socket.IO accept Privy access tokens verified with the configured app ID and public verification key. `AUTH_BYPASS=true` creates local identities and is for automated local testing only.

The API checks `banned_users` when PostgreSQL is available. Authentication does not prove wallet ownership, so economic actions additionally use wallet binding.

## Wallet binding

The server creates an EIP-191 message containing Privy user ID, checksummed wallet, chain ID 46630, UUID nonce, and five-minute expiry. It verifies the wallet signature and enforces one bound wallet per account through the database constraint.

The service has in-memory fallback behavior when persistence fails. That helps local development but is not acceptable as a shared-economic identity store.

## World runtime

`WorldStateService` keeps connected players, direction, cooldown, processed request IDs, and node availability in memory. It persists position, server inventory, XP, health, and energy when PostgreSQL is available.

The five-Hz gateway broadcasts an interest-filtered snapshot per connected player. Redis limits movement, economy, hunt, and chat action rates. Without Redis, behavior depends on the fallback implementation and should not be considered horizontally safe.

## Economic recording

- Hunts use serializable transactions to record idempotent economic events and the new player state.
- Shop fulfilments are unique by transaction hash/log and player/item.
- Medicine fulfilments are unique by settlement event.
- Signed settlement nonces are derived from action-specific keys and ultimately protected by the contract's `usedNonce` mapping.

Gather/craft signed entitlements are not a database-built inventory. Their durable result exists in the ERC-1155 contract after wallet finalization.

## Roster synchronization

`ChainRosterService`:

1. gets the user's verified wallet;
2. reads enumerable Wolf, Sheep, and Land ownership from Robinhood RPC;
3. reads traits and lifecycle eligibility;
4. deactivates stale projection rows;
5. upserts current eligible assets and returns access counts.

It scans at most 500 assets per collection group in the API path. The browser helper displays at most 100 owned tokens per collection. These limits must become paginated/indexed before scale.

The projection's `assigned_land_id`, animal health, and hunger columns are not lifecycle contract truth. The lifecycle HUD separately reads `sheepState`/`wolfState` for feed and assignment data.

## Social persistence

The HTTP social service implements:

- guild create/list/join/detail;
- authenticated global, trade, and guild chat;
- four seeded territory reads and owner/officer claim;
- 100 season points for a successful claim;
- current leaderboard;
- one-time message reports and audit records.

These endpoints require PostgreSQL and fail with service-unavailable rather than pretending persistence succeeded.

The job worker takes a PostgreSQL advisory transaction lock, releases territories whose seven-day upkeep deadline expired, completes an ended season, and creates the next 90-day testnet season. It records job/audit output. It does not distribute season rewards.

Guild-vault and siege tables/protocol types exist without a complete durable service. The Socket.IO `world:territory:claim` and `world:vault:action` handlers currently echo success text and must not be used as an authoritative path; use the implemented HTTP territory endpoint.

## Indexer

The indexer watches the 11 configured contracts, waits five confirmations by default, decodes known events, inserts them idempotently into `chain_events`, and persists a cursor. When the saved block hash changes, it deletes later journal rows and rewinds 50 blocks.

It is a confirmed event journal, not a full projection engine. It does not currently turn events into player balances, ownership tables, aggregate economy dashboards, or lifecycle state.
