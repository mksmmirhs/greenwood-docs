# Rules V2 change log

This page explains the current strategy where it differs from older plans and UI remnants.

## Current Rules V2

- Animal hunger suspends economics; it does not burn or kill the NFT.
- Sheep burn 2 Grass/day and Wolves burn 3 Meat/day.
- Auto-feed burns 10 `$WOOL` for seven days.
- Feed time may extend up to 28 days ahead.
- Every eligible Sheep has a fixed 3 `$WOOL`/day lifecycle rate.
- Land capacity is a hard assignment limit. The lifecycle does not apply an overcrowding yield multiplier.
- Sheep claims are request/resolve actions with a 20% full-claim theft roll.
- Hungry Wolves cannot receive stolen claims or lambs.
- Breeding is Sheep-only, intended to take one to seven days, and pauses parent production.
- Base breeding fee is 10 `$WOOL` transferred to treasury.
- Optional speedup burns 100 `$WOOL` per day removed.
- Intended lamb theft is 10%; intended secondary cap is 10,000.
- Gameplay `$WOOL` issuance has a 900-million lifecycle budget under the token's one-billion hard cap.
- Server feeding, production, breeding, and building mutations are retired/rejected.
- Durable world resources are signed by the server and finalized by the player's wallet in `GreenwoodSettlement`.

## Retired or non-current design claims

Do not describe these as current behavior:

- starvation permadeath for an unfed NFT;
- exponential `20 × 1.35^breeds` `$WOOL` burn;
- Grass/Food as a breeding input;
- Wolf breeding;
- Sheep trait/Land quality multipliers on current yield;
- overcrowding yield collapse;
- 14-day feed-ahead limit;
- server-side fleece counters that later “claim” `$WOOL`;
- a 100-million total `$WOOL` cap;
- 50% shop burns for Health/Energy;
- a guaranteed NFT starter pack;
- the generic 20-recipe catalog as the live craft surface.

## Current known inconsistencies

- The UI still includes some legacy 50% shop-burn copy.
- The server's live Bandage recipe differs from `game-core`.
- ERC-1155 ID 3 is named `FOOD` in the token and presented as Grass/Fodder in Rules V2.
- ERC-1155 Fleece exists but has no active release issuer and is not ERC-20 `$WOOL`.
- Collection caps (2,500 Wolf, 10,000 genesis Sheep, 2,000 Land) differ from the active paid distribution (1,000/8,000/1,000).

## Compatibility

V3 and V4 remain frozen lifecycle eligibility sources for existing paid assets. V5 is the active sale and automatically registers new animals. Rehearsal assets are intended to remain outside release rewards, but the public V5 testnet eligibility helper currently breaks that guarantee and must be removed/restricted in a replacement deployment.
