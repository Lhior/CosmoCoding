# Use the VS Code Debugger Properly

The debugger is not just a prettier way to run a script. It is the fastest way to inspect the exact program state that produced a failure.

## What the Debugger Gives You

- Breakpoints that stop execution before or at the failure.
- A variable view for the current stack frame.
- A watch list for expressions you care about.
- A call stack that shows how execution reached the current line.
- Exception breakpoints that stop when an error is raised, even if you forgot to set a line breakpoint.

If your workflow still relies on adding and removing print statements every time a pipeline changes shape, you are paying an avoidable tax.

## The Controls That Matter

- Continue: resume until the next breakpoint.
- Step over: run the current line without descending into called functions.
- Step into: move into the called function.
- Step out: finish the current function and return to its caller.
- Restart: rerun with the same configuration.
- Stop: end the debug session.

Also use these deliberately:

- Conditional breakpoints when you only care about one row, one stage, or one parameter combination.
- Logpoints when you want structured logging without editing the source.
- Exception breakpoints when you want to stop at the original error instead of at the final crash site.

## Anatomy of `launch.json`

The following pattern is adapted from an existing RAIL launcher in this workspace:

```json
{
  "name": "RAIL: build train_fair_16k",
  "type": "debugpy",
  "request": "launch",
  "program": "/path/to/conda/env/bin/rail-project",
  "cwd": "${workspaceFolder}",
  "env": {
    "RAIL_PROJECT_CONFIG_DIR": "${workspaceFolder}/rail_project_config"
  },
  "args": [
    "build",
    "--flavor",
    "train_fair_16k",
    "--force",
    "${workspaceFolder}/rail_project_config/roman/roman_3band_rubin_brett.yaml"
  ],
  "console": "integratedTerminal",
  "justMyCode": false
}
```

The important fields are:

- `program`: the actual script or executable you want VS Code to run.
- `cwd`: the working directory used to resolve relative paths.
- `env`: environment variables required by the code.
- `args`: command-line arguments exactly as you would pass them in the shell.
- `console`: usually `integratedTerminal` for CLI tools.
- `justMyCode`: set to `false` when you need to step into framework code, not just your wrapper.

## A Practical Debugging Workflow

1. Start with the smallest input that still reproduces the bug.
2. Put a breakpoint at the boundary where inputs are parsed, loaded, or transformed.
3. Turn on exception breakpoints.
4. Inspect the first wrong state, not just the last crash message.
5. Only after you understand the state mismatch should you edit the code.

For pipelines, this usually means debugging one stage or one command at a time instead of the whole workflow.

## Common Failure Modes in Scientific Workflows

- The wrong interpreter is selected, so the command entry point or import path is wrong.
- The working directory is wrong, so relative config paths silently point at the wrong files.
- A required environment variable is missing.
- A notebook kernel and the shell interpreter are not the same environment.
- Parallel or MPI execution hides the simple serial failure you should have debugged first.

## When To Use `justMyCode: false`

Turn it off when:

- you need to step through library or framework code,
- the bug appears inside a framework callback,
- a wrapper script is too thin to explain the failure.

Leave it on when you are only checking your own logic and the framework internals are noise.

## Remote and Cluster Work

You do not need to debug on the largest possible run. Debugging works best when you first reproduce the same control flow on a tiny input or on an interactive allocation.

Good pattern:

- make the command work at the terminal,
- copy that command into `launch.json`,
- add the same environment variables,
- reduce the data size,
- then attach breakpoints.

## Next Step

Choose the relevant software guide and copy the matching `debugging.json` template from the companion examples repository.
