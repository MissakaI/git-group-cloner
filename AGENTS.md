# Repository Guidelines

## Project Structure & Module Organization

This repository is a small POSIX shell project centered on the executable `git-group-cloner`. Keep production logic in that file unless the script grows enough to justify splitting helpers into separate shell modules. `README.md` contains user-facing usage, dependency, and FAQ content. `docs/call_graph.svg` is a generated architecture aid, and `justfile` contains the command used to regenerate it. There is currently no dedicated `tests/` directory or packaged asset tree.

## Build, Test, and Development Commands

- `sh -n git-group-cloner`: checks shell syntax without running API calls.
- `./git-group-cloner --help`: verifies the executable starts and prints usage.
- `just gen_call_graph`: regenerates `docs/call_graph.svg` using `callGraph`; run this after changing function relationships.
- `shellcheck git-group-cloner`: recommended static analysis when `shellcheck` is installed.

The script depends on `git`, `jq`, and either `curl` or `wget` at runtime. Set `TOKEN` before running real GitHub or GitLab API operations.

## Coding Style & Naming Conventions

Write portable `/bin/sh`, not Bash-specific code. Use four-space indentation inside most functions and keep variable names lower_snake_case for local script state, reserving uppercase names for environment variables and constants such as `TOKEN`, `GITLAB_BASE_URL`, and `ERROR_INVALID_JSON`. Quote variable expansions unless word splitting is intentional. Prefer small functions with action-oriented names like `make_api_request`, `get_projects`, and `run_git_clones`.

## Testing Guidelines

No automated test suite is present yet. For each change, at minimum run `sh -n git-group-cloner` and exercise non-destructive paths such as `--help` or list operations against a test group. When adding tests, place them under `tests/`, prefer fixtures for API JSON responses, and name cases after behavior, for example `tests/parse_url_test.sh`.

## Commit & Pull Request Guidelines

Recent history uses short imperative commits with occasional Conventional Commit prefixes, for example `fix: dash -> bash` and `Add justfile and docs/call_graph.svg`. Keep commit subjects concise and describe user-visible behavior. Pull requests should include the reason for the change, commands run for validation, any API-platform impact, and updated docs or call graph output when relevant.

## Security & Configuration Tips

Do not commit tokens, cloned repositories, or local output directories. Use `TOKEN`, `GITLAB_BASE_URL`, and `GITHUB_BASE_URL` from the environment, and avoid printing authorization headers in logs.
