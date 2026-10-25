# Silica

A fast, good-looking Minecraft modpack and shader profile for Apple Silicon Macs. The goal is modded Minecraft Java with shaders at 4K, without giving up the look.

The method is simple: measure first, then change one thing at a time. Most "optimization" advice for Minecraft is guesswork. This project replaces it with a repeatable benchmark that agents can run on their own.

## Why

On a Mac mini M4 Pro driving a 4K 240 Hz monitor:

| Setup | 4K fps |
|---|---|
| Lean modpack (27 mods), no shaders | 160 |
| Full modpack (about 90 mods), no shaders | 80 |
| Full modpack with Complementary Unbound | about 25 to 60, not yet measured cleanly |

Half the frame rate goes to mods before a shader is even loaded. The rest is real hardware cost plus Apple's OpenGL layer, which runs on top of Metal and is stuck at OpenGL 4.1. Details are in [docs/findings.md](docs/findings.md).

## Plan

**Phase 0: benchmark harness.** A Fabric dev project that drives the real client through the Fabric client gametest API. It loads a fixed-seed world, plays through a set of scenes and writes frame times and screenshots per scene. Done when two runs in a row agree within about 3%.

**Phase 1: Mac modpack.** Start from the lean pack and add features one at a time, measuring each. The result is a Modrinth pack plus a table of what every mod costs.

**Phase 2: 4K Mac profile for Complementary Unbound.** A measured settings profile that keeps the Reimagined-style look with Unbound's water and night sky, and hits 60 fps at 4K in every scene.

**Out of scope:** a custom client or loader, a custom Vulkan or Metal renderer, forking or redistributing existing shaderpacks, Windows and Linux testing.

## Status

Phase 0 has not started. See [AGENTS.md](AGENTS.md) for the next steps.

## Working on this repo

This repo is set up to be worked on by coding agents. [AGENTS.md](AGENTS.md) has the rules and the current plan. Task-specific instructions live in `.agents/skills/`.
