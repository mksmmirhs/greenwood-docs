# Player transaction flows

## V5 genesis mint

1. The wallet calls `summon(account)` with exactly `0.0001 ETH`.
2. The minter chooses among the remaining Land, Wolf, and Sheep quotas.
3. The appropriate NFT contract mints the token.
4. Wolf or Sheep is registered and activated in the lifecycle.
5. 20% of ETH goes to treasury.
6. 80% of ETH and 10,000 newly minted `$WOOL` go to the protocol pool.

Land is eligible but is not an animal and therefore has no lifecycle activation.

## First-time animal setup

1. Connect the wallet that owns the animal and Land.
2. For a legacy eligible animal, call lifecycle activation. V5 animals are normally already activated.
3. Select owner-held Land with remaining capacity.
4. Call `assignAnimalToLand`.
5. Feed before `fedUntil` expires.

The web **Bulk Setup** action must include both unactivated animals and activated animals whose `assignedLandId` is zero.

## Claiming Sheep `$WOOL`

Requirements:

- the connected wallet owns the Sheep;
- lifecycle state is activated;
- the Sheep has a valid assignment to Land owned by the same wallet;
- the Sheep is not breeding;
- eligible time exists after `lastWoolClaimAt`.

Flow:

1. The UI calls `requestWoolClaim(sheepId)`.
2. The transaction records the accrual checkpoint and an entropy block three blocks ahead.
3. The UI waits until after that block.
4. The UI calls `resolveWoolClaim(sheepId)`.
5. The lifecycle computes `elapsed eligible seconds × 3 WOOL/day`.
6. It mints either to the Sheep owner or, on the 20% theft outcome, to a qualifying fed Wolf owner.

Anyone may resolve a pending claim, but the recorded claimant remains the intended owner. If entropy is more than 200 blocks old, anyone may cancel the expired request and the owner may request again.

If the UI reports zero claimable yield for a new Sheep, inspect activation, `assignedLandId`, Land ownership epoch, feed time, breeding state, and pending request before assuming mint permissions are broken.

## Feeding with resources

1. Approve the lifecycle as an ERC-1155 operator for the resource collection if required.
2. Call `feedSheep(id, days)` or `feedWolf(id, days)`.
3. The lifecycle burns 2 Grass/day for Sheep or 3 Meat/day for Wolves.
4. `fedUntil` extends from the later of now or the existing feed deadline, within the 28-day maximum.

## Auto-feeding with `$WOOL`

1. Approve the lifecycle to spend the required `$WOOL`.
2. Call the animal-specific auto-feed function with one to four seven-day periods.
3. The lifecycle burns 10 `$WOOL` per period and extends `fedUntil`.

## Marketplace

The seller approves the NFT, creates an expiring supported-collection listing, and escrows the NFT. A buyer submits ETH or approved `$WOOL`. The marketplace settles ownership, payment, and protocol fee atomically.
