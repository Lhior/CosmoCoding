# TreeCorr

TreeCorr workflows are often simpler than full pipeline frameworks, which makes them a good place to build debugger habits. The main trick is to shrink the catalog size until you can inspect the whole path from config to correlation output.

## Launch Template Strategy

Use the companion template in the examples repo:

- [TreeCorr examples directory](https://github.com/Lhior/CosmoCoding-examples/tree/main/software/treecorr)

The companion directory includes a small buggy script for the Python API path and a placeholder config for a CLI-style run.

Fields you usually need to edit:

- the interpreter that has TreeCorr installed,
- the config file path,
- any catalog paths,
- optional thread settings for interactive work.

## Common Failure Modes

- Wrong column names or units.
- Config values that point to the wrong catalog or patch file.
- Trying to reason about performance before confirming the correlation setup is correct.

## Recommended Debug Points

- Catalog construction.
- The first call that defines the correlation object.
- The output-writing step where filenames and summary values are finalized.
