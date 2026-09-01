# Unity WebGL client

The Unity 6 project is a runtime-generated voxel presentation embedded in the Next.js `/world` page.

## Implemented presentation

- voxel terrain and homestead construction;
- forests, mountains, grassland, and swamp biome presentation;
- ranger model with movement-facing rotation, alternating limb swing, footfall bob, and hat bounce;
- pasture animals and wandering behavior;
- orbit camera and responsive WebGL canvas;
- remote player and resource-node snapshots;
- gather interaction;
- combat encounter/results presentation;
- equipment and homestead state updates.

## Bridge

`WolfBridge.jslib` sends Unity input to the page. `NetworkBridge.cs` receives authentication/bootstrap state, world snapshots, action results, homestead state, combat data, biome changes, and equipment updates.

The bridge should remain a transport boundary. Unity must not calculate token rewards or assume a wallet transaction succeeded.

## Building the player

Use `pnpm build:unity`. The script finds the Unity editor, builds WebGL through `WebGLBuilder`, and copies the output into the web application's public assets. A Hub login alone is insufficient if the editor lacks a valid license or the WebGL module.

See the existing Unity build runbook under `docs/runbooks/UNITY_WEBGL_BUILD.md` for editor installation and failure recovery.
