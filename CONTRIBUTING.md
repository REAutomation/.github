# Contributing Guidelines

Guidelines for contributing to REAutomation repositories.

## Repository Naming

Before creating a new repository, read [REPO_NAMING.md](REPO_NAMING.md).

## Branching Strategy

### Main Branches
- `main` - Production-ready code
- `develop` - Integration branch (if needed)

### Feature Branches
```
feature/[short-description]
feature/add-motion-control
feature/update-hmi-layout
```

### Bugfix Branches
```
bugfix/[short-description]
bugfix/fix-communication-timeout
```

## Commit Messages

Use clear, descriptive commit messages:

```
[type]: [short description]

[optional body]
```

### Types
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation
- `refactor:` - Code refactoring
- `test:` - Adding tests
- `chore:` - Maintenance

### Examples
```
feat: add TCP client functionality
fix: resolve timeout issue in motion controller
docs: update API documentation
refactor: simplify state machine logic
```

## CODESYS Specific

### Project Structure
Follow the standard CODESYS Git structure with `.project` files.

### Libraries
- Use semantic versioning (MAJOR.MINOR.PATCH)
- Document breaking changes
- Update library version in project settings

## Code Review

- All changes to `main` should be reviewed
- Use Pull Requests for collaboration
- Link to Jira tickets where applicable

## Questions?

Contact the repository owner or check `internal-docs`.
