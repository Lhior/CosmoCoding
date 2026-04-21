# cobaya

For cobaya, the most common debugging mistake is to jump straight into a full sampler run when the real problem is already visible in the model setup, likelihood code, or path handling.

## Debug in Serial First

Before you try MPI or long chains:

- run with the smallest input that still fails,
- use one process,
- inspect the likelihood setup and parameter dictionary,
- confirm where outputs are written.

## Launch Template Strategy

Use the companion template in the examples repo:

- [cobaya examples directory](https://github.com/Lhior/CosmoCoding-examples/tree/main/software/cobaya)

Edit these fields:

- `program`: your environment's `cobaya-run` entry point,
- `cwd`: the directory where you want outputs and chains to land,
- `args`: the YAML input file and any resume or force flags,
- `env`: thread-count or theory-code variables when needed.

## Common Failure Modes

- Output paths are not what you think they are.
- A custom likelihood import resolves in the shell but not in the debugger.
- Parallel execution hides a simpler serial error.

## Recommended Debug Points

- Likelihood initialization.
- The first log-likelihood evaluation.
- Any file path construction used for outputs or external theory data.
