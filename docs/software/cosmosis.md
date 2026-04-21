# cosmoSIS

cosmoSIS is most manageable in the debugger when you separate three concerns: the executable, the pipeline configuration, and the environment variables that locate plugin code and shared libraries.

## Launch Template Strategy

Use the companion template in the examples repo:

- [cosmoSIS examples directory](https://github.com/Lhior/CosmoCoding-examples/tree/main/software/cosmosis)

The template includes:

- a practice script with an intentional module bug,
- a placeholder pipeline INI file,
- a debugger template with room for environment bootstrap variables.

Fields you usually need to edit:

- `program`: the `cosmosis` executable or a wrapper script,
- `cwd`: the directory that contains the pipeline config,
- `env`: any variables needed for plugin lookup, data roots, or shared libraries.

## Common Failure Modes

- Wrong plugin path or missing shared library variables.
- Section or key mismatches in the INI file.
- Debugging the full executable before validating a custom module in isolation.

## Recommended Debug Points

- Module initialization.
- The first point where parameters are read from the data block.
- The place where your module writes derived quantities back to the block.
