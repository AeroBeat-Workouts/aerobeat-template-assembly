# AeroBeat Assembly Template

This is the official template for creating an **Assembly** repository within the AeroBeat ecosystem.

An **Assembly** is the top-level Godot project that composes released AeroBeat addons into a runnable game client.

## 📋 Repository Details

*   **Type:** Assembly (Game Client)
*   **License:** **GNU GPLv3** (Strict Copyleft)
*   **Primary dependency contract:** `addons.jsonc` at repo root
*   **Typical dependencies:**
    *   Lane-specific core repos as needed by the assembly (for example `aerobeat-input-core`, `aerobeat-ui-core`, `aerobeat-feature-core`, or others actually consumed)
    *   `aerobeat-ui-core` (Common baseline for UI-driven assemblies)
    *   `aerobeat-tool-*` (As needed)
    *   `aerobeat-input-*` (As needed)
    *   `aerobeat-ui-shell-*` (As needed)
    *   `aerobeat-ui-kit-*` (As needed / often transitive)
    *   `aerobeat-feature-*` (As needed)
    *   `aerobeat-asset-*` (As needed)

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

That restores the template's current baseline assembly manifest into `addons/`. Canonically, assemblies should describe themselves as composing only the lane/core repos and concrete addons they actually need, not a universal `aerobeat-core` hub.

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
- The current template baseline still pins the transition-era `aerobeat-core` package key alongside `aerobeat-ui-core` and GUT. Treat that as bootstrap-state drift rather than the canonical lane model.
- Assembly repos should explicitly call out the concrete lane/core repos they compose instead of implying that every assembly depends on one universal shared core.
- `addons/` is a generated install target and must not be committed.
- Downstream assemblies should replace or extend the baseline manifest entries with the concrete input/UI/feature/asset contracts they actually ship.

## 📂 Structure

*   `addons.jsonc` - Root GodotEnv assembly manifest.
*   `project.godot` - The runnable product project.
*   `test/` - Repo-local GUT tests for assembly-specific behavior.
*   `addons/` - Generated dependency install target (ignored).
