# FastAPI Example

[![Checked with mypy](https://www.mypy-lang.org/static/mypy_badge.svg)](https://mypy-lang.org/)
[![Linting: Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/charliermarsh/ruff/main/assets/badge/v2.json)](https://github.com/astral-sh/ruff)
[![Security: bandit](https://img.shields.io/badge/security-bandit-yellow.svg)](https://github.com/PyCQA/bandit)

## Overview

This repo contains a simple [FastAPI](https://fastapi.tiangolo.com) example along with standard Poetry setup, as well as [Mypy](https://github.com/python/mypy) static type checking,
code formatting and linting with [Ruff](https://github.com/astral-sh/ruff) and security linting with [Bandit](https://github.com/PyCQA/bandit). All
code quality checks are facilitated using [pre-commit](https://github.com/pre-commit/pre-commit) hooks. For SSL-related issues on Kainos machines in relation to package installation
via Poetry, see the [SharePoint guide](https://kainossoftwareltd.sharepoint.com/sites/InformationSecurity/SitePages/Corporate-Certification-Dev-Tool-Setup.aspx#python%2C-pyenv-poetry). The repo also uses [Task](https://taskfile.dev) (see the `taskfile.yaml` in the root) - see the docs for installation instructions.

## Local Setup

After installing Python, Poetry and Task, you can run the example setup task which will configure the Poetry virtual environment and install the pre-commit hooks:

```bash
task setup-local
```

## Running the API

To run the project, ensure you have Python 3.11 installed (ideally via [Pyenv](https://github.com/pyenv/pyenv)), as welll as [Poetry](https://github.com/python-poetry/poetry), then, from the terminal run:

```bash
poetry install
poetry run fastapi dev src/fast_api_example/main.py
```

Once the API is running, make a request:

```bash
curl http://127.0.0.1:8000
{"Hello":"World"}

curl http://127.0.0.1:8000/items/1234?query=somequery
{"item_id":1234,"query":"somequery"}
```

## Running Code Quality Checks

To run the project's default code quality checks, make sure to install the configured pre-commit hooks, then run them, after which
any commit to the repo will trigger the hooks:

```bash
poetry run pre-commit install
poetry run pre-commit run --all-files

Mypy static type checking................................................Passed
Ruff code linting........................................................Passed
Ruff code formatting.....................................................Passed
Bandit security linting..................................................Passed
```

If needed, hooks can be pypasses by adding the `--no-verify` flag to the commit, but avoid this wherever possible.

## Running Tests

The project has some simple unit tests defined in `tests/unit` which can be invoked via `pytest`:

```bash
poetry run pytest tests/unit --cov=src --cov-report=term-missing

============================ test session starts ============================
platform darwin -- Python 3.11.0, pytest-8.3.4, pluggy-1.5.0
rootdir: /Users/jamie/Documents/repos/internal/fast-api-example
configfile: pyproject.toml
plugins: cov-6.0.0, anyio-4.7.0
collected 2 items                                                                                                                                               

tests/unit/test_app.py ..                                       [100%]

---------- coverage: platform darwin, python 3.11.0-final-0 ----------
Name                               Stmts   Miss  Cover   Missing
----------------------------------------------------------------
src/fast_api_example/app.py           12      0   100%
src/fast_api_example/scripts.py        3      3     0%   1-5
----------------------------------------------------------------
TOTAL                                 15      3    80%

1 empty file skipped.


============================ 2 passed in 1.07s ============================
```

As shown above, we use `pytest-cov` to generate a coerage report showing our test coverage and highlighting
explicitly the lines in our code that are missing tests. Alternatively, you can invoke the unit tests using the example task:

```bash
task unit-tests
```

## Build a Deployable Artifact

### Docker Container

[TODO]

### Python Wheel or Package

To build the API into a Python wheel which can be pushed to a package repository or deployed directly to a server, from the terminal simply run:

```bash
poetry build
```

This will create a `dist` folder in the root containing a `.whl` file, and a `.tar.gz` file. These files can either be deployed directly to a remote environment or published to package repository using the `poetry publish` command.

## Run the API

### Docker Container

[TODO]

### Python Wheel or Package

Assuming the package has either been deployed to the server or pushed to a package respository, and that an appropriate Python version is installed, create a virtual environment and install the wheel/package:

```bash
python -m venv. venv
source .venv/bin/activate

# From wheel
pip install </path/to/api/.whl>

# From package manager
pip install fast-api-example
```

Finally, invoke the API startup script (see `tool.poetry.scripts` in `pyproject.toml`):

```bash
api-start

INFO:     Started server process [15561]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

For details on more advanced aspects of deployment (e.g., HTTPS) and alternate deployment
approaches, see the [FastAPI deployment docs](https://fastapi.tiangolo.com/deployment/).