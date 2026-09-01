# NFT collections and media

The repository has complete local rehearsal assets, but local generation and deployed media delivery are separate states.

## Collection contract namespaces

| Collection | Genesis token range/cap | Secondary range/cap | Contract maximum |
|---|---:|---:|---:|
| Wolf | 1–2,500 | starts at 2,501 | 50,000 |
| Sheep | 1–10,000 | 10,001–20,000 | 20,000 |
| Land | 1–2,000 | none | 2,000 |

These are collection caps, not the active paid-sale distribution.

## Active paid distribution

V5 migrates V3/V4 paid counters and preserves one cumulative 10,000-mint sale:

- 1,000 Wolves;
- 1,000 Lands;
- 8,000 Sheep.

The collection namespaces are larger because the contracts were designed for historical allocations and secondary mint ranges. Do not describe 2,500 Wolves, 10,000 genesis Sheep, and 2,000 Lands as the V5 sale allocation.

Every V5 Land is currently minted as Grassland, quality `10,000`, and carrying capacity `20`. Every V5 Wolf uses rarity tier `1`. Every V5 Sheep uses quality `10,000` and stored wool production rate `10`; lifecycle Rules V2 ignores that stored rate and pays its fixed 3 `$WOOL`/day.

## Local generated set

`art/collection-config.json` and the generator produce and validate:

- 2,500 Wolf images/metadata;
- 10,000 Sheep images/metadata;
- 2,000 Land images/metadata;
- 14,500 unique 256×256 WebP images total.

Generation is deterministic from a committed seed. The validator checks counts, hashes, image paths, unique generated images, unique Land coordinates, and the collection provenance root.

This proves reproducible local files. It does not prove that deployed token URIs serve them.

## Deployed metadata status

At the audit point:

- Wolf token URI prefix returned `https://api.wolf.game/wolf/`;
- Sheep returned `https://api.wolf.game/sheep/`;
- Land returned `https://api.wolf.game/land/`;
- `baseURIFrozen` was `false` on Wolf, Sheep, and Land.

The source `metadata/genesis_manifest.json` still contains a placeholder-looking IPFS base value, while generated metadata uses relative local image paths. The web dashboard also has hand-authored sample cards that do not map one-to-one to contract traits.

Therefore NFT media is **generated locally but not proven published, immutable, or redundant**.

## Release gate for NFT media

Before a public mint is represented as final art:

1. map every on-chain token ID to the intended generated metadata;
2. reconcile generated Land coordinates/traits with contract-minted coordinates/traits;
3. publish images and metadata to content-addressed redundant storage;
4. replace relative/placeholder URIs with tested production URIs;
5. publish provenance root and generation version;
6. verify random token URIs from multiple gateways;
7. freeze base URIs only after recovery and governance approval;
8. document what happens to bred Sheep media beyond genesis ID 10,000.
