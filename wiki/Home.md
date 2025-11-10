# Terraform Provider Cisco Catalyst Center Wiki

Welcome to the Terraform Provider for Cisco Catalyst Center wiki! This wiki provides comprehensive documentation for using and contributing to the Terraform provider.

[![Tests](https://github.com/CiscoDevNet/terraform-provider-catalystcenter/actions/workflows/test.yml/badge.svg)](https://github.com/CiscoDevNet/terraform-provider-catalystcenter/actions/workflows/test.yml)

## Overview

The Catalyst Center provider enables infrastructure as code for Cisco Catalyst Center. It provides resources to interact with a Cisco Catalyst Center instance via the REST API.

All resources and data sources have been tested with the following releases:

| Platform        | Version |
| --------------- | ------- |
| Catalyst Center | 2.3.7.9 |

## Documentation

- **[Getting Started](Getting-Started.md)** - Learn how to install and configure the provider
- **[Resources and Data Sources](Resources-and-Data-Sources.md)** - Complete reference of available resources
- **[Contributing](Contributing.md)** - Guidelines for contributing to the project
- **[Development](Development.md)** - Information for developers working on the provider
- **[Security Policy](Security-Policy.md)** - Security procedures and bug reporting
- **[Code of Conduct](Code-of-Conduct.md)** - Community guidelines and standards
- **[Troubleshooting](Troubleshooting.md)** - Common issues and solutions
- **[FAQ](FAQ.md)** - Frequently asked questions

## Quick Links

- [Official Documentation](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs)
- [GitHub Repository](https://github.com/aiseyes/terraform-provider-catalystcenter)
- [Issue Tracker](https://github.com/aiseyes/terraform-provider-catalystcenter/issues)
- [Changelog](https://github.com/aiseyes/terraform-provider-catalystcenter/blob/main/CHANGELOG.md)

## Requirements

- [Terraform](https://www.terraform.io/downloads.html) >= 1.0
- [Go](https://golang.org/doc/install) >= 1.23
- Cisco Catalyst Center >= 2.3.7.9

## Quick Start

```hcl
terraform {
  required_providers {
    catalystcenter = {
      source = "CiscoDevNet/catalystcenter"
    }
  }
}

provider "catalystcenter" {
  username = "admin"
  password = "password"
  url      = "https://10.1.1.1"
}
```

## Support

For support, please:
1. Check the [Troubleshooting](Troubleshooting.md) guide
2. Review [FAQ](FAQ.md) for common questions
3. Search existing [GitHub Issues](https://github.com/aiseyes/terraform-provider-catalystcenter/issues)
4. Open a new issue if your problem is not already reported

## License

This project is licensed under the Mozilla Public License 2.0 (MPL-2.0). See the [LICENSE](https://github.com/aiseyes/terraform-provider-catalystcenter/blob/main/LICENSE) file for details.
