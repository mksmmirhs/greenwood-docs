# Testing and deployment

## Repository checks

```bash
pnpm typecheck
pnpm test
pnpm build
```

`pnpm test` runs Python economic tests, deterministic `game-core` tests, and Foundry. `pnpm check` additionally validates source metadata, regenerates all NFT assets, validates generated provenance, type-checks, and builds.

For contract source changes, force a clean compilation path at least once:

```bash
cd contracts
forge test --force
```

The 2026-09-01 forced run passed 51 tests. Passing does not remove the known breeding blocker because the tests do not model realistic elapsed blocks during the one-to-seven-day timer.

## Required lifecycle tests

Before another lifecycle deployment, add tests that:

- roll realistic blocks while warping one and seven days;
- prove every requested breed can resolve or recover;
- prove cancellation clears both parent locks without duplicating issuance;
- call both overloads and verify no nested reentrancy failure;
- test claims pending during transfer, reassignment, feed, and attempted breed;
- fuzz Land ownership epochs and population accounting;
- enforce the cumulative Sheep collection/lifecycle caps;
- compare source bytecode and deployed bytecode.

## Server integration

Run the smoke suite with PostgreSQL and Redis:

```bash
pnpm test:integration
```

Add full browser/server/chain tests for:

1. Privy authentication and wallet binding;
2. gather → settlement prompt → confirmed ERC-1155 balance;
3. live Bandage/Plank input burns and outputs;
4. hunt energy/health commit and optional reward confirmation;
5. medicine burn → one-time +40 health;
6. every shop purchase and second-stage resource settlement;
7. roster synchronization after NFT transfer;
8. lifecycle activate/assign/feed/auto-feed/claim and expired-claim recovery;
9. marketplace list/revoke/stale/buy/withdraw;
10. restart, indexer rewind, and job-worker idempotency.

## Unity verification

```bash
pnpm build:unity
pnpm dev
```

Verify in the real browser:

- manifest/loader/WASM all load without console errors;
- stripped collider classes are preserved;
- WASD sends direction and server snapshots move the ranger;
- legs/arms/bob/facing visibly animate;
- remote players and node availability update;
- clicking a node reaches the wallet settlement flow;
- resizing, camera drag, and zoom work;
- overlays remain usable at supported viewport sizes;
- a rejected or disconnected action is not presented as confirmed.

## Robinhood deployment sequence

1. Freeze the release commit and generate bytecode/ABIs.
2. Run unit, fuzz, invariant, integration, and economic-bound tests.
3. Deploy with a dedicated deployer and recorded parameters.
4. Configure roles and one-time dependencies, then read them back.
5. Retire/revoke old issuer roles deliberately.
6. Update the manifest and export ABIs/addresses.
7. Configure web, API signer, and indexer.
8. Verify source/bytecode and publish deployment transactions.
9. Run a fresh-wallet live checklist for every player flow.
10. Reconcile supply partitions, pool reserves, treasury receipts, and event journal.
11. Publish NFT media and verify/freeze only after the media gate passes.
12. Update this GitBook from observed behavior.

Never run a migration script against the shared testnet merely to see whether it works. Dry-run/fork it, inspect state deltas and roles, then broadcast with an explicit rollback/retirement plan.
