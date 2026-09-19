# Pyengi

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
![Badge](https://img.shields.io/badge/badge)

[![Code style: Ruff](https://img.shields.io/badge/code%20style-Ruff-D7FF64?logo=ruff&logoColor=111111)](https://docs.astral.sh/ruff/)
[![Status: early development](https://img.shields.io/badge/status-early%20development-orange.svg)](#project-status)

**Pyengi is a developer helper toolkit for making everyday development work faster, more repeatable, and easier to automate.**

> The project is currently in early development. The commands and API shown below are intended as a clear starting point and should be updated as the implementation evolves.

## Why Pyengi?

Developers lose time to small, repetitive tasks: inspecting a project, checking configuration, generating boilerplate, validating files, and running the same setup steps across repositories. Pyengi is intended to collect those helpers in one consistent, scriptable tool instead of scattering them across ad-hoc shell commands.

## Project status

Pyengi is under active development. The repository is not yet considered stable, and interfaces may change between releases. If you are interested in a particular helper, open an issue describing the workflow you want to improve.

## Planned capabilities

The initial scope for Pyengi includes:

- **Project inspection** — summarize a repository’s structure and development configuration.
- **Developer utilities** — run common checks and repetitive maintenance tasks through one interface.
- **Scaffolding** — create consistent starter files and directories for new work.
- **Automation-friendly output** — support readable terminal output as well as machine-readable results where useful.
- **Extensibility** — make it straightforward to add new helpers without changing the whole command-line interface.

These are project goals, not a promise that every capability is already available.

## Installation

Pyengi does not yet have a published package release. Once packaging is available, installation will look like this:

```bash
python -m pip install pyengi
```

For local development, clone the repository and install it in editable mode:

```bash
git clone https://github.com/<your-github-username>/pyengi.git
cd pyengi
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\\Scripts\\activate
python -m pip install --upgrade pip
python -m pip install -e ".[dev]"
```

Replace `<your-github-username>` with the account or organization that owns the repository.

## Usage

The command-line interface is expected to follow a simple subcommand pattern:

```bash
pyengi --help
pyengi --version
```

Example workflows may look like:

```bash
# Inspect the current project
pyengi inspect .

# Run the project’s developer checks
pyengi check

# Generate a helper or starter configuration
pyengi init
```

The exact commands should be kept in sync with the CLI implementation. Run `pyengi --help` for the authoritative list of supported options.

### Python API

If Pyengi exposes a Python API, a typical integration may look like this:

```python
from pyengi import Project

project = Project.from_path(".")
print(project.summary())
```

This example is illustrative until the public API has been finalized.

## Development

Create a virtual environment before making changes:

```bash
python -m venv .venv
source .venv/bin/activate  # Windows: .venv\\Scripts\\activate
python -m pip install --upgrade pip
python -m pip install -e ".[dev]"
```

Run the test suite and quality checks with the commands configured by the project:

```bash
pytest
ruff check .
ruff format --check .
```

If a command is not yet configured, add it to the project’s development setup before treating it as a required check.

## Configuration

Pyengi should prefer explicit command-line options and project-local configuration over hidden global state. When configuration support is implemented, document the supported file names, environment variables, precedence rules, and examples in this section.

Do not commit secrets, tokens, private keys, or machine-specific credentials to the repository.

## Contributing

Contributions are welcome. Before opening a pull request:

1. Open an issue for larger changes so the proposed behavior can be discussed.
2. Keep each pull request focused on one improvement.
3. Add or update tests for behavior that changes.
4. Run the formatter, linter, and test suite locally.
5. Update this README when user-facing commands or configuration change.

Please use clear commit messages and include a short explanation of the motivation, implementation, and testing in each pull request.

## Roadmap

The roadmap will be refined as the project takes shape. Likely milestones include:

- Establish the core command-line interface.
- Add the first stable developer helpers.
- Define a plugin or extension model.
- Add automated tests and continuous integration.
- Publish versioned releases and usage documentation.

## License

Pyengi is intended to be released under the [MIT License](LICENSE). Add the complete license text to `LICENSE` before publishing the repository.

## Support and feedback

For bugs and feature requests, [open an issue](../../issues). For questions or broader design discussions, start a discussion if GitHub Discussions is enabled for the repository.

## Acknowledgements

Pyengi is inspired by the small tools and scripts developers build to remove friction from their daily workflows.

<!--
GitHub setup checklist:
- Replace <your-github-username> in the clone URL.
- Update the badges with the real repository owner and CI status.
- Add the actual supported Python versions and commands.
- Add LICENSE, pyproject.toml, tests, and a GitHub Actions workflow.
- Remove illustrative API examples once the public interface is finalized.
-->

[MIT License]: https://opensource.org/license/mit/
[Python]: https://www.python.org/
[Ruff]: https://docs.astral.sh/ruff/
