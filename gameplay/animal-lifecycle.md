# Animals, hunger, and Sheep yield

`GreenwoodAnimalLifecycle` is the canonical contract for eligible Wolf/Sheep activation, Land assignment, feeding, Sheep `$WOOL` production, theft selection, and intended Sheep breeding. The retired server `feed`, `produce`, and `breed` endpoints reject requests.

## Eligibility

The lifecycle checks token IDs against three frozen sources: historical paid V3, historical paid V4, and V5. Bred Sheep are registered directly by the lifecycle.

V5 contains a public `testnetMakeEligible` helper. This is a testnet vulnerability, not a player feature, because any caller can mark arbitrary existing IDs in the source as eligible.

## Activation and Land

- A V5 Wolf or Sheep is activated during minting and receives a three-day feed window.
- An eligible historical animal must be activated once by its current owner.
- Activation does not assign Land.
- The owner calls `assignAnimalToLand` and must own both the animal and eligible Land.
- V5 Land has Grassland biome, 10,000 quality basis points, and capacity 20.
- Land ownership epochs invalidate old assignments after transfer and prevent stale occupancy from permanently consuming capacity.

Land quality, Sheep quality, and the NFT's stored wool-production trait do **not** modify the current lifecycle yield. Rules V2 pays the same fixed rate to every qualifying Sheep.

## Hunger

The current strategy suspends production instead of destroying NFTs.

- Sheep feed costs 2 Grass per day.
- Wolf feed costs 3 Meat per day.
- Feed time can extend no farther than 28 days from the current block time.
- Auto-feed costs 10 `$WOOL` burned for each seven-day period.
- Hungry Sheep stop accruing `$WOOL` and cannot breed.
- Hungry Wolves cannot receive stolen `$WOOL` or lambs.
- `finalizeStarvation` always reverts with `HungerDoesNotCauseDeath`.

The animal `deathFinalizedAt` field and starvation event remain in the storage/interface, but no current Rules V2 path sets starvation death. The server's player health is unrelated to NFT life.

## Sheep `$WOOL` production

A Sheep accrues 3 `$WOOL` per eligible day while all of these are true:

- it is lifecycle-eligible and activated;
- the connected wallet owns it;
- it has a valid assignment to Land held by the same wallet;
- its feed window has not ended;
- it is not breeding.

Accrual stops at `fedUntil`. A hungry Sheep can still claim the yield accumulated before it became hungry.

### Claim flow

1. `requestWoolClaim` stores the claimant, accrual end, and entropy block three blocks ahead.
2. After that block, `resolveWoolClaim` calculates elapsed eligible seconds at 3 `$WOOL` per day.
3. The lifecycle increments the 900-million gameplay issuance counter.
4. It mints the complete amount to the owner or, on a 20% theft roll, to a different activated, living, fed Wolf owner.

Theft redistributes the complete claim; it does not burn supply. The selection probes at most 32 activated Wolf IDs. If it finds no qualifying different owner, the Sheep owner receives the claim.

Entropy expires 200 blocks after the selected block. Anyone can cancel an expired Sheep claim, after which the owner can request again. The web helper performs this recovery and handles a previous owner's pending request after transfer.

## Sheep breeding

Sheep breeding is live on the V2 lifecycle contract and accessible through the in-game **Breeding House** UI:

- **Requirements:** Same owner, two distinct fed Sheep assigned to the same Land parcel with available capacity.
- **Cooldown & Limits:** Neither parent may currently be breeding, on cooldown (7 days after previous breeding), or have reached the 20-breed maximum.
- **Costs:** 
  - 10 `$WOOL` treasury fee transferred directly to protocol treasury.
  - Optional speedup: burn 100 `$WOOL` per day reduced (up to 6 days reduced, for a 1-day minimum duration).
- **Duration:** 7-day base duration, reduced down to 1–6 days if speedup `$WOOL` is burned.
- **Wool Production:** Paused for both parent Sheep during the breeding period.
- **Offspring & Theft:** When resolved, a new Sheep NFT is minted and activated. 10% chance of lamb theft to an activated, fed Wolf belonging to a different owner.
- **Secondary Cap:** Maximum 10,000 secondary Sheep can be bred globally.

## Wolves today

Wolves can be activated, assigned, fed, and selected as recipients of stolen `$WOOL` (20% chance) or stolen bred lambs (10% chance). Hungry Wolves are ineligible to steal. There is no current Wolf breeding contract path, Wolf yield, or direct integration between an owned Wolf NFT and the server hunt outcome.
