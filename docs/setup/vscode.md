# Set Up VS Code

The goal is not just to install VS Code. The goal is to make it the place where you edit, run, debug, and inspect your cosmology workflows without losing track of which environment, host, or configuration you are actually using.

## Install the Core Extensions

For most cosmology projects, start with these extensions:

- Python
- Jupyter
- Remote - SSH
- GitHub Copilot
- GitHub Copilot Chat

If you regularly open notebooks, also confirm that the Jupyter extension is using the same Python environment you use on the command line.

## Pick One Interpreter Per Project

The debugger only behaves predictably when the interpreter in VS Code matches the interpreter you used to install the software.

1. Create or activate the conda environment you actually use for the project.
2. In VS Code, run `Python: Select Interpreter`.
3. Choose the interpreter inside that environment.
4. If you use notebooks, select the same kernel in the notebook toolbar.

When a tool exposes a console entry point such as `rail-project`, `cobaya-run`, or `cosmosis`, that command must come from the same environment.

## Connect to a Cluster with Remote-SSH

Use SSH keys first. Then teach VS Code how to connect.

An example `~/.ssh/config` entry:

```sshconfig
Host my-cluster
    HostName login.example.edu
    User your_username
    IdentityFile ~/.ssh/id_ed25519
    ServerAliveInterval 60
    ServerAliveCountMax 10
```

Once that works from the terminal:

1. Open VS Code.
2. Run `Remote-SSH: Connect to Host`.
3. Choose the `Host` entry from your SSH config.
4. Open the remote folder you actually work in.

Practical advice:

- Keep your SSH config clean and explicit.
- Start with small test data on the cluster before launching large runs.
- If the real job needs a scheduler allocation, debug the same code path first on a login node or interactive node with a tiny input.
- Keep environment activation steps reproducible. If you need modules plus conda, write them down once and reuse them.

## Set Environment Variables Intentionally

Many cosmology tools depend on environment variables such as data roots, plugin directories, or config directories. Put those in `launch.json` so the debugger run matches your shell run.

```json
{
  "env": {
    "RAIL_PROJECT_CONFIG_DIR": "${workspaceFolder}/rail_project_config",
    "OMP_NUM_THREADS": "1"
  }
}
```

Use `OMP_NUM_THREADS=1` for interactive debugging unless you explicitly need to inspect threaded behavior.

## Use Copilot as a Working Assistant, Not as Blind Autocomplete

For agentic work in VS Code:

1. Sign in to GitHub inside VS Code.
2. Install GitHub Copilot and GitHub Copilot Chat.
3. Open the repository root, not just an isolated file.
4. Let Copilot inspect the relevant config, scripts, and docs before asking it to modify launch settings or pipeline code.

Good usage patterns:

- Ask for help locating the controlling entry point before editing.
- Ask for a debugger template for a specific tool and environment.
- Ask for a focused validation step after a config change.
- Review diffs and terminal commands before you accept them.

Bad usage patterns:

- Asking for broad rewrites without giving the tool the relevant files.
- Letting it invent paths, environment variables, or command names you have not verified.
- Treating generated launch configurations as correct without testing them.

## Nice Defaults for Research Projects

These settings are worth considering in project workspaces:

```json
{
  "python.terminal.activateEnvironment": true,
  "jupyter.askForKernelRestart": false,
  "files.trimTrailingWhitespace": true,
  "editor.formatOnSave": true
}
```

The exact formatting tools depend on the repo, but the principle is stable: keep the editor predictable so the debugger is the only moving part when you are diagnosing failures.

## Next Step

Move on to [Use the debugger](../debugger/index.md) before you try to debug any large pipeline. Most wasted time comes from unclear breakpoints and mismatched environments, not from the debugger itself.
