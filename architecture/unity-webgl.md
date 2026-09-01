# Unity WebGL client

The active `/world` page embeds a Unity 6 build generated from `apps/unity`. Unity is presentation and input; the server and contracts remain authoritative.

## Current scene

`GreenwoodBootstrap` constructs the scene at runtime with voxel primitives. The current art direction is a bright farm/homestead rather than the older four-quadrant placeholder:

- expansive terrain, clouds, lighting, fog, trees, rocks, and pasture;
- red barn, silo, fields, hay bales, fences, a Sheep pen, and farm blueprint;
- voxel Sheep, Wolves, ranger, resource objects, and decorative creatures;
- orbit camera;
- runtime biome ambience helpers;
- optional equipment and combat presentation objects.

The scene is still procedurally assembled from primitives, not a content-complete MMO world with authored quests, navigation, interiors, animation clips, or streamed zones.

## Ranger movement

`NetworkBridge` sends WASD/arrow direction changes and receives server snapshots. It interpolates player transforms toward server targets.

Every ranger receives `RangerWalkingAnimator`, which measures actual transform movement and applies:

- alternating leg and boot swing;
- opposing arm swing;
- foot lift, torso bob, and hat bounce;
- smooth facing rotation;
- return to idle pose.

Because animation depends on received/interpolated movement, a disconnected or rejected player does not animate as though it moved.

## JavaScript bridge

`WolfBridge.jslib` emits Unity actions into the page. `NetworkBridge.cs` receives:

- connection/bootstrap state;
- player/resource snapshots;
- action results;
- homestead presentation state;
- combat encounter/results;
- biome and equipment changes.

The page currently forwards Unity `world:move`, `world:gather`, `world:craft`, and `world:hunt` actions. Combat receive methods exist, but the active server has no encounter start path.

`Assets/link.xml` preserves `BoxCollider`, `CapsuleCollider`, and `SphereCollider` in WebGL builds because `GameObject.CreatePrimitive` creates them through native code and managed stripping otherwise removes their classes.

## Build

Run:

```bash
pnpm build:unity
```

The build script invokes `WebGLBuilder` and writes the loader, data, framework, WASM, and manifest under `apps/web/public/unity`. Unity Hub being signed in is not sufficient by itself: the exact Unity editor, valid license, and WebGL Build Support module must be installed.

After rebuilding, restart/refresh the web app so its manifest cache-busting timestamp points to the new files. See `docs/runbooks/UNITY_WEBGL_BUILD.md` for recovery steps.
