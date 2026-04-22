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

### LSST Pipeline Environments

For LSST pipeline users, a common friction point is that activating the conda environment is not always enough for VS Code sessions. If the environment still expects you to run `setup lsst_distrib` manually after activation, the debugger and integrated terminal can end up missing the LSST stack state you thought you had.

One practical fix is to add an activation hook under `${CONDA_PREFIX}/etc/conda/activate.d/`. The filename does not matter. For example, you could create `lsst_activate.sh` with:

```bash
# Activate LSST pipeline
if [[ ! ${EUPS_PATH} ]]; then
  source ${CONDA_PREFIX}/etc/conda/activate.d/rubin-env-nosysroot_activate.sh
fi

# call eups setup to get all galsim stuff in the path
{
  setup lsst_distrib >/dev/null 2>&1
} || {
  echo "DM stack could not be activated!"
}
```

After that, activating the environment should also perform the `lsst_distrib` setup step, which makes VS Code much easier to use because you no longer need to remember a separate manual `setup lsst_distrib` before running or debugging.

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

## Work on HPC Clusters

Remote-SSH is the entry point, but HPC work adds three constraints that matter for debugging:

- the login node is usually for editing, lightweight builds, and setup rather than real computation,
- interactive compute allocations expire and can drop your editor session,
- cluster-specific authentication and file-system rules often matter as much as your code.

General advice:

- Keep your SSH config clean and explicit.
- Start with small test data on the cluster before launching large runs.
- If the real job needs a scheduler allocation, debug the same code path first on a login node or interactive node with a tiny input.
- Keep environment activation steps reproducible. If you need modules plus conda, write them down once and reuse them.

On SLURM-based HPC clusters, a short partial-node interactive allocation often looks like this:

```bash
salloc -p HENON -N 1 --ntasks=1 --cpus-per-task=8 --mem=32G -t 00:15:00
```

Treat that as a template rather than a universal command. The partition name, memory policy, wall time, and CPU layout vary by cluster, but the general idea is the same: request a small interactive allocation that is large enough to reproduce the bug and small enough to get quickly.

### Perlmutter with VS Code

NERSC's VS Code guidance is close to the standard Remote-SSH workflow, but there are two important differences:

- VS Code will connect to a Perlmutter login node by default, which is fine for development and light setup but not for real computation.
- If your Global HOME usage is over quota, VS Code remote connections can fail before you even reach the point of debugging.

For login-node access, the normal Remote-SSH flow is enough once your SSH keys are set up. For compute-node access, NERSC recommends using `sshproxy` keys and an SSH config that can hop through the Perlmutter login host.

An example Unix-like `~/.ssh/config` for Perlmutter-style access is:

```sshconfig
Host dtn*.nersc.gov perlmutter*.nersc.gov *.nersc.gov
  LogLevel QUIET
  IdentityFile ~/.ssh/nersc
  IdentitiesOnly yes
  ForwardAgent yes
  # User your_nersc_username

Host nid??????
  LogLevel QUIET
  IdentityFile ~/.ssh/nersc
  StrictHostKeyChecking no
  ProxyJump perlmutter.nersc.gov
  Hostname %h
  # User your_nersc_username
```

On Windows, the same idea applies but the key path needs to point to your Windows OpenSSH directory:

```sshconfig
Host dtn*.nersc.gov perlmutter*.nersc.gov *.nersc.gov
  LogLevel QUIET
  IdentityFile C:\Users\your_windows_user\.ssh\nersc
  IdentitiesOnly yes
  ForwardAgent yes
  # User your_nersc_username

Host nid??????
  LogLevel QUIET
  IdentityFile C:\Users\your_windows_user\.ssh\nersc
  StrictHostKeyChecking no
  ProxyJump perlmutter.nersc.gov
  Hostname %h
  # User your_nersc_username
```

NERSC also recommends two VS Code settings for compute-node sessions:

```json
{
  "remote.SSH.maxReconnectionAttempts": 2,
  "remote.SSH.useFlock": false
}
```

The reconnection limit helps VS Code fail fast once an interactive allocation ends. The `useFlock` setting matters because Perlmutter compute nodes do not support file locking in the way VS Code expects for the mounted home directory.

### Perlmutter Compute-Node Workflow

The usual workflow is:

1. Connect to `perlmutter.nersc.gov` with Remote-SSH.
2. Request an interactive allocation from the login node.
3. Note the allocated `nid...` hostname.
4. Start a second Remote-SSH session to that `nid...` hostname.

NERSC's documentation shows a GPU example like this:

```bash
salloc --nodes 1 --qos interactive --time 00:60:00 -C gpu -A m0000
```

Once the allocation is granted, connect VS Code to the `nid...` host that Slurm prints. For CPU-only debugging, follow the same pattern but use the CPU partition, constraints, account, and memory settings appropriate for your project and system policy.

### Perlmutter `sshproxy` and 24-Hour Keys

NERSC's `sshproxy` tool is the standard way to turn MFA into a short-lived SSH certificate. By default it generates keys under `~/.ssh` that are valid for 24 hours.

On Linux, macOS, or WSL2, the core command is:

```bash
sshproxy -u your_nersc_username
```

If you authenticate through Federated ID instead of a local NERSC password, use:

```bash
sshproxy -f
```

`sshproxy` writes three files by default:

- `~/.ssh/nersc`
- `~/.ssh/nersc.pub`
- `~/.ssh/nersc-cert.pub`

You can inspect the expiration time with:

```bash
ssh-keygen -L -f ~/.ssh/nersc-cert.pub | grep Valid
```

If Remote-SSH suddenly starts asking for `Password + OTP` again, the first thing to suspect is an expired `sshproxy` certificate.

### Windows and WSL2 Workaround

One practical Windows workflow is to run `sshproxy` inside WSL2 and then copy the resulting NERSC key files into the Windows `.ssh` directory that VS Code on Windows actually uses. This is useful when the WSL path is the easiest place to run the Unix-like `sshproxy` client, but Remote-SSH is running in the Windows VS Code process.

Your alias-based workaround is a concrete example of that pattern:

```bash
alias nersc='~/sshproxy.sh -u lhior; cp -r ~/.ssh/nersc* /mnt/c/Users/LEONE/.ssh/'
```

The important idea is not the exact username or path. It is the sequence:

1. generate or refresh the 24-hour NERSC key inside WSL2,
2. copy `nersc`, `nersc.pub`, and `nersc-cert.pub` into `C:\Users\...\.ssh`,
3. let Windows VS Code Remote-SSH use those files.

If you use this workflow, adjust both the NERSC username and the Windows home path, and remember that every refreshed key in WSL2 must be copied over again before Windows VS Code will see it.

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
