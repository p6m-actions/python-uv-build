# Python UV Build

![Latest Release](https://img.shields.io/github/v/release/p6m-actions/python-uv-build?style=flat-square&label=Latest%20Release&color=blue)

## Description

A comprehensive GitHub Action for building Python projects using UV with flexible lint, test, and build commands. This action provides intelligent detection of project configurations and supports customizable workflows for Python development.

## Features

- **Flexible Commands**: Customize lint, test, and build commands to fit your project needs
- **Intelligent Detection**: Automatically detects and validates project configurations in `pyproject.toml`
- **UV Integration**: Built specifically for UV-managed Python projects with sensible defaults
- **Coverage Support**: Optional code coverage archiving with configurable paths
- **Error Handling**: Comprehensive error reporting and graceful handling of missing configurations
- **Dependency Management**: Automatic dependency synchronization using UV

## Usage

### Basic Usage

```yaml
- name: Setup Python with UV
  uses: p6m-actions/python-uv-setup@v1
  with:
    python-version: "3.11"
    
- name: Build Python Project
  uses: p6m-actions/python-uv-build@v1
```

### Advanced Usage

```yaml
- name: Build with Custom Configuration
  uses: p6m-actions/python-uv-build@v1
  with:
    lint-command: "uv run ruff check"
    lint-args: "--fix --unsafe-fixes"
    test-command: "uv run pytest"
    test-args: "--cov=src --cov-report=html"
    build-command: "uv build"
    archive-coverage: "true"
    coverage-path: "htmlcov"
```

## Inputs

### Lint Configuration

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `run-lint` | Whether to run linting | `true` | No |
| `lint-command` | Command to run for linting | `uv run ruff check` | No |
| `lint-args` | Additional arguments for the lint command | `""` | No |

### Test Configuration

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `run-test` | Whether to run tests | `true` | No |
| `test-command` | Command to run for testing | `uv run pytest` | No |
| `test-args` | Additional arguments for the test command | `""` | No |

### Build Configuration

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `run-build` | Whether to run build | `true` | No |
| `build-command` | Command to run for building | `uv build` | No |
| `build-args` | Additional arguments for the build command | `""` | No |

### Python Configuration

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `python-options` | Python options for all processes | `""` | No |

### Coverage Configuration

| Name | Description | Default | Required |
|------|-------------|---------|----------|
| `archive-coverage` | Whether to archive code coverage results | `false` | No |
| `coverage-path` | Path to the coverage reports | `htmlcov` | No |

## Examples

### Complete Workflow with Python UV Setup

```yaml
name: Build and Test

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        
      - name: Setup Python with UV
        uses: p6m-actions/python-uv-setup@v1
        with:
          python-version: "3.11"
          
      - name: Build Python Project
        uses: p6m-actions/python-uv-build@v1
```

### Custom Commands and Coverage

```yaml
name: Custom Build Pipeline

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        
      - name: Setup Python with UV
        uses: p6m-actions/python-uv-setup@v1
        with:
          python-version: "3.12"
          
      - name: Build with Coverage
        uses: p6m-actions/python-uv-build@v1
        with:
          lint-command: "uv run ruff check"
          lint-args: "--fix"
          test-command: "uv run pytest"
          test-args: "--cov=src --cov-report=html --cov-report=xml"
          archive-coverage: "true"
          coverage-path: "htmlcov"
```

### Skip Specific Steps

```yaml
name: Build Only

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        
      - name: Setup Python with UV
        uses: p6m-actions/python-uv-setup@v1
        with:
          python-version: "3.11"
          
      - name: Build Without Testing
        uses: p6m-actions/python-uv-build@v1
        with:
          run-lint: "true"
          run-test: "false"
          run-build: "true"
```

### FastAPI Project Example

```yaml
name: FastAPI Build

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        
      - name: Setup Python with UV
        uses: p6m-actions/python-uv-setup@v1
        with:
          python-version: "3.11"
          
      - name: Build FastAPI Project
        uses: p6m-actions/python-uv-build@v1
        with:
          test-command: "uv run pytest"
          test-args: "--cov=app --cov-report=html"
          archive-coverage: "true"
```

## Requirements

- **Python Project**: Requires a `pyproject.toml` file for build operations
- **UV Setup**: This action should be used after `python-uv-setup` action
- **Test Files**: For pytest, requires test files matching `test_*.py` or `*_test.py` patterns, or pytest configuration in `pyproject.toml`
- **Ruff Configuration**: For ruff linting, requires `[tool.ruff]` section in `pyproject.toml`

## Troubleshooting

### Common Issues

**"No ruff configuration found in pyproject.toml"**
- Add a `[tool.ruff]` section to your `pyproject.toml` file
- Or set `run-lint: "false"` to skip linting

**"No pytest configuration or test files found"**
- Add test files with names matching `test_*.py` or `*_test.py`
- Or add `[tool.pytest]` section to your `pyproject.toml`
- Or set `run-test: "false"` to skip testing

**"No pyproject.toml found"**
- Ensure your project has a `pyproject.toml` file for build operations
- Or set `run-build: "false"` to skip building