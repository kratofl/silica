---
name: fps-measurement
description: Use when measuring, comparing or reporting Minecraft frame rates, judging whether a mod, setting or shader option is worth its cost, or reading fps numbers the owner reports.
---

## Measuring fps

Until the Phase 0 harness exists, measurements are manual: the owner plays, the agent sets up and records. Once the harness exists, prefer it and update this skill.

### Manual protocol

1. Same instance, same world, same spot, same view direction. Write down the coordinates (F3) and time of day, or fix them with `/tp`, `/time set` and `/weather clear`.
2. After launching or after toggling shaders, wait **2 minutes** before reading anything. Iris compiles programs, Sodium rebuilds every chunk mesh and Distant Horizons rebuilds its LODs. Numbers from that window are worthless.
3. Window in the foreground, nothing covering it. macOS throttles occluded windows.
4. Read fps over about 10 seconds and note the low end too, not only the peak.
5. Change exactly one thing. Repeat steps 2 to 4.
6. Record the result in `docs/findings.md` with date, instance, resolution, shader and what changed.

### Reading a reported number

Before trusting a number, ask or check in `logs/latest.log`:
- How long after a shader toggle or launch was it read? Search for `Toggled shaders` and `Render data cleared` timestamps.
- Which resolution? `fullscreenResolution` in `options.txt`, and remember the owner switches between 4K and 1440p.
- Which shaderpack? Search for `Using shaderpack`.

### Interpreting

- No shaders: the render thread (CPU, mods, OpenGL driver overhead) is usually the limit. Resolution barely changes fps. Removing mods helps.
- With shaders: the GPU is usually the limit. Resolution and shader options change fps a lot.
- If lowering the resolution doesn't raise fps, the bottleneck is not the GPU.
