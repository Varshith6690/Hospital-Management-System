# Contributing to HMS

Thank you for your interest in contributing to Hospital Management System!

## Getting Started

1. **Fork** the repository
2. **Clone** your fork
3. **Create** a feature branch

```bash
git checkout -b feature/amazing-feature
```

4. **Make** your changes
5. **Commit**

```bash
git commit -m "Add amazing feature"
```

6. **Push**

```bash
git push origin feature/amazing-feature
```

7. **Open** a Pull Request

## Coding Standards

### Backend

- Use ESLint for code linting
- Follow Express.js best practices
- Write meaningful commit messages
- Add error handling for all routes
- Use middleware for validation

### Frontend (Angular)

- Follow Angular style guide
- Use OnPush change detection where appropriate
- Write unit tests for services
- Use lazy loading for feature modules
- Follow TypeScript strict mode

### Mobile (React Native)

- Use React hooks (avoid class components)
- Follow Expo best practices
- Optimize performance with useMemo and useCallback
- Write meaningful component names

## Testing

All code must include tests.

```bash
# Backend
cd backend && npm test

# Web Frontend
cd web-frontend && npm test

# Mobile App
cd mobile-app && npm test
```

## Commit Messages

Follow Conventional Commits:

```text
feat: Add new feature
fix: Fix bug in component
docs: Update documentation
style: Format code
refactor: Refactor component
test: Add unit tests
chore: Update dependencies
```

## Security

- Never commit `.env` files
- Never hardcode secrets
- Report security issues privately
- Use HTTPS for all API calls
- Validate all user inputs

## Documentation

Update relevant documentation when:

- Adding new API endpoints
- Changing database schema
- Adding new features
- Modifying security measures

## Bug Reports

Include:

- Clear title and description
- Steps to reproduce
- Expected vs actual behavior
- Screenshots or logs if applicable
- Environment details

## Feature Requests

Include:

- Use case and motivation
- Proposed solution
- Alternatives considered
- Impact on existing features

## Development Workflow

### Branch Naming

```text
feature/new-feature
fix/bug-description
docs/documentation-update
refactor/module-name
```

### Pull Request Checklist

Before submitting a Pull Request:

- Code builds successfully
- Tests pass
- Documentation updated if required
- No sensitive information included
- Code follows project standards
- Commit messages follow Conventional Commits

## Project Structure

```text
hms-sprint5-deploy/
├── backend/
├── web-frontend/
├── mobile-app/
├── ARCHITECTURE.md
├── API_DOCUMENTATION.md
├── CONTRIBUTING.md
└── README.md
```

## Documentation References

- ./README.md
- Architecture Overview
- ./API_DOCUMENTATION.md
- ./backend/README.md
- ./backend/DATABASE_SCHEMA.md
- ./web-frontend/README.md
- ./mobile-app/README.md

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

## Questions?

Feel free to open an issue or contact the maintainers.

**Happy Contributing! ❤️**