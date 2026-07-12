# uv-demo

A small repo for practicing `uv`, project environments, and dependency
troubleshooting.

The point is not the Python code itself. The point is to see how a project
records the packages it needs, how `uv` uses those records, and what to do when
a script asks for a package that is not available in the current project
environment.

## Demo flow

After cloning the repo:

```bash
uv run python hello.py
```

This should run without extra packages.

Then try:

```bash
uv run python orders_summary.py
```

You will likely hit a `ModuleNotFoundError` for `pandas`. If you do not, your
machine may already have enough package state available for this command to get
farther than expected. That is part of the reason project environments matter:
different machines can start from different states.

Next, build the project environment from the project files:

```bash
uv sync
uv run python orders_summary.py
```

Now the script should get past `pandas`, but hit a `ModuleNotFoundError` for
`seaborn`.

Look at `pyproject.toml`, then add the missing dependency:

```bash
uv add seaborn
```

Look at `pyproject.toml` again, then rerun:

```bash
uv run python orders_summary.py
```

## What to notice

- `pyproject.toml` records project dependencies.
- `uv.lock` records exact resolved package versions after syncing or adding.
- `.venv/` is the local project environment.
- Git should track scripts, data, and dependency metadata.
- Git should not track `.venv/`.
