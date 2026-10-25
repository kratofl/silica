# Findings

Facts collected on 2026-10-25, before the benchmark harness existed. All fps numbers are read by eye from the in-game counter. Treat them as rough until Phase 0 can reproduce them.

## Instances

Both live under `~/Library/Application Support/ModrinthApp/profiles/`.

| | Vanilla Enhanced 26_2 1.0.1 | MacOS Optimized |
|---|---|---|
| Mods | about 90 | 27 |
| Distant Horizons | yes, LOD radius 128 (was 256) | no |
| Render distance | 16 | 12 |
| Resource packs | about 20, incl. Reimagined and Fresh Animations | 3 small ones |
| 4K, no shaders | 80 fps | 160 fps |

The mod and settings difference alone halves the frame rate at the same resolution on the same hardware.

## Shader numbers

- Vanilla Enhanced, 4K, Complementary Unbound r5.9.3 with Euphoria Patches 1.10.5: first reported at about 60 fps. A later reading of 25 fps was taken 38 seconds after toggling shaders on, while Iris, Sodium and Distant Horizons were still rebuilding. Neither number is clean.
- Reference PC: RTX 4080 Super, 1440p, Complementary, about 120 fps.

## Shader settings the owner uses

Complementary Unbound, tuned toward the Reimagined look: Reimagined clouds (`CLOUD_STYLE_DEFINE=1`, `DOUBLE_REIM_CLOUDS`), pixelated shadows, AO and block light. The owner keeps Unbound for its water and night sky (nebulae, aurora, shooting stars, twinkling stars). Volumetric light is boosted at night (`LIGHTSHAFT_NIGHT_I=200`, `LIGHTSHAFT_SMOKE`). Shadow distance 128.

Suspected cost order at 4K, unmeasured: volumetric light, shadows, water reflections, then the night sky (cheap, sky pixels only).

## Mod suspects in Vanilla Enhanced

Unmeasured guesses, to be checked in Phase 1:
1. Distant Horizons together with Iris (LODs go through the shader pipeline).
2. Fresh Animations on Entity Model Features (CPU animation per mob per frame).
3. The particle mods together: Subtle Effects, Particle Effects, Particle Interactions, Wakes, Footprint Particle, Cool Rain, Immersive Storms.
4. LambDynamicLights with shaders (chunk rebuilds on held light changes).

## JVM

- Zulu 25.0.4.1 with `-XX:+UseZGC -XX:+ZGenerational` crashed with SIGBUS inside ZGC (`ZFlipAgePagesTask`) about 70 seconds after launch. Back to G1.
- Distant Horizons warns about G1. That affects stutter, not average fps.

## Changes applied to Vanilla Enhanced

- Distant Horizons LOD radius 256 to 128, DH SSAO off. Backup at `config/DistantHorizons.toml.bak-fps`.
- Fullscreen resolution was tried at 2560x1440. The owner prefers native 4K. Backup at `options.txt.bak-fps`.
