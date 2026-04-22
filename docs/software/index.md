# Cosmology Code Examples

This section collects worked debugger guidance for common cosmology software. The goal is not to document every feature of each codebase. The goal is to show how to get from a real command or script to a reproducible VS Code debugging workflow.

Each guide focuses on the same practical questions:

- what command or entry point you actually debug,
- which environment needs to be active,
- which environment variables matter,
- how to reduce the problem to a small reproducible case,
- where the matching `debugging.json` template lives in the companion examples repository.

## Available Guides

- [RAIL](rail.md) for CLI-driven photometric redshift pipelines and config-heavy runs.
- [TXPipe](txpipe.md) for stage-level debugging before scaling up to the full pipeline.
- [cobaya](cobaya.md) for serial-first debugging of samplers, likelihoods, and path setup.
- [cosmoSIS](cosmosis.md) for environment-aware debugging of modules and pipeline configuration.
- [TreeCorr](treecorr.md) for smaller correlation workflows and catalog/config validation.

## How To Use This Section

1. Start from the code you are actually running today, not from an abstract example.
2. Pick the closest software page.
3. Copy the matching `debugging.json` template from the companion examples repository.
4. Replace paths, environment names, and arguments with the ones that already work in your shell.
5. Reproduce the bug on the smallest dataset or shortest pipeline slice you can manage.

If you are new to VS Code itself, read [Set Up VS Code](../setup/vscode.md) and [Use the Debugger](../debugger/index.md) first. Those pages explain the environment and debugger concepts that the code-specific pages assume.
