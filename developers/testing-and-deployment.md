# Testing and deployment

## Verification commands

```bash
pnpm typecheck
pnpm test
pnpm build
```

The root test command runs Python economic tests, deterministic game-core tests, and Foundry contract tests. The full repository check additionally validates and generates NFT metadata:

```bash
pnpm check
```

Run the API smoke test with local PostgreSQL and Redis available:

```bash
pnpm test:integration
```

## Contract test requirements

Contract tests must cover happy paths, reverts, fuzzing, and supply/ownership invariants. Delayed actions need realistic timestamp **and block-number** progression. A test that only warps time will miss expiry defects such as the current breeding blocker.

## Unity

```bash
pnpm build:unity
```

After the build, start the web app and verify canvas load, movement animation, server snapshots, bridge actions, resize behavior, and browser console errors.

## Robinhood Testnet deployment

Use the Foundry deployment scripts and the existing deployment runbook. After deployment:

1. verify roles and ownership;
2. configure and freeze lifecycle sources;
3. export ABIs and addresses;
4. update web and indexer configuration;
5. record deployment block and transaction hashes;
6. run live read/call smoke checks;
7. mint, assign, feed, claim, trade, and reconcile with a fresh wallet;
8. update this book only after the behavior is proven.
