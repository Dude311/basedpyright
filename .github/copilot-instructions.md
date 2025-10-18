# AI Assistant Instructions for basedpyright

This guide helps AI coding assistants understand the key aspects of the basedpyright codebase, a fork of Microsoft's pyright with enhanced type checking and features.

## Project Overview

Basedpyright is a Python type checker and language server that extends pyright with:
- Improved type checking rules
- Pylance features
- Better defaults and configuration handling
- Enhanced CI integration
- Language server improvements

## Architecture

The project consists of several key components:
- `/basedpyright/` - Python wrapper scripts for the CLI and language server
- `/packages/pyright/` - Core type checker implementation
- `/packages/pyright-internal/` - Shared internal modules
- `/packages/vscode-pyright/` - VS Code extension
- `/build/` - Build scripts and version management
- `/tests/` - Python test suite

## Development Environment

Key aspects to understand:

1. **Dependency Management**:
   - Uses `uv` for Python dependencies and virtualenv management
   - Node.js is managed via `nodejs-wheel` package - no manual Node.js installation needed
   - Run `./pw uv sync` to install all dependencies

2. **Build System**:
   - TypeScript compilation via `npm run watch:extension` or `npm run watch:testserver`
   - Python package building handled by `pdm-backend`
   - Version management in `build/py3_8/version.py`

3. **Testing and Validation**:
   - Uses multiple linters: basedpyright (self-hosted), ruff, pylint
   - Pytest for Python tests
   - Jest for TypeScript tests

## Common Tasks

1. **Building**:
   ```powershell
   ./pw uv sync  # Install dependencies
   ```

2. **Running Tests**:
   - Python tests: `pytest`
   - TypeScript tests: Use Jest debug configuration

3. **Debugging**:
   - CLI: Use "Pyright CLI" debug target
   - VS Code Extension: Use "Pyright extension" + "Pyright extension attach server"
   - Language Server: Use "LSP client" launch config

## Project Conventions

1. **Code Style**:
   - Python: Strict type annotations required (checked by basedpyright)
   - Ruff for formatting and linting with extensive rule set
   - Complex pylint configuration for rules not yet in ruff

2. **Error Handling**:
   - Type checking errors follow pyright's error code system
   - Custom error codes prefixed with "BP" for basedpyright-specific rules

3. **Documentation**:
   - Major features documented in `/docs/`
   - Type checking concepts in `/docs/getting_started/type-concepts.md`
   - Development guide in `/docs/development/`

## Integration Points

1. **Language Server Protocol**:
   - LSP implementation in `basedpyright/langserver.py`
   - Supports LSP inspector for debugging (see `npm: lsp-inspect` task)

2. **VS Code Extension**:
   - Extension configuration in `packages/vscode-pyright/package.json`
   - Uses standard VS Code extension APIs

## Best Practices

1. Always run type checking and linting before commits:
   ```powershell
   basedpyright .
   ruff check .
   pylint basedpyright build tests
   ```

2. For language server changes:
   - Test with LSP inspector/client
   - Verify behavior in multiple editors (VS Code, Sublime, etc.)

3. When adding features:
   - Document in `/docs/benefits-over-pyright/`
   - Add tests in `/tests/`
   - Update type stubs if needed

## Common Pitfalls

1. Windows path handling requires special attention in language server code
2. Type checking rules must account for Python version differences (3.8+)
3. Avoid direct node/npm commands - use pw wrapper script instead