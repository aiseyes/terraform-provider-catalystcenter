# Contributing

Thank you for your interest in contributing to the Terraform Provider for Cisco Catalyst Center! This page provides guidelines for contributing to the project.

## Code of Conduct

All interactions in this project are subject to our [Code of Conduct](Code-of-Conduct.md). Please review it before contributing.

## Ways to Contribute

There are many ways to contribute:

- **Report bugs** - Help us identify and fix issues
- **Suggest features** - Share ideas for new functionality
- **Improve documentation** - Help others understand the provider
- **Submit pull requests** - Contribute code improvements
- **Review pull requests** - Help validate proposed changes
- **Answer questions** - Support other users in issues
- **Write tests** - Improve test coverage

## Reporting Issues

Before reporting a new issue:

1. Search [existing issues](https://github.com/aiseyes/terraform-provider-catalystcenter/issues) to avoid duplicates
2. Check if the issue is already fixed in a newer version
3. Gather relevant information (version, configuration, error messages)

When creating a new issue, include:

- **Clear title and description**
- **Steps to reproduce** the issue
- **Expected behavior** vs actual behavior
- **Version information** (Terraform, provider, Catalyst Center)
- **Configuration snippets** (sanitize sensitive data)
- **Error messages and logs**

### Security Issues

**Do not report security vulnerabilities through GitHub issues.**

For security bugs, please follow the procedures in [Security Policy](Security-Policy.md) and email `oss-security@cisco.com`.

## Development Process

### Setting Up Development Environment

1. **Install prerequisites:**
   - [Go](https://golang.org/doc/install) >= 1.23
   - [Terraform](https://www.terraform.io/downloads.html) >= 1.0
   - Git

2. **Clone the repository:**
   ```bash
   git clone https://github.com/aiseyes/terraform-provider-catalystcenter.git
   cd terraform-provider-catalystcenter
   ```

3. **Build the provider:**
   ```bash
   go install
   ```

For more details, see [Development](Development.md).

## Pull Request Process

### Before Submitting

1. **Check existing PRs** - Ensure similar work isn't already in progress
2. **Create an issue** - Discuss major changes before implementing
3. **Review guidelines** - Follow coding standards and conventions
4. **Write tests** - Include tests for new functionality
5. **Update documentation** - Keep docs in sync with changes

### Submitting a Pull Request

1. **Fork the repository** and create a feature branch:
   ```bash
   git checkout -b feature/my-feature
   ```

2. **Make your changes:**
   - Write clean, readable code
   - Follow existing code style
   - Add comments for complex logic
   - Keep changes focused and minimal

3. **Test your changes:**
   ```bash
   # Build the provider
   go install
   
   # Run tests
   make test
   
   # Run acceptance tests (if applicable)
   make testacc
   ```

4. **Commit your changes:**
   ```bash
   git add .
   git commit -m "Description of changes"
   ```
   
   Use clear, descriptive commit messages.

5. **Push to your fork:**
   ```bash
   git push origin feature/my-feature
   ```

6. **Open a pull request:**
   - Provide a clear description of changes
   - Reference any related issues
   - Include test results if applicable
   - Be responsive to review feedback

### Pull Request Guidelines

- **One feature per PR** - Keep changes focused
- **Include tests** - Cover affected behavior
- **Update documentation** - Keep docs current
- **Follow semantic versioning** - Breaking changes may wait for major releases
- **Be patient** - Maintainers review within 10 days
- **Stay engaged** - PRs may be closed after 60 days of inactivity

## Coding Standards

### Go Code Style

- Follow standard Go formatting (`go fmt`)
- Use meaningful variable and function names
- Add comments for exported functions
- Handle errors appropriately
- Write unit tests for new functions

### Terraform Configuration

- Use consistent naming conventions
- Include examples for new resources
- Document all attributes
- Follow HCL style guidelines

### Documentation

- Write clear, concise documentation
- Include code examples
- Use proper markdown formatting
- Keep examples up to date

## Testing

### Unit Tests

Run unit tests with:
```bash
make test
```

### Acceptance Tests

Acceptance tests create real resources and require:

- Valid Catalyst Center credentials
- Environment variables: `CC_USERNAME`, `CC_PASSWORD`, `CC_URL`

Run acceptance tests with:
```bash
make testacc
```

**Note:** Acceptance tests create real resources and may incur costs.

### Writing Tests

- Test both success and failure cases
- Use table-driven tests when appropriate
- Mock external dependencies when possible
- Keep tests isolated and independent

## Documentation

Update documentation for any changes:

1. **Provider documentation** - `docs/` directory
2. **Resource documentation** - Auto-generated from code
3. **Guides** - `docs/guides/` directory
4. **Examples** - `examples/` directory
5. **Wiki pages** - Keep wiki updated

Generate documentation:
```bash
go generate
```

## Review Process

Maintainers will review your pull request and may:

- Request changes or clarifications
- Suggest improvements
- Ask for additional tests
- Merge the pull request

### Timeline

- Initial review: Within 10 days
- Inactive PRs closed: After 60 days
- Response time varies based on complexity

## Getting Help

If you need help:

1. Check the [FAQ](FAQ.md)
2. Review [existing issues](https://github.com/aiseyes/terraform-provider-catalystcenter/issues)
3. Ask in your pull request or issue
4. Consult the [Troubleshooting](Troubleshooting.md) guide

## Recognition

Contributors are valued members of our community! Your contributions help improve the provider for everyone.

## Additional Resources

- [Development Guide](Development.md)
- [Testing Documentation](Development.md#testing)
- [Go Code Review Comments](https://github.com/golang/go/wiki/CodeReviewComments)
- [Terraform Provider Development](https://www.terraform.io/docs/extend/index.html)

Thank you for contributing! :heart:
