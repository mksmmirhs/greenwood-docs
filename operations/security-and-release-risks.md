# Security and release risks

## Critical before another lifecycle release

- Redesign breeding randomness/timing so resolution is possible after the wait.
- Clear parent breeding state on every cancellation/recovery path.
- Remove the nested reentrancy-guard overload.
- Restrict or remove `testnetMakeEligible`.
- Keep lifecycle `$WOOL` allowance checks on auto-feed and add the equivalent approval flow before breeding is re-enabled.
- Add live end-to-end tests for activation, assignment, resource feed, auto-feed, claim request/resolve, breeding, transfers, and Land ownership change.

## Critical before mainnet

- Replace blockhash-based randomness with a supported VRF or independently committed randomness beacon.
- Complete an external audit of token roles, supply partitions, marketplace escrow, liquidity math, lifecycle accounting, settlement signatures, and treasury controls.
- Move ownership and treasury authority to a multisig with documented signer rotation and emergency procedures.
- Prove all token media and metadata are immutable, available from redundant content-addressed storage, and covered by provenance manifests.
- Remove testnet migration/admin helpers.
- Establish a testnet reset policy and never imply test assets have guaranteed value.

## Application risks

- Privy account and wallet binding must remain separate checks; an authenticated user is not automatically an owner of any submitted wallet.
- Settlement keys require rate limits, nonce durability, monitoring, and immediate rotation procedures.
- WebSocket actions require idempotency, sequence validation, range/cooldown enforcement, and abuse throttles.
- Indexer lag or reorg must never grant assets; projection state must reconcile back to chain truth.
- Admin endpoints need allowlisted identities, complete audit logs, and least privilege.

## Operational gates

Do not call the project a full MMO release until it demonstrates load, backup restore, indexer rebuild, signer rotation, contract pause/unpause, moderation, economic reconciliation, and stable multi-day player testing.

## Secret hygiene

Never paste or commit a Privy app secret, settlement private key, deployer key, or treasury key. If a secret has appeared in chat, logs, screenshots, or a commit, rotate it and invalidate the old value.
