# Rules V2 change log

This page separates the current source from older design material.

## Current Rules V2

- Hunger no longer burns or kills Wolf/Sheep NFTs.
- Hunger suspends Sheep production and breeding; hungry Wolves cannot steal.
- Sheep consume 2 Grass/day; Wolves consume 3 Meat/day.
- `$WOOL` auto-feed costs 10 per seven days.
- Maximum feed ahead increased to 28 days.
- Sheep produce 3 `$WOOL`/day while eligible.
- Claims use delayed on-chain resolution with 20% complete-claim theft.
- Breeding base fee is a flat 10 `$WOOL` transfer to treasury, not a burn.
- Optional breeding speedup burns 100 `$WOOL` per day removed.
- Breeding duration is one to seven days and pauses parent production.
- Lamb theft is 10%.
- Secondary Sheep supply is capped at 10,000.
- Gameplay emissions are capped at 900 million `$WOOL` in addition to the token's one-billion hard cap.

## Retired rules

The following may still appear in older plans or generated artifacts but are not the current lifecycle behavior:

- starvation permadeath from failing to feed;
- a 20 `$WOOL × 1.35^breeds` breeding burn;
- a Grass cost for breeding;
- Wolf breeding;
- a 14-day maximum feed horizon;
- server-mutated feeding, fleece collection, or breeding;
- a 100 million total `$WOOL` cap.

## Compatibility

Historical V3/V4 minters remain eligibility sources for already minted assets. V5 is the active paid mint. The database can project assets from all configured sources, but lifecycle rules are applied only by the current lifecycle address.
