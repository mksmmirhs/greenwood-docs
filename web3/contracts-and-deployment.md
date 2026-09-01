# Contracts and deployment

Network: **Robinhood Chain Testnet**, chain ID `46630`.

The address source of truth is `contracts/deployments/46630.json`, exported to `packages/contracts-abi`. Environment variables may override browser defaults, so release checks must verify the actual runtime configuration too.

## Current addresses

| Contract | Address |
|---|---|
| Animal Lifecycle V2 | `0xF7578ADf443c6aF0b571731D9E69aa0C4a35cC2E` |
| Genesis Minter V5 | `0xddE9C75eEf6f014511b754e324880656AC7fAD22` |
| Protocol Liquidity Pool | `0x87d81CA8c4BF2Ec4482E6fD93a586A5D18EF3Cba` |
| `$WOOL` | `0xA12C21cE750DAaA6B7755Dc12324493F32BD70E8` |
| Wolf NFT | `0x1609626BeACA7670f995dbf1937213B37F377747` |
| Sheep NFT | `0xdEf7668773caaA04D3de483FEb8Cb7265e418b6c` |
| Land NFT | `0x694E4d2A554dc4A122b482821C334ff36c09b23D` |
| Greenwood Resources | `0x584f6202dB3D1d47791324c3E44cC27c260122D8` |
| Greenwood Settlement | `0xd129E7142fDE34e38A6E4A84fc6C0eEd6657dB12` |
| Marketplace | `0x7A722D3984eB24501f5B0eD540Ee952611FA09d0` |
| Game Shop | `0x5EAc5C5A2aa2c8F98B72a66C815E133b7D512472` |
| Treasury | `0x7DC0F5783C59e6c270903eE1183E5E329f846bD1` |

The previous version of this book omitted one hexadecimal character from the Wolf address. The address above matches the manifest, ABI package, environment examples, and live contract.

Historical paid eligibility sources:

- V3: `0x2AC9eF587061f382aB30Ff2B0f6FE4e43bEa8d7E`
- V4: `0xa9C77001260FC7Cb585d7D61dEb72A892F968aac`

V5 is the active sale. V3 and V4 remain configured only so already-paid assets can remain eligible.

## Responsibilities

### Tokens

- `WoolToken` — capped ERC-20 with role-gated minting, holder burn, and allowance burn.
- `WolfNFT` — ERC-721Enumerable Wolf genetics and lineage; collection maximum 50,000, but current lifecycle has no Wolf breeding path.
- `SheepNFT` — ERC-721Enumerable genesis/bred Sheep, maximum 20,000.
- `LandNFT` — ERC-721Enumerable parcels, unique coordinates, biome, quality, and capacity; maximum 2,000.
- `GreenwoodResources` — 11 ERC-1155 resource IDs, each capped at one billion whole units.

### Economy

- `GenesisMinterV5` — random paid distribution, cumulative quotas, lifecycle registration, treasury ETH, and sale-backed liquidity.
- `WoolLiquidityPool` — custom protocol-owned discrete-bin pool with dynamic fees.
- `MarketplaceEscrow` — approval-based listing and atomic purchase for ETH or `$WOOL`; despite its name, listing does not transfer the NFT into custody.
- `GameShop` — burns approved `$WOOL` and emits a purchase event. The server supplies the gameplay effect after verifying that event.
- `Treasury` — owner-controlled custody/withdrawal.

### Gameplay settlement

- `GreenwoodSettlement` — EIP-712 typed settlements, active rules version, per-player nonce replay protection, deadlines, and exact ERC-1155 burn/mint execution.
- `GreenwoodAnimalLifecycle` — paid-source eligibility, activation, Land assignment, feed burns, fixed Sheep emissions, theft, and the currently blocked breeding state machine.

## Role relationships

- V5 holds the collection minter roles and `$WOOL` minter role needed for sale-backed liquidity.
- Lifecycle holds Sheep breeder, `$WOOL` minter, and resource burner roles.
- Settlement holds resource minter/burner permissions and accepts signatures only from `SETTLEMENT_SIGNER_ROLE`.
- The pool accepts genesis liquidity only from its one-time liquidity manager.
- Collection metadata and pause powers remain role-controlled; their two-day default-admin transfer delay does not make metadata immutable.

## Deployment discipline

Every contract change must update and verify, as one release:

1. the Foundry deployment/migration script;
2. roles, admin ownership, pause state, and immutable dependencies;
3. `contracts/deployments/46630.json`;
4. generated ABIs and `packages/contracts-abi/src/index.ts`;
5. web/API/indexer environment values;
6. indexer start block and watched address set;
7. live read/call smoke tests;
8. this page and the current-status page.

Do not overwrite a manifest merely because a script compiled. Record deployment block, transaction hashes, bytecode verification, role checks, migration counters, and rollback/retirement decisions.
