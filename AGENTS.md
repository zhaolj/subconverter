# Repository Guidelines

## Project Structure & Module Organization

This C++20 service converts proxy subscriptions between formats. `src/parser/` reads subscriptions; `src/generator/` produces configurations; `src/handler/` and `src/server/` expose the HTTP interface. Shared utilities and configuration types live in `src/utils/` and `src/config/`. `include/` contains bundled headers. Runtime examples, rules, profiles and templates live under `base/`; CMake discovery modules are in `cmake/`. Platform release scripts are in `scripts/`, and CI workflows are in `.github/workflows/`.

## Build, Test, and Development Commands

Install the dependencies listed in `CMakeLists.txt`, including curl, RapidJSON, toml11, yaml-cpp, PCRE2, QuickJS and libcron, then run:

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j 4
```

The first command configures an out-of-tree build; the second builds the executable. For a self-contained runtime, place the executable alongside the configuration files in a copy of `base/`, then start it there. Check a running server with `curl http://127.0.0.1:25500/version`. See `README-docker.md` for container usage and `scripts/build.*.release.sh` for platform-specific release builds; inspect those scripts before running them because they install dependencies and modify files.

## Coding Style & Naming Conventions

Follow the surrounding C++ code: four-space indentation, braces on separate lines for functions and control blocks, and descriptive names consistent with the touched module. Keep `.cpp` and corresponding `.h` files together. Match existing conventions in configuration formats and scripts. CMake enables compiler warnings; no repository-wide formatter configuration is provided.

## Testing Guidelines

The repository does not currently define a project test suite or coverage threshold in CMake. Build the affected target and exercise changed behavior with representative, sanitized subscription inputs and output formats. For HTTP changes, also check the relevant endpoint and `/version`; describe the cases and results in the pull request. Add focused regression tests when practical and document how to run them.

## Commit & Pull Request Guidelines

Recent commits use short imperative subjects, often with Conventional Commit prefixes such as `fix(clash):` or `feat:`. Keep changes focused. Pull requests should explain the problem and solution, link relevant issues, and list build and manual verification results. Include example input and output for conversion changes, with credentials and subscription tokens removed.

## Security & Configuration

Treat subscription URLs, node credentials and upload tokens as secrets. Use placeholders in fixtures, logs and documentation; never commit personal runtime configuration or generated caches.

## Agent skills

### Issue tracker

Issues are tracked in GitHub Issues (github.com/zhaolj/subconverter) via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Uses the five default triage label strings as-is. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: `GLOSSARY.md` + `docs/adr/` at the repo root. See `docs/agents/domain.md`.
