# Repository Guidelines

## Project Structure & Module Organization
This repository is a ZMK user config for ErgoHaven boards.
- `config/`: all firmware-facing configuration.
- `config/*.keymap`: key behavior and layer definitions per keyboard/shield variant.
- `config/*.conf`: per-keyboard Kconfig overrides (power, combos, features).
- `config/*.json`: physical layout metadata used by tooling/visualization.
- `config/west.yml`: west manifest for pulling `ergohaven-zmk`.
- `build.yaml`: CI build matrix (board/shield/keymap/artifact combinations).
- `.github/workflows/build.yml`: triggers the reusable upstream build workflow.

## Build, Test, and Development Commands
Use west with the local `config` manifest.
```powershell
west init -l config
west update
```
Build a target locally (example: Velvet left):
```powershell
west build -s zmk/app -d build/velvet_v3_left -b ergohaven -- -DSHIELD=velvet_v3_left -DZMK_CONFIG="%CD%/config"
```
Build a named keymap variant (example: Trackball v3.1):
```powershell
west build -s zmk/app -d build/trackball_v3_1 -b ergohaven -- -DSHIELD=trackball -DKEYMAP=trackball_v3.1 -DZMK_CONFIG="%CD%/config"
```
CI also builds on every `push` and `pull_request` using `build.yaml`.

## Coding Style & Naming Conventions
- Follow existing DTS/Kconfig style: 4-space indentation, braces on separate lines, aligned keymap columns where practical.
- Keep names consistent with current patterns: base variants like `velvet_v3.keymap`, `op36.conf`, and `k03.json`.
- Use language variant names as `*_ruen.keymap`.
- Use Qube variant config names as `*_qube.conf`.
- Prefer small, focused edits per keyboard family.

## Testing Guidelines
- There is no unit-test suite in this repo; successful firmware builds are the test gate.
- For each change, build at least one directly affected target locally.
- If you modify shared behavior (e.g., includes, common macros, or `build.yaml`), validate multiple affected shields before opening a PR.

## Commit & Pull Request Guidelines
- Keep commit messages short, imperative, and lowercase, matching history (e.g., `add trackball v3.1`, `fix ruen mo`).
- PRs should state what keyboard/shield(s) are affected.
- PRs should describe behavior changes (layers, combos, media keys, etc.).
- PRs should call out any `build.yaml` artifact-name updates.
- PRs should confirm local build(s) and CI passed.
