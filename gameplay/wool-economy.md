# `$WOOL` economy

`$WOOL` is an ERC-20 with 18 decimals and an immutable maximum supply of one billion tokens.

## Supply partitions

The design separates the cap into two issuance budgets:

- up to 100 million `$WOOL` for genesis-sale-backed protocol liquidity;
- up to 900 million `$WOOL` for lifecycle gameplay emissions.

The cap is also enforced by `WoolToken`, so neither path can mint beyond one billion total.

## Genesis liquidity

Each V5 mint costs `0.0001 ETH`:

- 20% ETH is sent to the team treasury;
- 80% ETH is added to the protocol-owned liquidity pool;
- 10,000 `$WOOL` is minted directly to the pool and paired with that ETH.

At the 10,000-mint cap this creates the planned 100 million `$WOOL` genesis liquidity allocation. The player does not receive this liquidity `$WOOL` as a mint reward.

## Gameplay emissions

Each productive Sheep accrues 3 `$WOOL` per day. Claims increase `gameplayWoolMinted` and stop permanently when the 900 million gameplay budget is exhausted.

At the nominal rate, one continuously fed Sheep produces 1,095 `$WOOL` per year. Actual aggregate issuance depends on active Sheep count, feeding uptime, breeding pauses, claim behavior, and the 10,000 secondary Sheep cap.

## Sinks and transfers

- Auto-feed burns 10 `$WOOL` per seven days.
- Breeding speedup burns 100 `$WOOL` per day removed.
- The base 10 `$WOOL` breeding fee transfers to treasury; it is not burned.
- Marketplace and shop fees route according to their deployed contracts.
- Protocol swaps charge a dynamic liquidity fee.

The 20% Sheep-claim theft and 10% lamb theft redistribute value; they do not reduce total supply.

## Sustainability warning

A hard cap prevents infinite issuance but does not by itself create a healthy multi-year economy. Before mainnet, the team must simulate realistic player counts and feeding behavior, tune sinks against issuance, monitor concentration and pool depth, and establish a versioned governance process. The stale constants in older design documents must not be used as runtime truth.
