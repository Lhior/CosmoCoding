# TXPipe

TXPipe jobs are often orchestrated through pipeline tooling rather than a single short script. That is exactly why you should debug one stage at a time.

## Practical Approach

1. Identify the stage that first produces bad output.
2. Run that stage directly with the smallest configuration that reproduces the issue.
3. Debug that stage before you go back to the full pipeline launcher.

## Launch Template Strategy

Use the companion template in the examples repo:

- [TXPipe examples directory](https://github.com/Lhior/CosmoCoding-examples/tree/main/software/txpipe)

The template includes one configuration for the intentionally buggy practice script and one placeholder for a real TXPipe or `ceci`-driven run.

Fields you usually need to edit:

- the interpreter that has TXPipe installed,
- the pipeline YAML path,
- the specific stage name,
- any `PYTHONPATH` entries if you are debugging a local checkout.

## Common Failure Modes

- Stage configuration paths are wrong.
- The pipeline launcher is correct, but the stage-level imports are from the wrong environment.
- You are trying to debug a multi-stage run before confirming the stage that actually fails.

## Recommended Debug Points

- Config parsing for the stage.
- The first place input data is loaded.
- The output-writing boundary where arrays or tables change shape.
