# Project Rules & Guidelines

## Project Overview
This is a **recsys** (Recommendation System) project implementing BigQuery-based item-to-item collaborative filtering for e-commerce. The project uses **uv** as the primary package manager and development tool.

## Development Environment

### Prerequisites
- Python 3.9+ (supports 3.9-3.13)
- uv package manager
- Git with pre-commit hooks

### Setup
```bash
# Install development environment
make install

# This will:
# - Create virtual environment using uv
# - Install all dependencies
# - Set up pre-commit hooks
```

## Package Management Rules

### Use uv exclusively
- ❌ Never use `pip install` directly
- ✅ Use `uv add <package>` for runtime dependencies
- ✅ Use `uv add --group dev <package>` for development dependencies
- ✅ Use `uv sync` to install from lock file
- ✅ Use `uv run <command>` to execute commands in the virtual environment

### Dependency Groups
- **Runtime**: Add to `[project.dependencies]` in pyproject.toml
- **Development**: Add to `[dependency-groups.dev]` in pyproject.toml
- Always specify version constraints for stability

## Code Quality Standards

### Type Safety (mypy)
- All code must have type annotations
- Configuration: `disallow_untyped_defs = true`
- No implicit optionals allowed
- Check untyped definitions

### Code Formatting & Linting (ruff)
- Line length: 120 characters
- Target version: Python 3.9+
- Auto-fix enabled
- Comprehensive rule set (security, performance, style)

### Pre-commit Hooks
- Run automatically on commit
- Fix issues before committing: `uv run pre-commit run -a`
- Never bypass pre-commit hooks

## Testing Requirements

### Coverage Standards
- Minimum coverage enforced via pytest-cov
- Source coverage from `src/` directory
- Branch coverage enabled
- Skip empty files in reports

### Test Structure
- Tests in `tests/` directory
- Test file naming: `test_*.py`
- Import from package: `from recsys.module import function`

### Running Tests
```bash
# Local testing
make test

# Multi-version testing
tox

# Specific Python version
tox -e py311
```

## Development Workflow

### Daily Commands
```bash
# Check code quality
make check

# Run tests
make test

# Build documentation
make docs

# Build package
make build
```

### Code Changes Process
1. Create feature branch from main
2. Make changes following coding standards
3. Run `make check` to verify quality
4. Run `make test` to ensure tests pass
5. Commit (pre-commit hooks will run)
6. Push and create PR

## File Organization

### Source Code Structure
```
src/recsys/
├── __init__.py          # Package initialization
├── models/             # ML models and algorithms
├── data/               # Data processing utilities
├── bigquery/           # BigQuery-specific code
└── utils/              # Helper functions
```

### Documentation
- Use Google-style docstrings
- All public functions must be documented
- Example format in `src/recsys/foo.py`

## BigQuery Specific Rules

### SQL Best Practices
- Use parameterized queries
- Implement proper error handling
- Follow BigQuery optimization patterns
- Document complex queries thoroughly

### Data Processing
- Process data in chunks for large datasets
- Implement proper logging
- Use appropriate BigQuery job configurations
- Handle rate limits and quotas

## CI/CD Guidelines

### Automated Checks
- Code quality (ruff, mypy)
- Test coverage
- Documentation builds
- Multi-version compatibility (tox)

### Release Process
1. Update version in pyproject.toml
2. Update CHANGELOG.md
3. Create release tag
4. Automated build and publish to PyPI

## Documentation Standards

### MkDocs Configuration
- Material theme for modern appearance
- API documentation auto-generated
- Code examples must be tested
- Keep documentation current with code

### README Maintenance
- Installation instructions
- Quick start guide
- Link to full documentation
- Project status and roadmap

## Security & Performance

### Security Rules
- No hardcoded credentials
- Use environment variables for sensitive data
- Regular dependency updates
- Security linting enabled (bandit rules)

### Performance Guidelines
- Profile BigQuery queries
- Monitor memory usage for large datasets
- Implement proper caching strategies
- Use async where appropriate

## Dependency Management

### Version Pinning Strategy
- Pin major versions for stability
- Allow minor/patch updates
- Review dependencies quarterly
- Use `uv lock` to generate lock file

### Dependency Groups
```toml
[dependency-groups]
dev = [
    "pytest>=7.2.0",
    "pre-commit>=2.20.0", 
    "tox-uv>=1.11.3",
    "deptry>=0.23.0",
    "mypy>=0.991",
    "pytest-cov>=4.0.0",
    "ruff>=0.11.5",
    "mkdocs>=1.4.2",
    "mkdocs-material>=8.5.10",
    "mkdocstrings[python]>=0.26.1",
]
```

## Error Handling

### Exception Guidelines
- Use specific exception types
- Include helpful error messages
- Log errors appropriately
- Implement proper cleanup in finally blocks

### BigQuery Error Handling
- Handle quota exceeded errors
- Retry transient failures
- Provide meaningful error messages for SQL errors

## Configuration Management

### Environment Variables
- Use `.env` files for local development
- Document all required environment variables
- Provide sensible defaults where possible
- Never commit sensitive configuration

## Monitoring & Logging

### Logging Standards
- Use structured logging (JSON format recommended)
- Include appropriate context in log messages
- Set appropriate log levels
- Avoid logging sensitive information

### Performance Monitoring
- Track BigQuery job performance
- Monitor memory usage
- Log processing times for large datasets
- Set up alerts for failures

## Troubleshooting

### Common Issues
1. **uv.lock missing**: Run `uv sync` to generate
2. **Pre-commit failures**: Run `uv run pre-commit run -a`
3. **Type errors**: Check mypy configuration and add type annotations
4. **Test failures**: Ensure proper test isolation and cleanup

### Debug Mode
- Enable verbose logging for debugging
- Use BigQuery dry-run for query validation
- Test with smaller datasets during development

---

## Quick Reference

### Essential Commands
```bash
# Setup
make install

# Development
make check test docs

# Package management  
uv add <package>
uv sync
uv run <command>

# Quality checks
uv run pre-commit run -a
uv run mypy
uv run pytest
```

### Key Files
- `pyproject.toml` - Project configuration
- `Makefile` - Development commands
- `tox.ini` - Multi-version testing
- `docs/` - Documentation source
- `tests/` - Test suite
