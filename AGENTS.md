# AGENTS.md

## Project

Silica: make modded Minecraft Java with shaders run well on one specific Mac at 4K, then publish the result as a modpack and shader profile. The scope and phases are in [README.md](README.md). Measured facts so far are in [docs/findings.md](docs/findings.md). Read both before starting.

The owner plays, judges the look and decides trade-offs. Agents build, measure and report.

## Target machine

- Mac mini, Apple M4 Pro, **16-core GPU**, 48 GB RAM, macOS 27.
- Samsung Odyssey G80SH, 3840x2160 at 240 Hz, macOS scaling 1:1 (not HiDPI). Mods that halve the Retina backing scale, such as retiNO or Sodium Extra's "reduce resolution on mac", do nothing here.
- Minecraft 26.2, Fabric Loader 0.19.5, Sodium 0.9.2, Iris 1.11.4, Java Zulu 25.
- OpenGL 4.1 through Apple's Metal layer. Some extensions are missing, for example `GL_ARB_derivative_control`.

## Rules

- **Measure before and after every change.** A change without numbers is not done. Follow the `fps-measurement` skill.
- **One variable per measurement.** Never change a mod and a setting in the same run.
- **The owner's Modrinth instances are their game, not a test bench.** Read them freely. Before writing to one, ask, make sure the game is closed, and keep a `.bak` copy. Follow the `modrinth-instance` skill.
- **Never upload logs.** JVM crash files (`hs_err_pid*.log`) contain the Minecraft access token in plain text. Don't commit them, don't paste them anywhere public.
- **Don't add JVM flags without a measured reason.** `-XX:+UseZGC -XX:+ZGenerational` crashed the JVM on this machine (see findings). G1 is the default and stays the default.
- **Complementary's license restricts redistributing modified versions.** Ship settings profiles, never edited shader files. Check the current license before publishing anything that touches a shaderpack.
- **Report numbers honestly.** Say which runs were warm, which scenes, which build. If something wasn't measured, say so.

## Phase 0: benchmark harness (next)

Goal: one command runs the real client through a fixed set of scenes and writes results an agent can compare.

Planned shape:
- A Fabric Loom project in this repo for Minecraft 26.2.
- Uses `fabric-client-gametest-api-v1` to start the client, create or load a fixed-seed world, set time, weather and camera per scene, warm up, record frame times, take a screenshot.
- Sodium and Iris as runtime dependencies from the Modrinth Maven (`https://api.modrinth.com/maven`). The shaderpack and its settings file are copied into the run directory.
- Output in `results/<timestamp>/`: `results.json` (per scene: average fps, 1% low, frame time percentiles, settings fingerprint) and one PNG per scene.
- 8 to 10 scenes: forest at noon, village with mobs, ocean, cave with torches, night sky, rain at night, and a fast camera pan for chunk loading.

Verify these first, before building on them:
1. Fabric Loom setup for 26.2. Minecraft is unobfuscated since 26.1, so check how current Loom handles mappings.
2. Whether client gametests run with Iris and a shaderpack enabled, and whether Iris can be pointed at a pack programmatically.
3. Whether a gametest can hold a scene for a fixed wall-clock time and read real frame times, not just tick counts.
4. Run-to-run variance. Done means two consecutive runs within about 3% per scene.

The game window must be in the foreground and unobstructed during a run. macOS throttles occluded windows, which ruins the numbers.

## Writing

Docs, comments and commit messages: plain words, short sentences, sentence case headings. No em or en dashes. No filler.
