# Contracts and deployment

## Current Robinhood Testnet deployment

Chain ID: `46630`

- Animal Lifecycle V2: `0xF7578ADf443c6aF0b571731D9E69aa0C4a35cC2E`
- Genesis Minter V5: `0xddE9C75eEf6f014511b754e324880656AC7fAD22`
- Protocol Liquidity Pool: `0x87d81CA8c4BF2Ec4482E6fD93a586A5D18EF3Cba`
- `$WOOL`: `0xA12C21cE750DAaA6B7755Dc12324493F32BD70E8`
- Wolf NFT: `0x1609626BeACA7670f995db1937213B37F377747`
- Sheep NFT: `0xdEf7668773caaA04D3de483FEb8Cb7265e418b6c`
- Land NFT: `0x694E4d2A554dc4A122b482821C334ff36c09b23D`
- Greenwood Resources: `0x584f6202dB3D1d47791324c3E44cC27c260122D8`
- Greenwood Settlement: `0xd129E7142fDE34e38A6E4A84fc6C0eEd6657dB12`
- Marketplace Escrow: `0x7A722D3984eB24501f5B0eD540Ee952611FA09d0`
- Game Shop: `0x5EAc5C5A2aa2c8F98B72a66C815E133b7D512472`
- Treasury: `0x7DC0F5783C59e6c270903eE1183E5E329f846bD1`

The machine-readable source is `contracts/deployments/46630.json`. Client defaults are exported by `packages/contracts-abi`.

## Contract responsibilities

### Tokens

- `WoolToken` — capped ERC-20 with role-gated minting and holder/allowance burns.
- `WolfNFT` — genesis Wolf identity and genetic traits.
- `SheepNFT` — genesis and bred Sheep, generation and parent lineage.
- `LandNFT` — unique coordinates, biome, quality, fertility, and capacity.
- `GreenwoodResources` — ERC-1155 resource balances burned by lifecycle and crafting flows.

### Economy and settlement

- `GenesisMinterV5` — paid random mint, caps, lifecycle registration, ETH routing, and sale-backed liquidity.
- `WoolLiquidityPool` — protocol-owned `$WOOL`/ETH swaps and liquidity accounting.
- `MarketplaceEscrow` — supported NFT listings and purchases in ETH or `$WOOL`.
- `GameShop` — contract-backed consumable purchases.
- `GreenwoodSettlement` — EIP-712-style server-authorized resource and state settlement.
- `Treasury` — protocol custody and controlled withdrawals.

### Gameplay

- `GreenwoodAnimalLifecycle` — activation, Land assignment, feeding, `$WOOL` production, delayed claim theft, and breeding.

`GenesisSummoner` V3/V4 addresses remain historical eligibility/liquidity sources. They are not the active sale contract.

## Deployment rules

After any redeploy, update all of these together:

1. `contracts/deployments/46630.json`;
2. `packages/contracts-abi/src/index.ts`;
3. web public environment values;
4. indexer environment values and start block;
5. this GitBook deployment page;
6. contract export artifacts and tests.
