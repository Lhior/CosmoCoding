# Work on Perlmutter

This page pulls the HPC material out of the VS Code setup guide and focuses it on NERSC's Perlmutter system. The closest upstream references are NERSC's [Visual Studio Code / VSCode](https://docs.nersc.gov/connect/vscode/), [job examples](https://docs.nersc.gov/jobs/examples/), and [monitoring](https://docs.nersc.gov/jobs/monitoring/) pages.

## Use VS Code on Perlmutter

Remote-SSH is still the entry point, but Perlmutter adds three constraints that matter for debugging:

- the login node is for editing, lightweight builds, and setup rather than real computation,
- compute-node sessions come from Slurm allocations and can disappear when the allocation ends,
- NERSC-specific authentication, quotas, and filesystem behavior can matter as much as your code.

NERSC's VS Code guidance is close to the standard Remote-SSH workflow, but there are two practical differences worth remembering:

- VS Code will connect to a Perlmutter login node by default, which is fine for development and light setup but not for real computation.
- If your Global HOME usage is over quota, Remote-SSH can fail before you even reach the point of debugging.

### SSH Config for Login and Compute Nodes

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

### VS Code Settings for Compute-Node Sessions

NERSC also recommends a small set of VS Code user settings for compute-node sessions. Put these in your User Preferences JSON rather than in a workspace file so they apply to your Remote-SSH sessions in general.

To open that file:

- on Windows or Linux, open the Command Palette with `Ctrl+Shift+P`,
- on macOS, open the Command Palette with `Cmd+Shift+P`,
- then run `Preferences: Open User Settings (JSON)`.

For Perlmutter-style remote work, a practical settings block is:

```json
{
  "remote.SSH.maxReconnectionAttempts": 2,
  "remote.SSH.useFlock": false,
  "remote.extensionKind": {
    "*": ["ui"],
    "github.copilot": ["ui"],
    "github.copilot-chat": ["ui"]
  },
  "remote.SSH.enableWorkspaceFolderWatcher": false,
  "remote.SSH.defaultExtensions": [],
  "search.followSymlinks": false
}
```

The reconnection limit helps VS Code fail fast once an interactive allocation ends. The `useFlock` setting matters because Perlmutter compute nodes do not support file locking in the way VS Code expects for the mounted home directory.

The `remote.extensionKind` block is the important one if you want to reduce memory pressure on the cluster. Setting an extension to `"ui"` tells VS Code to run that extension in the local UI-side extension host instead of the remote extension host on the login node or compute node. In practice, that shifts the extension's memory and CPU cost onto your laptop or desktop, which is usually the right trade when the remote side is a shared HPC environment. For tools like GitHub Copilot and Copilot Chat, `"ui"` mode is a good fit because they integrate with the editor UI and do not need to execute most of their work on the cluster.

The remaining settings also reduce avoidable remote overhead:

- `remote.SSH.enableWorkspaceFolderWatcher: false` avoids running the remote workspace file watcher, which can be noisy and expensive on large or network-mounted filesystems.
- `remote.SSH.defaultExtensions: []` stops VS Code from automatically installing your usual local extensions into the remote host unless you explicitly choose to do that.
- `search.followSymlinks: false` keeps search from walking through symlink-heavy trees, which is often the safer default on shared filesystems with large software stacks or mirrored directories.

These settings do not eliminate the need for a real Slurm allocation when you need compute resources, but they do help keep editor convenience features from consuming memory and filesystem activity on Perlmutter unnecessarily.

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

For CPU-only debugging, follow the same pattern but use `-C cpu` and the smallest wall time and resource request that actually reproduces the problem. Interactive jobs are still real compute jobs, so keep them short and intentional.

### Windows and WSL2 Workaround

One practical Windows workflow is to run `sshproxy` inside WSL2 and then copy the resulting NERSC key files into the Windows `.ssh` directory that VS Code on Windows actually uses. This is useful when the WSL path is the easiest place to run the Unix-like `sshproxy` client, but Remote-SSH is running in the Windows VS Code process.

A generic alias for that pattern looks like this:

```bash
alias nersc='~/sshproxy.sh -u your_nersc_username; cp -r ~/.ssh/nersc* /mnt/c/Users/your_windows_user/.ssh/'
```

The important idea is not the exact username or path. It is the sequence:

1. generate or refresh the 24-hour NERSC key inside WSL2,
2. copy `nersc`, `nersc.pub`, and `nersc-cert.pub` into `C:\Users\...\.ssh`,
3. let Windows VS Code Remote-SSH use those files.

If you use this workflow, adjust both the NERSC username and the Windows home path, and remember that every refreshed key in WSL2 must be copied over again before Windows VS Code will see it.

## Request the Right Size Slurm Job

Perlmutter gives you several ways to get onto compute nodes. The point is not just to get a job running. The point is to request resources that match the work.

- Use the interactive QOS when you need a short live debugging session on a compute node.
- Use the shared QOS when the workload only needs part of a CPU node.
- Use a full node only when the code can actually use it or when node exclusivity is required for performance, memory, or communication reasons.

On Perlmutter CPU nodes, Slurm counts hyperthreads as `cpu`s. That means the `--cpus-per-task` number is larger than the physical-core count you may have in mind, and it is one reason the NERSC examples often look doubled relative to physical cores.

### Partial-Node CPU Jobs

NERSC's shared-QOS examples are the model to follow when you do not need an entire Perlmutter CPU node. A simple single-process batch job might look like this:

```bash
#!/bin/bash
#SBATCH -A m0000
#SBATCH --qos=shared
#SBATCH --constraint=cpu
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=16
#SBATCH --mem=64G
#SBATCH --time=00:30:00

export OMP_NUM_THREADS=8
srun --cpu-bind=cores python analyze_tile.py
```

That example reserves a slice of a node instead of the whole thing. That is the right habit when the code only needs a modest number of cores and a bounded amount of memory.

This is also where utilization discipline matters. A full Perlmutter CPU node has a lot of compute. If you request the whole node and only keep a small fraction of it busy, you are wasting allocation time and making the scheduler's job harder for everyone else. If your workflow only uses a fraction of the node, request a fraction of the node. If you really do need the whole node, make sure the code is configured to use most of what you reserved.

### Check CPU Utilization on the Compute Node

Do not guess from the login node. First check where the job is actually running:

```bash
squeue -j 123456 -o "%.18i %.8T %.20N"
```

If you are not sure of the job ID yet, start with:

```bash
squeue -u $USER
```

Once you have the `nid...` host, SSH to that specific compute node and inspect it directly:

```bash
ssh nid001234
htop -u $USER
```

This is the fastest way to see whether the job is really using the CPUs you asked for. If `htop` shows only a few busy cores while most of the reserved node sits idle, the job shape is wrong. Either request fewer resources, move to `--qos=shared`, or increase the concurrency so the compute node is being used well. Requesting a full node and then exercising only a small fraction of it is wasteful on Perlmutter.

## Stack Slurm Jobs with Dependencies

Sometimes the cleanest workflow is not one giant batch script but a chain of separate jobs where each stage starts only after the previous one is truly finished. On Perlmutter, the standard Slurm mechanism for that is a dependency.

For a simple two-stage pipeline:

```bash
first_job=$(sbatch --parsable first_job.slurm)
sbatch --dependency=afterok:$first_job second_job.slurm
```

`afterok` means the second job will wait until the first exits successfully. That is usually what you want for real pipelines because it prevents the downstream stage from starting on broken or incomplete output.

For a longer stacked chain:

```bash
job1=$(sbatch --parsable prep.slurm)
job2=$(sbatch --parsable --dependency=afterok:$job1 fit.slurm)
sbatch --dependency=afterok:$job2 summarize.slurm
```

If you want the next job to start once the previous one finishes regardless of success or failure, use `afterany` instead of `afterok`. NERSC also documents sequential `srun` calls inside a single batch allocation, but separate dependent jobs are usually easier to monitor, rerun, and reason about when the stages have different runtimes or resource needs.