# TemplateGame

TemplateGame is the reference external game project for the Foundation engine.

## Layout

```text
template-game/
  engine/   # Foundation engine submodule, checkout, junction, or symlink
  game/     # TemplateGame Cargo package, source, manifest, and game assets
  scripts/  # Game-facing build, run, package, and validation commands
```

Game-owned assets live under `game/assets`. Foundation-owned assets may live under `engine/assets` and are packaged separately under `assets/engine` when present.

## Clone

Clone with submodules to get the default Foundation engine association:

```cmd
git clone --recurse-submodules https://github.com/JonLangfordUK/template-game.git
```

If the repository was cloned without submodules, initialize the engine later:

```cmd
git submodule update --init --recursive
```

## Alternate engine path

By default, scripts use `engine/`. To use another Foundation checkout, set `FOUNDATION_ENGINE_PATH` before running scripts:

```cmd
set FOUNDATION_ENGINE_PATH=E:\GameDev\Foundation
scripts\build.cmd --configuration test --target game
```

The alternate engine path must contain `scripts\foundation-build.cmd`.

## Commands

Build the game:

```cmd
scripts\build.cmd --platform windows-x64 --configuration test --target game
```

Run the game:

```cmd
scripts\run.cmd --platform windows-x64 --configuration test --target game
```

Runtime arguments go after `--`:

```cmd
scripts\run.cmd --platform windows-x64 --configuration test --target game -- --log
```

### Common Script Arguments

| Argument | Description | Example |
| --- | --- | --- |
| `--platform <platform>` | Selects the build platform. Current common value is `windows-x64`. | `--platform windows-x64` |
| `--configuration <configuration>` | Selects the build configuration. Use `debug` for local debugging, `test` for optimized internal testing, or `shipping` for public/package builds. | `--configuration test` |
| `--target <target>` | Selects the output target. Use `game` for the standalone game or `game-editor` for editor-enabled non-shipping builds. | `--target game` |
| `--` | Separates build/script options from runtime arguments forwarded to the game executable. | `scripts\run.cmd --configuration test -- --log` |

### Runtime Arguments

| Argument | Description | Example |
| --- | --- | --- |
| `--editor` | Enables Foundation editor-time mode when the game is built with editor support. `game-editor` runs pass this automatically. Not available for shipping builds. | `scripts\run.cmd --target game-editor` |
| `--log` | Requests visible log output in non-shipping builds. On Windows this opens a separate Foundation log window by default. | `scripts\run.cmd -- --log` |
| `--log-inline` | Requests visible log output in the current terminal instead of a separate log window. | `scripts\run.cmd -- --log-inline` |
| `--scene <scene>` | Non-shipping startup override. Opens one registered scene key or direct `.bsn` asset path instead of the default splash flow. | `scripts\run.cmd -- --scene main_menu` |
| `--scene [<scene>, <scene>]` | Non-shipping startup stack override. Opens scenes in order from bottom to top. Spaces around commas are allowed; quote the value when using spaces. | `scripts\run.cmd -- --scene "[gameplay_level, scenes/main_menu.bsn]"` |

Package the game:

```cmd
scripts\package.cmd --platform windows-x64 --configuration shipping --target game
```

Validate the game crate:

```cmd
scripts\validate.cmd
```

## Branch workflow

This repository uses long-lived `dev` and `main` branches.

- Work happens on `feature/*` or `hotfix/*` branches.
- Pull requests into `dev` are normal feature integration.
- Pull requests into `main` should come only from `dev` or `hotfix/*`.
- GitHub branch protection should require the TemplateGame Build checks before merging.
