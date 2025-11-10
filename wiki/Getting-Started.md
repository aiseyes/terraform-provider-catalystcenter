# Getting Started

This guide will help you get started with the Terraform Provider for Cisco Catalyst Center.

## Prerequisites

Before you begin, ensure you have:

- [Terraform](https://www.terraform.io/downloads.html) >= 1.0 installed
- Access to a Cisco Catalyst Center instance (>= 2.3.7.9)
- Valid credentials (username and password)
- Network connectivity to your Catalyst Center instance

## Installation

The Catalyst Center provider is available on the [Terraform Registry](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest) and can be installed automatically via `terraform init`.

### Configure the Provider

Create a `main.tf` file with the following provider configuration:

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

### Using Environment Variables

For better security, you can use environment variables instead of hardcoding credentials:

```hcl
provider "catalystcenter" {
  # Configuration can be provided via environment variables:
  # CC_USERNAME, CC_PASSWORD, CC_URL
}
```

Set the environment variables:

```bash
export CC_USERNAME="admin"
export CC_PASSWORD="password"
export CC_URL="https://10.1.1.1"
```

## Provider Configuration Options

The provider supports the following configuration options:

| Parameter | Description | Environment Variable | Default |
|-----------|-------------|---------------------|---------|
| `username` | Username for Catalyst Center | `CC_USERNAME` | - |
| `password` | Password for Catalyst Center | `CC_PASSWORD` | - |
| `url` | URL of Catalyst Center instance | `CC_URL` | - |
| `insecure` | Allow insecure HTTPS client | `CC_INSECURE` | `true` |
| `retries` | Number of retries for REST API calls | `CC_RETRIES` | `3` |
| `max_timeout` | Timeout in seconds for asynchronous tasks | `CC_MAX_TIMEOUT` | `180` |
| `allow_existing_on_create` | **Experimental** - Allow managing existing objects | `CC_ALLOW_EXISTING_ON_CREATE` | `false` |

## First Example: Creating Sites

This example demonstrates how to create a hierarchical site structure:

```hcl
# Create an area
resource "catalystcenter_area" "san_jose" {
  name        = "San Jose"
  parent_name = "Global"
}

# Create a building within the area
resource "catalystcenter_building" "main_building" {
  name        = "Main Building"
  parent_name = "Global/${catalystcenter_area.san_jose.name}"
  latitude    = 37.338
  longitude   = -121.832
}

# Create a floor within the building
resource "catalystcenter_floor" "ground_floor" {
  name        = "Ground Floor"
  parent_name = "${catalystcenter_building.main_building.parent_name}/${catalystcenter_building.main_building.name}"
  rf_model    = "Drywall Office Only"
  width       = 30.5
  length      = 50.5
  height      = 3.5
}
```

## Initialize and Apply

1. Initialize Terraform:
   ```bash
   terraform init
   ```

2. Review the execution plan:
   ```bash
   terraform plan
   ```

3. Apply the configuration:
   ```bash
   terraform apply
   ```

## IP Pool Management Example

Create and manage IP pools for your sites:

```hcl
# Create a global IP pool
resource "catalystcenter_ip_pool" "mgmt_pool_1" {
  name             = "MgmtPool1"
  ip_address_space = "IPv4"
  type             = "Generic"
  ip_subnet        = "10.100.0.0/16"
  dhcp_server_ips  = ["10.101.1.1"]
  dns_server_ips   = ["10.101.1.101", "10.101.1.102"]
}

# Create a reservation for a specific site
resource "catalystcenter_ip_pool_reservation" "san_jose_main_building_mgmt_1" {
  site_id            = catalystcenter_building.main_building.id
  name               = "SanJoseMainBuildingMgmt1"
  type               = "Generic"
  ipv6_address_space = false
  ipv4_global_pool   = catalystcenter_ip_pool.mgmt_pool_1.ip_subnet
  ipv4_prefix        = true
  ipv4_prefix_length = 24
  ipv4_subnet        = "10.100.0.0"
  ipv4_gateway       = "10.100.0.1"
}
```

## Additional Examples

For more comprehensive examples, check out:

- [Image Management Guide](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs/guides/image_management) - Learn how to upload, distribute, and activate software images
- [Example Configurations](https://github.com/aiseyes/terraform-provider-catalystcenter/tree/main/examples) - Browse complete example configurations

## Next Steps

- Explore [Resources and Data Sources](Resources-and-Data-Sources.md) to see all available resources
- Review the [Troubleshooting](Troubleshooting.md) guide for common issues
- Check out the [FAQ](FAQ.md) for frequently asked questions

## Useful Resources

- [Official Provider Documentation](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs)
- [Terraform Best Practices](https://www.terraform.io/docs/cloud/guides/recommended-practices/index.html)
- [Catalyst Center API Documentation](https://developer.cisco.com/docs/dna-center/)
