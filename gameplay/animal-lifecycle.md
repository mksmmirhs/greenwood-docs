# Animals, hunger, and breeding

`GreenwoodAnimalLifecycle` is the canonical state machine for Wolf and Sheep care. The API no longer simulates feeding, fleece production, or breeding; its old economy endpoints reject those actions and direct the client to the chain.

## Eligibility, activation, and Land

An NFT must come from a frozen eligibility source or be a bred Sheep registered by the lifecycle. Activation starts a three-day initial feed window. A paid V5 animal is activated during mint; legacy eligible animals require `activateLegacyAnimal`.

Activation does **not** assign Land. The owner must call `assignAnimalToLand`. This is why a newly minted, activated Sheep may display zero claimable `$WOOL`: production requires a valid Land assignment.

Assignments are protected by a Land ownership epoch. If Land ownership changes, old animal assignments become invalid instead of silently benefiting the new owner.

## Hunger

Rules V2 use economic suspension rather than NFT death:

- Sheep consume 2 Grass per fed day.
- Wolves consume 3 Meat per fed day.
- Feeding can extend at most 28 days ahead of current time.
- Auto-feed burns 10 `$WOOL` for each seven-day period.
- A hungry Sheep stops accruing `$WOOL` and cannot breed.
- A hungry Wolf cannot receive stolen `$WOOL` or lambs.
- `finalizeStarvation` always reverts; hunger does not destroy an NFT.

Health and stamina are separate server-side systems. Hunger does not replace combat health, and server health does not mint or burn an NFT.

## Sheep production

A Sheep accrues 3 `$WOOL` per day only while it is activated, assigned to valid owner-held Land, fed, and not breeding. Claims use two transactions:

1. `requestWoolClaim` checkpoints the eligible accrual and selects a future entropy block.
2. After that block, `resolveWoolClaim` mints the exact checkpointed amount.

There is a 20% chance that the complete claim is sent to a randomly selected eligible, living, fed Wolf owner. If no qualifying Wolf is found, the Sheep owner receives it.

## Intended breeding rules

The current source expresses these rules:

- both Sheep must have the same owner and valid Land assignment;
- both must be fed, not breeding, within the breed-count limit, and off cooldown;
- Land must have capacity for another animal;
- 10 `$WOOL` goes to the team treasury;
- the player may burn 100 `$WOOL` per day to reduce seven days down to a one-day minimum;
- parent production pauses during breeding;
- a resolved lamb has a 10% chance of going to a qualifying Wolf owner;
- at most 10,000 secondary Sheep may be bred;
- each parent has a seven-day cooldown following the breeding ready time.

## Deployed breeding status: blocked

The timing and entropy model cannot currently resolve after a real one-to-seven-day wait. Do not initiate breeding with valuable test state until a corrected lifecycle is deployed; expired cancellation can leave parent Sheep marked as breeding.
