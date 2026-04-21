# RAIL

RAIL is usually easiest to debug from the exact `rail-project` command you already run in the terminal. The main job is to make the debugger launch the same command with the same config directory and working directory.

## What to Debug First

- One flavor instead of every flavor.
- One stage instead of the whole chain.
- A tiny config or small sample before any full build.

If the bug shows up inside framework code, keep `justMyCode: false` so you can step through RAIL internals.

## Launch Template Strategy

Use the companion template in the examples repo:

- [RAIL examples directory](https://github.com/Lhior/CosmoCoding-examples/tree/main/software/rail)

Edit these fields first:

- `program`: point to your environment's `rail-project` entry point.
- `cwd`: usually the repository root.
- `env.RAIL_PROJECT_CONFIG_DIR`: point to the config directory that holds your YAML includes.
- `args`: copy the exact command that already works in the shell.

## Why `RAIL_PROJECT_CONFIG_DIR` Matters

The existing launcher in this workspace exports `RAIL_PROJECT_CONFIG_DIR` before running `rail-project build`. That matters because many RAIL configs rely on includes and relative references rooted in that directory. If the variable is wrong, the failure often looks like a missing file or a confusing config parse error.

## Recommended Breakpoints

- The first function that reads the config.
- The first stage that mutates the dataset you care about.
- Any wrapper code that constructs the command-line arguments dynamically.

## Common Failure Modes

- Wrong conda environment, so `rail-project` resolves to a different install.
- Wrong `cwd`, so relative YAML paths break.
- Wrong `RAIL_PROJECT_CONFIG_DIR`, so config includes cannot be resolved.
- Debugging too much at once, so you do not know which stage introduced the bad state.

## Companion Files

The companion repo includes:

- a `debugging.json` template,
- a `rail_project.yaml` placeholder,
- a small `buggy_rail.py` script you can use to practice stepping through a pipeline-style failure.
