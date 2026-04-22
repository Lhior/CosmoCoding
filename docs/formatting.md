# Code Formatting

Code formatting is one of the cheapest ways to make collaboration less painful. If multiple people are working in the same repository, a formatter removes most of the low-value arguments about whitespace, import ordering, and line wrapping so code review can stay focused on behavior.

## Why It Helps

- It keeps style consistent across collaborators, even when people use different editors.
- It reduces noisy diffs caused by hand formatting.
- It makes code review faster because style is mostly automated.
- It lowers the barrier for new contributors because the repository has a predictable style.

For scientific collaboration, this matters more than people often admit. Shared repositories accumulate enough friction already from environments, data paths, and pipeline configuration. Formatting should not be another source of noise.

## What the Common Python Tools Do

### Black

Black is an opinionated formatter. Its job is to rewrite Python code into a consistent layout with minimal configuration.

Use it for:

- line wrapping,
- spacing,
- quote and bracket normalization,
- removing style drift across files.

### isort

isort organizes imports into a predictable order. That is especially useful in projects with a mix of standard-library, scientific Python, and local pipeline imports.

Use it for:

- sorting imports alphabetically,
- grouping standard-library, third-party, and local imports,
- reducing merge conflicts in import blocks.

### Flake8

Flake8 is a linter rather than a formatter. It checks for style problems and common mistakes instead of rewriting the file for you.

Use it for:

- syntax and style warnings,
- unused imports or variables,
- simple correctness issues that a formatter will not fix.

In a typical workflow, `isort` runs first, `black` formats the result, and `flake8` acts as a guardrail.

## Install the VS Code Extensions

For a clean Python formatting workflow in VS Code, install these extensions:

- Python
- Black Formatter
- isort
- Flake8

You can install them from the Extensions view by searching for those exact names.

For reproducibility across machines and CI, it is also a good idea to install the tools into the active project environment:

```bash
pip install black isort flake8
```

That way, your editor, terminal, and CI can all use the same tools and versions.

## Recommended VS Code Settings

These settings are a good default for Python repositories that want automatic formatting and import cleanup on save:

```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.organizeImports": "explicit"
  },
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter"
  },
  "isort.args": [
    "--profile",
    "black"
  ],
  "flake8.args": [
    "--max-line-length=88"
  ]
}
```

Why these choices:

- `formatOnSave` means people do not have to remember to run the formatter manually.
- `editor.defaultFormatter` ensures Python files use Black instead of whatever formatter happens to be installed.
- `isort --profile black` keeps import formatting compatible with Black.
- the `flake8` line length is set to 88 to match Black's default.

If you want imports to be sorted automatically on every save instead of only when explicitly requested, you can switch `"source.organizeImports": "explicit"` to `true`. Some teams prefer the explicit mode because it makes save behavior a little less surprising.

## Recommended Project Configuration

If the repository has more than one contributor, put the style rules in version-controlled config instead of relying only on each person's editor settings.

A minimal `pyproject.toml` example is:

```toml
[tool.black]
line-length = 88

[tool.isort]
profile = "black"
line_length = 88

[tool.flake8]
max-line-length = 88
extend-ignore = ["E203"]
```

This gives contributors and CI the same formatting contract.

## A Practical Workflow

1. Install the VS Code extensions.
2. Install `black`, `isort`, and `flake8` in the project environment.
3. Turn on format-on-save.
4. Commit the formatting config to the repository.
5. Run the formatter before large refactors so code review stays readable.

## What To Avoid

- Do not let every collaborator choose a different formatter.
- Do not rely only on editor-local settings if the repository is shared.
- Do not mix incompatible line-length rules between Black, isort, and Flake8.
- Do not reformat the entire repository in the same commit as behavioral changes unless you really need to.

Formatting works best when it is boring, automatic, and consistent.
