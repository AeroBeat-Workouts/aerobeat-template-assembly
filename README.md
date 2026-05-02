# AeroBeat Assembly Template

This repo is the official template for creating an **Assembly** repository within the AeroBeat ecosystem.

An **Assembly** is the top-level Godot project that composes the concrete AeroBeat addons needed to produce a runnable game client.

## Current product truth

Use this template against the locked AeroBeat v1 scope:

- **official v1 gameplay:** Boxing and Flow
- **official v1 gameplay input:** camera only
- **primary release target:** PC community first
- **non-camera gameplay inputs:** future work, not current parity promises
- **mouse/touch:** valid for UI navigation, not gameplay parity claims

This template stays intentionally bounded to the assembly contract itself. It should teach assemblies to compose the concrete lane/core addons they actually ship instead of implying one universal shared gameplay core.

## Repository details

- **Type:** Assembly (Game Client)
- **License:** **GNU GPLv3** (Strict Copyleft)
- **Primary dependency contract:** `addons.jsonc` at repo root
- **Current baseline dependencies:**
  - `aerobeat-input-core` pinned at `v0.1.2`
  - `gut` for repo-local validation
- **Common assembly additions as needed:**
  - `aerobeat-input-*` gameplay/input addons such as the current camera-first MediaPipe path
  - `aerobeat-ui-core`, `aerobeat-ui-shell-*`, and `aerobeat-ui-kit-*`
  - `aerobeat-feature-*`
  - `aerobeat-asset-*`
  - `aerobeat-tool-*`

## Assembly dependency truth

The committed template baseline now starts from `aerobeat-input-core` rather than the old transition-era `aerobeat-core` package key.

That is a conscious lane-specific choice:

- the current active AeroBeat v1 assembly architecture is camera-first
- the live community assembly mounts its shared gameplay-input foundation at `res://addons/aerobeat-input-core/`
- downstream assemblies should still add, replace, or extend dependencies based on the concrete runtime they actually ship

This template does **not** claim that every assembly must use the exact same addon set as `aerobeat-assembly-community`. It only stops teaching the stale universal-core story.

## GodotEnv assembly flow

Assembly repos use a **root** `addons.jsonc` manifest instead of the package-repo `.testbed/addons.jsonc` convention.

- Canonical runtime/dev manifest: `addons.jsonc`
- Installed addons: `addons/`
- GodotEnv cache: `.addons/`
- Repo-local tests: `test/`

The repo root is the actual product project and the canonical place to restore dependencies, open the editor, and run validation.

### Restore dependencies

From the repo root:

```bash
godotenv addons install
```

That restores the template's current baseline assembly manifest into `addons/`.

### Open the assembly

From the repo root:

```bash
godot --editor --path .
```

### Import smoke check

From the repo root:

```bash
godot --headless --path . --import
```

### Run unit tests

From the repo root:

```bash
godot --headless --path . --script addons/gut/gut_cmdln.gd \
  -gdir=res://test \
  -ginclude_subdirs \
  -gexit
```

## Validation notes

- `addons.jsonc` is the committed assembly dependency contract.
- the template baseline now mounts its shared gameplay-input foundation at `res://addons/aerobeat-input-core/`
- `aerobeat-ui-core` is no longer presented as a universal default; add it only when an assembly actually consumes it
- `addons/` is a generated install target and must not be committed
- downstream assemblies should replace or extend the baseline manifest entries with the concrete input/UI/feature/asset contracts they actually ship

## Structure

- `addons.jsonc` - Root GodotEnv assembly manifest
- `project.godot` - Runnable product project
- `test/` - Repo-local GUT tests for assembly-specific behavior
- `addons/` - Generated dependency install target (ignored)
