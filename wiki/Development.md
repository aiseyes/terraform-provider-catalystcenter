# Development

This guide provides information for developers working on the Terraform Provider for Cisco Catalyst Center.

## Prerequisites

Before you start developing:

- [Go](https://golang.org/doc/install) >= 1.23
- [Terraform](https://www.terraform.io/downloads.html) >= 1.0
- Git
- Text editor or IDE (VS Code, GoLand, etc.)
- Access to a Catalyst Center instance for testing

## Setting Up Your Development Environment

### 1. Clone the Repository

```bash
git clone https://github.com/aiseyes/terraform-provider-catalystcenter.git
cd terraform-provider-catalystcenter
```

### 2. Install Dependencies

The provider uses Go modules for dependency management:

```bash
go mod download
```

### 3. Build the Provider

Build the provider using the Go `install` command:

```bash
go install
```

This will build the provider and place the binary in `$GOPATH/bin`.

### 4. Verify the Build

Check that the provider was built successfully:

```bash
ls -la $GOPATH/bin/terraform-provider-catalystcenter
```

## Project Structure

```
terraform-provider-catalystcenter/
├── docs/                    # Documentation files
│   ├── data-sources/       # Data source documentation
│   ├── resources/          # Resource documentation
│   ├── guides/             # User guides
│   └── index.md            # Provider documentation
├── examples/               # Example configurations
│   ├── basic/             # Basic usage examples
│   ├── data-sources/      # Data source examples
│   └── resources/         # Resource examples
├── gen/                   # Code generation utilities
│   └── definitions/       # YAML definitions for resources
├── internal/              # Internal provider code
│   └── provider/          # Provider implementation
├── templates/             # Documentation templates
├── wiki/                  # Wiki documentation
├── main.go               # Provider entry point
├── go.mod                # Go module definition
└── GNUmakefile           # Build and test targets
```

## Building the Provider

### Standard Build

```bash
go install
```

### Build for Specific Platform

```bash
GOOS=linux GOARCH=amd64 go build -o terraform-provider-catalystcenter
```

### Development Build

For development, you can use a local build:

```bash
go build -o ~/.terraform.d/plugins/local/catalystcenter/catalystcenter/1.0.0/darwin_arm64/terraform-provider-catalystcenter
```

Update the path based on your OS and architecture.

## Adding Dependencies

This provider uses [Go modules](https://github.com/golang/go/wiki/Modules) for dependency management.

To add a new dependency:

```bash
go get github.com/author/dependency
go mod tidy
```

Then commit the changes to `go.mod` and `go.sum`:

```bash
git add go.mod go.sum
git commit -m "Add dependency: github.com/author/dependency"
```

## Testing

### Unit Tests

Run unit tests:

```bash
make test
```

Or directly with Go:

```bash
go test ./... -v
```

### Acceptance Tests

Acceptance tests create real resources in Catalyst Center.

**Prerequisites:**
- Access to a Catalyst Center instance
- Valid credentials

Set environment variables:

```bash
export CC_USERNAME="admin"
export CC_PASSWORD="password"
export CC_URL="https://10.1.1.1"
```

Run acceptance tests:

```bash
make testacc
```

**Note:** Acceptance tests create real resources and may take time to complete.

### Run Specific Test

```bash
go test ./internal/provider -run TestAccDataSourceCatalystCenterArea -v
```

### Test with Coverage

```bash
go test ./... -coverprofile=coverage.out
go tool cover -html=coverage.out
```

## Code Generation

### Generate Documentation

The provider uses `tfplugindocs` to generate documentation:

```bash
go generate
```

This updates documentation in the `docs/` directory based on:
- Resource and data source schemas
- Examples in `examples/` directory
- Templates in `templates/` directory

### Generate Code from Definitions

The provider uses YAML definitions to generate resources:

```bash
go run gen/generator.go
```

### Update Documentation Categories

Update documentation categories:

```bash
go run gen/doc_category.go
```

## Developing Resources

### Adding a New Resource

1. **Create YAML definition** in `gen/definitions/`:

```yaml
name: MyResource
doc_category: Resources
rest_endpoint: /dna/intent/api/v1/my-resource
attributes:
  - model_name: name
    tf_name: name
    type: String
    required: true
```

2. **Generate code:**

```bash
go run gen/generator.go
```

3. **Add example** in `examples/resources/catalystcenter_my_resource/`:

```hcl
resource "catalystcenter_my_resource" "example" {
  name = "My Resource"
}
```

4. **Generate documentation:**

```bash
go generate
```

5. **Test the resource:**

```bash
make testacc
```

### Resource Development Best Practices

- Follow existing resource patterns
- Implement CRUD operations (Create, Read, Update, Delete)
- Handle API errors gracefully
- Add appropriate retry logic
- Include validation for required fields
- Write comprehensive tests
- Document all attributes

## Debugging

### Enable Debug Logging

Set environment variable for verbose logging:

```bash
export TF_LOG=DEBUG
terraform apply
```

### Debug Provider Code

Use VS Code or GoLand debugging features, or add debug prints:

```go
import "log"

log.Printf("[DEBUG] My debug message: %v", variable)
```

### Common Issues

1. **Provider not found:**
   - Ensure provider is built and in correct location
   - Check `~/.terraform.d/plugins/` directory
   - Run `terraform init` again

2. **Import errors:**
   - Run `go mod tidy` to clean up dependencies
   - Check for missing packages

3. **Test failures:**
   - Verify environment variables are set
   - Check Catalyst Center connectivity
   - Review test logs for details

## Code Style

### Go Formatting

Format code automatically:

```bash
go fmt ./...
```

### Linting

Run linters:

```bash
golangci-lint run
```

### Pre-commit Checks

Before committing:

```bash
go fmt ./...
go vet ./...
go test ./...
```

## Documentation

### Update Provider Documentation

1. Update resource schemas in code
2. Update examples in `examples/` directory
3. Run `go generate` to regenerate docs
4. Review generated documentation in `docs/`

### Add Guides

Create new guides in `docs/guides/`:

```markdown
---
subcategory: "Guides"
page_title: "My Guide"
description: |-
    My guide description
---

# My Guide

Guide content here...
```

## Release Process

Releases are managed by maintainers:

1. Update `CHANGELOG.md`
2. Create and push version tag
3. GitHub Actions builds and publishes release
4. Provider published to Terraform Registry

## Troubleshooting Development Issues

### Build Failures

Check Go version:
```bash
go version
```

Clean and rebuild:
```bash
go clean
go mod tidy
go install
```

### Test Failures

Check environment:
```bash
echo $CC_URL
echo $CC_USERNAME
```

Verify Catalyst Center connectivity:
```bash
curl -k https://$CC_URL
```

### Documentation Generation Issues

Ensure examples are valid:
```bash
terraform validate examples/resources/*/
```

Clean and regenerate:
```bash
rm -rf docs/
go generate
```

## Useful Commands

```bash
# Build provider
make build

# Run tests
make test

# Run acceptance tests
make testacc

# Format code
make fmt

# Lint code
make lint

# Generate documentation
make docs
```

## Resources

- [Terraform Plugin Framework](https://developer.hashicorp.com/terraform/plugin/framework)
- [Terraform Plugin Testing](https://developer.hashicorp.com/terraform/plugin/testing)
- [Go Documentation](https://golang.org/doc/)
- [Catalyst Center API Documentation](https://developer.cisco.com/docs/dna-center/)

## Getting Help

- Review [Contributing](Contributing.md) guide
- Check [FAQ](FAQ.md) for common questions
- Open an issue for bugs or questions
- Join community discussions

Happy coding! 🚀
