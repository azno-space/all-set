# Contributing to AllSet

First off, thank you for considering contributing to AllSet! 🎉

## How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check existing issues to avoid duplicates. When creating a bug report, include:

- **Clear title and description**
- **Steps to reproduce**
- **Expected vs actual behavior**
- **Environment details** (.NET version, OS, PostgreSQL version)
- **Error messages or logs**

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues. When creating an enhancement suggestion:

- **Use a clear and descriptive title**
- **Provide detailed description** of the suggested enhancement
- **Explain why this would be useful** to most users
- **List examples** of how it would work

### Pull Requests

1. Fork the repository
2. Create a new branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Write or update tests if applicable
5. Update documentation (README, XML comments, etc.)
6. Commit your changes (`git commit -m 'Add amazing feature'`)
7. Push to your branch (`git push origin feature/amazing-feature`)
8. Open a Pull Request

#### Pull Request Guidelines

- Follow existing code style and conventions
- Keep commits atomic and well-described
- Update README.md if you change functionality
- Add XML documentation comments for public APIs
- Test your changes thoroughly
- Reference related issues in PR description

### Code Style

- Use C# naming conventions (PascalCase for public members, camelCase for private)
- Follow existing patterns in the codebase
- Add XML documentation for public APIs
- Keep methods focused and concise
- Use async/await properly

### Development Setup

```bash
# Clone your fork
git clone https://github.com/YOUR-USERNAME/all-set.git
cd all-set

# Add upstream remote
git remote add upstream https://github.com/azno-space/all-set.git

# Create branch
git checkout -b feature/my-feature

# Make changes and test
dotnet build
dotnet run

# Commit and push
git commit -m "Description of changes"
git push origin feature/my-feature
```

### Priority Areas for Contribution

- [ ] Unit and integration tests
- [ ] Additional database providers (MySQL, SQL Server)
- [ ] Calendar integrations (Google Calendar, Outlook)
- [ ] Webhook system for notifications
- [ ] Advanced filtering and search
- [ ] Performance optimizations
- [ ] Documentation improvements
- [ ] Example implementations

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inspiring community for all.

### Our Standards

- Be respectful and inclusive
- Accept constructive criticism gracefully
- Focus on what's best for the community
- Show empathy towards others

### Unacceptable Behavior

- Harassment or discriminatory language
- Trolling or insulting comments
- Publishing others' private information
- Other unprofessional conduct

## Questions?

Feel free to open an issue with the `question` label or start a discussion in GitHub Discussions.

Thank you for contributing! 🚀
