---
name: modrinth-instance
description: Use when reading or changing the owner's Modrinth Minecraft instances, their mods, options.txt, shader settings, Distant Horizons config, JVM arguments, or when reading game logs and crash files.
---

## Modrinth instances

Instances live in `~/Library/Application Support/ModrinthApp/profiles/<name>/`. The two that matter are `Vanilla Enhanced 26_2 1.0.1` (full pack) and `MacOS Optimized` (lean pack).

### Where things are

- `mods/`: jars. A disabled mod ends in `.jar.disabled`. Disable by renaming, never delete.
- `options.txt`: vanilla video settings (`renderDistance`, `maxFps`, `fullscreenResolution`, `resourcePacks`).
- `config/iris.properties`: `enableShaders` and the active `shaderPack`.
- `shaderpacks/<pack>.txt`: the shader settings for that pack. Only non-default values are stored.
- `config/sodium-options.json`, `config/sodium-extra-options.json`: Sodium settings.
- `config/DistantHorizons.toml`: DH settings. Graphics are under `[client.advanced.graphics.*]`.
- `logs/latest.log`: current or last session. Look for `Using shaderpack`, `Garbage collectors` and `OpenGL Version`.
- `hs_err_pid*.log` in the instance root: JVM crash files.

### JVM arguments and memory

These live in Modrinth's database (`~/Library/Application Support/ModrinthApp/app.db`, table `instance_launch_overrides`, binary encoded). Read it with `sqlite3 -readonly` if needed. Never write to it. Ask the owner to change JVM arguments in the Modrinth UI.

### Before changing anything

1. Ask the owner first. These are their game installs.
2. Check the game is not running: `pgrep -f net.fabricmc`. Minecraft rewrites `options.txt` on exit and overwrites edits made while it runs.
3. Copy the file to `<file>.bak-<reason>` first.
4. Change one thing, then verify the new value with `grep`.

### Never

- Upload or paste `hs_err_pid*.log` anywhere. It contains `--accessToken` in plain text.
- Press or recommend Crash Assistant's "Upload all logs" button for the same reason.
- Kill Minecraft or the Modrinth app by name. Only stop processes you started, by PID.
