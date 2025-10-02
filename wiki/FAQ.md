# Frequently Asked Questions (FAQ)

Common questions and answers about the Terraform Provider for Cisco Catalyst Center.

## Table of Contents

- [General Questions](#general-questions)
- [Installation and Setup](#installation-and-setup)
- [Authentication and Security](#authentication-and-security)
- [Resources and Configuration](#resources-and-configuration)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Contributing](#contributing)

## General Questions

### What is the Terraform Provider for Cisco Catalyst Center?

The Terraform Provider for Cisco Catalyst Center enables Infrastructure as Code (IaC) for Cisco Catalyst Center (formerly DNA Center). It allows you to manage Catalyst Center configurations using Terraform's declarative syntax.

### What versions of Catalyst Center are supported?

The provider is tested with:
- Catalyst Center 2.3.7.9 and above

Check the [README](https://github.com/aiseyes/terraform-provider-catalystcenter) for the most current compatibility information.

### What Terraform versions are supported?

- Terraform >= 1.0
- Terraform >= 0.13 may work but is not officially supported

### Where can I find the official documentation?

- [Terraform Registry Documentation](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs)
- [GitHub Repository](https://github.com/aiseyes/terraform-provider-catalystcenter)
- [Wiki](Home.md)

### Is this provider officially supported by Cisco?

Yes, this provider is maintained by Cisco DevNet.

## Installation and Setup

### How do I install the provider?

The provider is available on the Terraform Registry and installs automatically:

```hcl
terraform {
  required_providers {
    catalystcenter = {
      source = "CiscoDevNet/catalystcenter"
    }
  }
}
```

Then run:
```bash
terraform init
```

### Do I need to download anything manually?

No, `terraform init` automatically downloads the provider from the Terraform Registry.

### How do I upgrade to a newer provider version?

1. Update version constraint in your configuration:
   ```hcl
   terraform {
     required_providers {
       catalystcenter = {
         source  = "CiscoDevNet/catalystcenter"
         version = "~> 1.0"
       }
     }
   }
   ```

2. Run:
   ```bash
   terraform init -upgrade
   ```

### Can I use a specific provider version?

Yes:
```hcl
terraform {
  required_providers {
    catalystcenter = {
      source  = "CiscoDevNet/catalystcenter"
      version = "1.0.5"  # Specific version
    }
  }
}
```

## Authentication and Security

### How do I authenticate to Catalyst Center?

Use username and password via provider configuration or environment variables:

**Provider configuration:**
```hcl
provider "catalystcenter" {
  username = var.username
  password = var.password
  url      = var.url
}
```

**Environment variables:**
```bash
export CC_USERNAME="admin"
export CC_PASSWORD="password"
export CC_URL="https://10.1.1.1"
```

### Should I hardcode credentials in my Terraform files?

**No!** Never hardcode credentials. Use:
- Environment variables
- Terraform variables
- Secrets management solutions (Vault, AWS Secrets Manager, etc.)
- Terraform Cloud encrypted variables

### How do I handle SSL certificate issues?

For development/testing:
```hcl
provider "catalystcenter" {
  url      = "https://10.1.1.1"
  insecure = true  # Not recommended for production
}
```

For production:
- Install a valid SSL certificate on Catalyst Center
- Add the CA certificate to your system's trust store

### What permissions does my user need?

Your Catalyst Center user needs:
- Appropriate RBAC role (typically Network Admin or Super Admin)
- API access enabled
- Permissions for the resources you want to manage

## Resources and Configuration

### How many resources does the provider support?

The provider supports 50+ resources and 40+ data sources. See [Resources and Data Sources](Resources-and-Data-Sources.md) for the complete list.

### Can I import existing Catalyst Center configurations?

Yes! Many resources support import:

```bash
# Import a site
terraform import catalystcenter_area.example "Global/San Jose"

# Import an IP pool
terraform import catalystcenter_ip_pool.example "<pool-uuid>"
```

Check individual resource documentation for specific import syntax.

### What's the difference between resources and data sources?

- **Resources** - Create, update, and delete objects (e.g., `resource "catalystcenter_area"`)
- **Data sources** - Read/query existing objects (e.g., `data "catalystcenter_sites"`)

### How do I reference one resource in another?

Use Terraform interpolation:

```hcl
resource "catalystcenter_area" "region" {
  name        = "San Jose"
  parent_name = "Global"
}

resource "catalystcenter_building" "hq" {
  name        = "HQ"
  parent_name = "Global/${catalystcenter_area.region.name}"
}
```

### Can I use Terraform modules with this provider?

Yes! Create reusable modules:

```hcl
module "site_hierarchy" {
  source = "./modules/sites"
  
  area_name     = "San Jose"
  building_name = "Main Building"
  latitude      = 37.338
  longitude     = -121.832
}
```

### How do I manage multiple Catalyst Center instances?

Use provider aliases:

```hcl
provider "catalystcenter" {
  alias    = "prod"
  url      = "https://prod.example.com"
  username = var.prod_username
  password = var.prod_password
}

provider "catalystcenter" {
  alias    = "dev"
  url      = "https://dev.example.com"
  username = var.dev_username
  password = var.dev_password
}

resource "catalystcenter_area" "prod_area" {
  provider = catalystcenter.prod
  name     = "Production"
}
```

## Best Practices

### Should I commit my state file to Git?

**No!** State files may contain sensitive data. Instead:
- Use remote state (Terraform Cloud, S3, etc.)
- Add `*.tfstate*` to `.gitignore`
- Enable state locking

### How should I organize my Terraform files?

Common structure:
```
project/
├── main.tf           # Main configuration
├── variables.tf      # Input variables
├── outputs.tf        # Output values
├── terraform.tfvars  # Variable values (don't commit if sensitive)
├── providers.tf      # Provider configuration
└── modules/          # Reusable modules
```

### How do I handle secrets in Terraform?

Use:
- Environment variables
- Terraform Cloud encrypted variables
- HashiCorp Vault
- Cloud provider secrets services (AWS Secrets Manager, Azure Key Vault)
- `.tfvars` files (excluded from version control)

### Should I use workspaces?

Workspaces are useful for:
- Managing multiple environments (dev, staging, prod)
- Isolating state for testing
- Team collaboration

### How often should I run `terraform plan`?

Always run `terraform plan` before `terraform apply` to review changes.

## Troubleshooting

### Why does my apply fail with "resource already exists"?

The resource may already exist in Catalyst Center. Options:
1. Import the existing resource
2. Remove from Catalyst Center and let Terraform create it
3. Use the experimental `allow_existing_on_create` flag (not recommended for production)

### Why is Terraform timing out?

Increase the timeout:
```hcl
provider "catalystcenter" {
  max_timeout = 300  # 5 minutes
}
```

Also check:
- Catalyst Center performance
- Network connectivity
- API rate limits

### How do I debug provider issues?

Enable debug logging:
```bash
export TF_LOG=DEBUG
terraform apply
```

Save logs to file:
```bash
export TF_LOG=DEBUG
export TF_LOG_PATH=terraform.log
terraform apply
```

### What do I do if I encounter a bug?

1. Search [existing issues](https://github.com/aiseyes/terraform-provider-catalystcenter/issues)
2. If not found, open a new issue with:
   - Provider version
   - Terraform version
   - Catalyst Center version
   - Configuration (sanitized)
   - Error messages and logs

### Where can I get more help?

- [Troubleshooting Guide](Troubleshooting.md)
- [GitHub Issues](https://github.com/aiseyes/terraform-provider-catalystcenter/issues)
- [Official Documentation](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs)

## Contributing

### How can I contribute?

See the [Contributing Guide](Contributing.md) for:
- Reporting bugs
- Suggesting features
- Submitting pull requests
- Improving documentation

### Can I add new resources?

Yes! Contributions are welcome. See [Development Guide](Development.md) for:
- Setting up development environment
- Adding new resources
- Writing tests
- Submitting pull requests

### How do I report a security issue?

**Never report security issues via GitHub Issues.**

Email security issues to: **oss-security@cisco.com**

See [Security Policy](Security-Policy.md) for details.

### How long does it take for pull requests to be reviewed?

Maintainers typically review within 10 days. Complex changes may take longer.

## Performance

### Why is `terraform plan` slow?

Several reasons:
- Large number of resources
- Slow API responses
- State refresh taking time

Solutions:
- Use targeted plans: `terraform plan -target=resource.name`
- Break into smaller configurations
- Use modules

### Can I run Terraform in parallel?

Yes, Terraform automatically parallelizes operations. Adjust with:
```bash
terraform apply -parallelism=10
```

### Is there a rate limit on the API?

Yes, Catalyst Center has API rate limits. If you hit limits:
- Reduce parallelism
- Add delays between operations
- Contact Cisco support for adjustments

## Advanced Topics

### Can I use this provider in CI/CD pipelines?

Yes! The provider works well in CI/CD:
- GitHub Actions
- GitLab CI
- Jenkins
- Azure DevOps
- CircleCI

Use environment variables for credentials and remote state for team collaboration.

### Can I use Terraform Cloud with this provider?

Yes, the provider is compatible with Terraform Cloud. Configure sensitive variables in Terraform Cloud workspace settings.

### How do I handle drift between Terraform and Catalyst Center?

1. Run `terraform plan` to detect drift
2. Either:
   - Update Terraform config to match reality, or
   - Run `terraform apply` to fix Catalyst Center

### Can I use count or for_each with resources?

Yes:

```hcl
# Using count
resource "catalystcenter_area" "regions" {
  count       = length(var.regions)
  name        = var.regions[count.index]
  parent_name = "Global"
}

# Using for_each
resource "catalystcenter_building" "sites" {
  for_each = var.buildings
  
  name        = each.value.name
  parent_name = each.value.parent
  latitude    = each.value.lat
  longitude   = each.value.lon
}
```

## Miscellaneous

### What's the difference between Catalyst Center and DNA Center?

Cisco DNA Center was rebranded as Cisco Catalyst Center. This provider supports both (they use the same API).

### Does the provider support GraphQL?

The provider uses the REST API. GraphQL is not currently supported.

### Can I contribute to the documentation?

Yes! Documentation improvements are always welcome. See [Contributing Guide](Contributing.md).

### Where can I find example configurations?

- [GitHub Examples Directory](https://github.com/aiseyes/terraform-provider-catalystcenter/tree/main/examples)
- [Getting Started Guide](Getting-Started.md)
- [Provider Documentation](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs)

### Is there a community forum or chat?

Check the GitHub repository for:
- GitHub Discussions
- Issue tracker for questions
- Links to community resources

---

**Have a question not answered here?**

- Check the [Troubleshooting Guide](Troubleshooting.md)
- Search [GitHub Issues](https://github.com/aiseyes/terraform-provider-catalystcenter/issues)
- Open a new issue with your question
