# `$WOOL` economy

`$WOOL` is `WoolToken`, an 18-decimal ERC-20 with an immutable maximum supply of one billion.

It is not the server inventory field named `wool`, and it is not ERC-1155 Fleece (`resource ID 7`). Sheep lifecycle claims mint the ERC-20 directly.

## Issuance partitions

### Genesis liquidity: maximum 100 million

For every cumulative V3–V5 paid mint:

- the buyer pays `0.0001 ETH`;
- `0.00002 ETH` goes to the team treasury;
- `0.00008 ETH` goes to the protocol liquidity pool;
- `10,000 WOOL` is minted into that pool.

At 10,000 paid mints this reaches exactly 100 million pool `$WOOL`. The buyer gets the NFT, not these tokens.

### Gameplay: maximum 900 million

The lifecycle tracks `gameplayWoolMinted` separately and rejects a claim that would exceed 900 million. Sheep yield is 3 `$WOOL` per eligible day, calculated by seconds.

`WoolToken` independently rejects any mint above the global one-billion cap. Both checks must pass.

## Active sinks and transfers

| Action | Amount | Supply effect |
|---|---:|---|
| Animal auto-feed | 10 `$WOOL` per 7 days | Burned |
| Breeding speedup | 100 `$WOOL` per day removed (up to 6d) | Burned |
| Shop Health Elixir | 5 `$WOOL` | 100% burned |
| Shop Energy Flask | 3 `$WOOL` | 100% burned |
| Shop Timber Crate | 10 `$WOOL` | 100% burned |
| Shop Stone Crate | 10 `$WOOL` | 100% burned |
| Shop Master Bundle | 25 `$WOOL` | 100% burned |
| Base breeding fee | 10 `$WOOL` | Transferred to treasury, not burned |
| Marketplace fee | 3% | Transferred/credited to treasury, not burned |
| Sheep claim theft | 20% chance of full claim | Redistribution, not a burn |
| Lamb theft | 10% chance of full lamb | Ownership redistribution, not a token burn |
| Pool swap fee | 0.30% base, up to 2% | Retained in pool accounting, not burned |

The active chain configuration reported 100% burn for all five shop items during the audit. The Health/Energy cards still display obsolete “50% Burned · 50% Treasury” text and should be corrected.

## Protocol liquidity pool

`WoolLiquidityPool` is a custom protocol-owned discrete-bin pool:

- initial center price: `8,000,000,000` wei per 1 `$WOOL`;
- bin step: 0.25%;
- base fee: 0.30%;
- dynamic fee cap: 2%;
- genesis liquidity manager can be set only once;
- each mint spreads ETH and `$WOOL` around the current active bin.

The pool is not an LP-token contract and does not issue positions to players. It has custom loop/accounting logic and requires dedicated economic/security review before value-bearing release.

The web client currently probes the V5, V3, and V4 protocol pools and routes quotes, approvals, and swaps to whichever reports the most ETH reserve. New V5 mints always add liquidity to the V5 pool. This compatibility behavior can move user swaps to a historical pool and should be replaced by an explicit, governance-controlled active-pool registry before production.

## Supply horizon

The strongest guarantee is the cap. A conservative upper bound uses 20,000 productive Sheep at 3 `$WOOL` per day:

```text
900,000,000 / (20,000 × 3) = 15,000 days ≈ 41.1 years
```

The intended paid distribution allows 8,000 genesis Sheep plus 10,000 bred Sheep, which gives about 45.7 years at continuous full eligibility. The current public eligibility helper weakens that intended boundary on testnet, so the 20,000-Sheep bound remains the safer contract-level calculation.

## What the horizon does not prove

A long issuance horizon does not guarantee liquidity, demand, fair distribution, or a sustainable price. Claim theft merely changes recipients. Resource feeding can avoid a `$WOOL` burn entirely. Auto-feeding a Sheep burns 10 over a week while the Sheep can emit 21.

Before mainnet, operations must track and publish:

- gameplay issuance and unissued budget;
- burns by sink and net daily supply change;
- Grass/Meat settlements versus feed burns;
- claim/theft concentration;
- pool depth, fee accumulation, price movement, and failed swaps;
- abandoned signed settlements and wallet-confirmation friction;
- active productive Sheep and Land-capacity utilization.
