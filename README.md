# Claude Code Template

A starter repository preconfigured for [Claude Code](https://claude.com/claude-code): settings, a session-handoff workflow, ignore rules, and LF line endings from the first commit.

## What's included

| File | Purpose |
|------|---------|
| `.claude/settings.json` | Claude Code settings (`model: opus`, `effortLevel: xhigh`), a permission allowlist, and a `SessionStart` hook that prints `PROGRESS.md` into context. |
| `.claude/commands/wrapup.md` | `/wrapup` — writes a session handoff to `PROGRESS.md`, then commits. |
| `CLAUDE.md` | Project instructions Claude Code loads every session: code principles, explanation style, conventions, environment. Edit to fit your project. |
| `PROGRESS.md` | Session handoff maintained by `/wrapup`. |
| `.gitignore` | OS cruft, Python/Node build artifacts, editor files. |
| `.gitattributes` | LF line endings; binary file types. |
| `LICENSE` | Project license. |

## Usage

1. Click **Use this template** on GitHub, or clone the repo.
2. Delete what you don't need and start building.
3. Adjust `.claude/settings.json` to taste ([settings docs](https://code.claude.com/docs/en/settings)). Personal overrides go in `.claude/settings.local.json`, which is gitignored.

## Development setup (Debian 13)

`docker` is assumed installed. Base toolchain:

```bash
sudo apt install -y git gh python3 python3-dev build-essential curl jq wget
```

`python3-dev` and `build-essential` are only needed for Python packages built from source.

`uv` is not packaged for Debian; install it with the standalone installer (needs `curl` from above). It puts `uv` and `uvx` in `~/.local/bin`, which Debian's default `~/.profile` adds to `PATH` on next login:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

Verify: `git --version && gh --version && curl --version && uv --version`.

**More tools.** `sudo` is passwordless, and Claude Code may install whatever a task needs:

```bash
sudo apt update && sudo apt install -y <package>
```

Use `apt` for system tools; keep Python packages in the project venv. Add any lasting dependency to the command above.

**Python.** Use `uv` for every venv and package install, not `pip` or `python3 -m venv` (Debian 13 also blocks system-wide `pip install`, PEP 668). One venv per project:

```bash
uv sync                                          # project with a pyproject.toml; add packages with: uv add <pkg>
uv venv && uv pip install -r requirements.txt    # project with only a requirements.txt
```

For a packaged CLI, `uv tool install .` avoids managing a venv by hand.

**Docker multi-arch (optional, one-time).** Cross-building `linux/amd64` + `linux/arm64` needs QEMU and a `docker-container` builder:

```bash
docker run --privileged --rm tonistiigi/binfmt --install all
docker buildx create --name multiarch --driver docker-container --bootstrap --use
```

On Apple Silicon with OrbStack, emulation is built in, so only the second line is needed.

**Not used.** `node`/`npm` (serve static content with `python3 -m http.server`) and the .NET SDK.

## License

See [LICENSE](LICENSE).
