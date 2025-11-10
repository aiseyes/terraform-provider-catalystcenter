# Resources and Data Sources

This page provides an overview of all available resources and data sources in the Terraform Provider for Cisco Catalyst Center.

## Overview

The provider includes resources for managing Catalyst Center configurations and data sources for querying existing configurations.

- **Resources** - Create, update, and delete Catalyst Center objects
- **Data Sources** - Query and retrieve information about existing objects

## Complete Documentation

For detailed documentation including all attributes, examples, and import syntax, visit the [Official Provider Documentation](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs).

## Resource Categories

Resources are organized into the following categories:

### Sites and Hierarchy

Manage the site hierarchy and locations:

- **catalystcenter_area** - Logical groupings of sites
- **catalystcenter_building** - Physical buildings with geolocation
- **catalystcenter_floor** - Individual floors within buildings

Example:
```hcl
resource "catalystcenter_area" "example" {
  name        = "San Jose"
  parent_name = "Global"
}
```

### Network Settings

Configure network-wide settings:

- **catalystcenter_aaa_settings** - AAA server settings
- **catalystcenter_banner_settings** - Login banner configuration
- **catalystcenter_dhcp_settings** - DHCP server settings
- **catalystcenter_dns_settings** - DNS server settings
- **catalystcenter_ntp_settings** - NTP server settings
- **catalystcenter_snmp_settings** - SNMP configuration
- **catalystcenter_telemetry_settings** - Telemetry settings
- **catalystcenter_timezone_settings** - Timezone configuration

### IP Address Management

Manage IP pools and reservations:

- **catalystcenter_ip_pool** - Global IP address pools
- **catalystcenter_ip_pool_reservation** - Site-specific IP pool reservations

Example:
```hcl
resource "catalystcenter_ip_pool" "example" {
  name             = "MgmtPool"
  ip_address_space = "IPv4"
  type             = "Generic"
  ip_subnet        = "10.0.0.0/16"
}
```

### Credentials

Manage device credentials:

- **catalystcenter_credentials_cli** - CLI credentials
- **catalystcenter_credentials_https_read** - HTTPS read credentials
- **catalystcenter_credentials_https_write** - HTTPS write credentials
- **catalystcenter_credentials_snmpv2_read** - SNMPv2 read credentials
- **catalystcenter_credentials_snmpv2_write** - SNMPv2 write credentials
- **catalystcenter_credentials_snmpv3** - SNMPv3 credentials
- **catalystcenter_assign_credentials** - Assign credentials to sites

### Wireless

Configure wireless settings:

- **catalystcenter_wireless_profile** - Wireless network profiles
- **catalystcenter_wireless_ssid** - SSID configurations
- **catalystcenter_wireless_rf_profile** - RF profiles
- **catalystcenter_assign_managed_ap_locations** - Assign AP locations

### Fabric (SD-Access)

Manage Software-Defined Access fabric:

- **catalystcenter_fabric_site** - Enable fabric at sites
- **catalystcenter_fabric_l2_handoff** - Layer 2 handoffs
- **catalystcenter_fabric_l3_handoff_ip_transit** - Layer 3 IP transit
- **catalystcenter_fabric_l3_handoff_sda_transit** - Layer 3 SDA transit
- **catalystcenter_fabric_l3_virtual_network** - Layer 3 virtual networks
- **catalystcenter_fabric_virtual_network** - Virtual network configurations
- **catalystcenter_anycast_gateway** - Anycast gateway configurations

### Device Management

Manage network devices:

- **catalystcenter_assign_device_to_site** - Assign devices to sites
- **catalystcenter_device_configuration_backup** - Backup configurations
- **catalystcenter_pnp_device** - Plug and Play device provisioning
- **catalystcenter_pnp_device_claim** - Claim PnP devices

### Image Management

Manage software images:

- **catalystcenter_image** - Upload software images
- **catalystcenter_image_activation** - Activate images on devices
- **catalystcenter_image_distribution** - Distribute images to devices

Example:
```hcl
resource "catalystcenter_image" "example" {
  name        = "cat9k_iosxe.17.12.02.SPA.bin"
  source_path = "/path/to/image.bin"
}
```

### Templates and Configuration

Manage configuration templates:

- **catalystcenter_project** - Template projects
- **catalystcenter_template** - Configuration templates
- **catalystcenter_template_deployment** - Deploy templates
- **catalystcenter_assign_templates_to_tag** - Assign templates to tags

### Tags

Manage device and template tags:

- **catalystcenter_tag** - Create tags
- **catalystcenter_assign_devices_to_tag** - Tag devices
- **catalystcenter_assign_templates_to_tag** - Tag templates

### Network Profiles

Manage network profiles:

- **catalystcenter_network_profile** - Network profile configurations
- **catalystcenter_associate_site_to_network_profile** - Associate profiles with sites

### Authentication

Configure authentication servers:

- **catalystcenter_authentication_policy_server** - AAA/ISE servers

## Data Sources

Data sources allow you to query existing Catalyst Center objects.

### Site Data Sources

- **catalystcenter_area** - Query area information
- **catalystcenter_building** - Query building information
- **catalystcenter_floor** - Query floor information
- **catalystcenter_sites** - Query multiple sites

### Network Settings Data Sources

- **catalystcenter_aaa_settings** - Query AAA settings
- **catalystcenter_banner_settings** - Query banner settings
- **catalystcenter_dhcp_settings** - Query DHCP settings
- **catalystcenter_dns_settings** - Query DNS settings
- **catalystcenter_ntp_settings** - Query NTP settings
- **catalystcenter_snmp_settings** - Query SNMP settings
- **catalystcenter_telemetry_settings** - Query telemetry settings
- **catalystcenter_timezone_settings** - Query timezone settings

### IP Address Management Data Sources

- **catalystcenter_ip_pool** - Query IP pool details
- **catalystcenter_ip_pools** - Query multiple IP pools

### Device Data Sources

- **catalystcenter_network_device** - Query single device
- **catalystcenter_network_devices** - Query multiple devices
- **catalystcenter_device_detail** - Query detailed device information
- **catalystcenter_pnp_device** - Query PnP device status

### Wireless Data Sources

- **catalystcenter_wireless_profile** - Query wireless profiles
- **catalystcenter_wireless_ssid** - Query SSID configurations
- **catalystcenter_wireless_rf_profile** - Query RF profiles

### Fabric Data Sources

- **catalystcenter_fabric_site** - Query fabric site information
- **catalystcenter_fabric_sites** - Query multiple fabric sites
- **catalystcenter_fabric_l2_handoff** - Query L2 handoffs
- **catalystcenter_fabric_l3_handoff_ip_transit** - Query L3 IP transit
- **catalystcenter_fabric_l3_handoff_sda_transit** - Query L3 SDA transit
- **catalystcenter_fabric_l3_virtual_network** - Query L3 virtual networks
- **catalystcenter_fabric_virtual_network** - Query virtual networks
- **catalystcenter_anycast_gateway** - Query anycast gateways
- **catalystcenter_anycast_gateways** - Query multiple gateways

### Template and Configuration Data Sources

- **catalystcenter_project** - Query project details
- **catalystcenter_projects** - Query multiple projects
- **catalystcenter_template** - Query template details

## Usage Examples

### Using Resources

Create a new site hierarchy:

```hcl
resource "catalystcenter_area" "region" {
  name        = "North America"
  parent_name = "Global"
}

resource "catalystcenter_building" "hq" {
  name        = "Headquarters"
  parent_name = "Global/${catalystcenter_area.region.name}"
  latitude    = 37.338
  longitude   = -121.832
}
```

### Using Data Sources

Query existing sites:

```hcl
data "catalystcenter_sites" "all" {
}

output "site_count" {
  value = length(data.catalystcenter_sites.all.sites)
}
```

Query specific device:

```hcl
data "catalystcenter_network_device" "switch" {
  management_ip_address = "10.1.1.100"
}

output "device_hostname" {
  value = data.catalystcenter_network_device.switch.hostname
}
```

## Import Support

Many resources support importing existing Catalyst Center objects into Terraform state.

Example import:

```bash
terraform import catalystcenter_area.example "Global/San Jose"
terraform import catalystcenter_ip_pool.example "pool-uuid-here"
```

Refer to individual resource documentation for specific import syntax.

## Resource Relationships

Many resources have dependencies on others:

```hcl
# Building depends on Area
resource "catalystcenter_building" "example" {
  parent_name = "Global/${catalystcenter_area.example.name}"
}

# Floor depends on Building
resource "catalystcenter_floor" "example" {
  parent_name = "${catalystcenter_building.example.parent_name}/${catalystcenter_building.example.name}"
}

# IP Pool Reservation depends on Site and Pool
resource "catalystcenter_ip_pool_reservation" "example" {
  site_id          = catalystcenter_building.example.id
  ipv4_global_pool = catalystcenter_ip_pool.example.ip_subnet
}
```

Terraform automatically handles these dependencies and creates resources in the correct order.

## Common Attributes

Many resources share common attributes:

### Identification
- `id` - Unique identifier (Read-only)
- `name` - Human-readable name

### Site Hierarchy
- `parent_name` - Parent site path
- `site_id` - Site identifier

### API Behavior
- Automatic retries on temporary failures
- Asynchronous task handling
- Validation before API calls

## Best Practices

1. **Use data sources** to reference existing objects
2. **Leverage dependencies** for correct ordering
3. **Use variables** for reusable configurations
4. **Tag resources** for organization
5. **Import existing** objects when adopting Terraform
6. **Version control** your configurations
7. **Use remote state** for team collaboration
8. **Test changes** with `terraform plan`

## Additional Resources

- [Official Documentation](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs)
- [Example Configurations](https://github.com/aiseyes/terraform-provider-catalystcenter/tree/main/examples)
- [Getting Started Guide](Getting-Started.md)
- [Troubleshooting](Troubleshooting.md)

## Feedback

Found an issue or have a suggestion? Please [open an issue](https://github.com/aiseyes/terraform-provider-catalystcenter/issues) on GitHub.
