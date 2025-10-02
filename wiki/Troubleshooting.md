# Troubleshooting

This guide helps you troubleshoot common issues when using the Terraform Provider for Cisco Catalyst Center.

## Table of Contents

- [Provider Installation Issues](#provider-installation-issues)
- [Authentication Issues](#authentication-issues)
- [Connection Issues](#connection-issues)
- [Resource Creation Issues](#resource-creation-issues)
- [State Management Issues](#state-management-issues)
- [Performance Issues](#performance-issues)
- [Debugging](#debugging)
- [Common Error Messages](#common-error-messages)

## Provider Installation Issues

### Provider Not Found

**Error:**
```
Error: Failed to query available provider packages
Could not retrieve the list of available versions for provider
```

**Solutions:**

1. **Check provider source:**
   ```hcl
   terraform {
     required_providers {
       catalystcenter = {
         source = "CiscoDevNet/catalystcenter"
       }
     }
   }
   ```

2. **Run terraform init:**
   ```bash
   terraform init
   ```

3. **Clear cache and reinitialize:**
   ```bash
   rm -rf .terraform .terraform.lock.hcl
   terraform init
   ```

### Version Constraint Issues

**Error:**
```
Error: Failed to install provider
```

**Solution:**
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

## Authentication Issues

### Invalid Credentials

**Error:**
```
Error: authentication failed: invalid username or password
```

**Solutions:**

1. **Verify credentials:**
   ```bash
   curl -k -X POST "https://$CC_URL/dna/system/api/v1/auth/token" \
     -u "$CC_USERNAME:$CC_PASSWORD"
   ```

2. **Check environment variables:**
   ```bash
   echo $CC_USERNAME
   echo $CC_URL
   # Don't echo password in production!
   ```

3. **Use correct provider configuration:**
   ```hcl
   provider "catalystcenter" {
     username = var.username
     password = var.password
     url      = var.url
   }
   ```

### Permission Issues

**Error:**
```
Error: insufficient permissions
```

**Solutions:**

1. Ensure the user has appropriate RBAC roles in Catalyst Center
2. Check that the user can perform the operation via the Catalyst Center UI
3. Verify API permissions in Catalyst Center

## Connection Issues

### Cannot Connect to Catalyst Center

**Error:**
```
Error: connection timeout
Error: connection refused
```

**Solutions:**

1. **Verify network connectivity:**
   ```bash
   ping <catalyst-center-ip>
   telnet <catalyst-center-ip> 443
   ```

2. **Check URL format:**
   ```hcl
   provider "catalystcenter" {
     url = "https://10.1.1.1"  # Include https://
   }
   ```

3. **Test with curl:**
   ```bash
   curl -k https://10.1.1.1
   ```

### SSL/TLS Certificate Issues

**Error:**
```
Error: x509: certificate signed by unknown authority
```

**Solutions:**

1. **For development/testing (not production):**
   ```hcl
   provider "catalystcenter" {
     url      = "https://10.1.1.1"
     insecure = true
   }
   ```

2. **For production - add CA certificate:**
   ```bash
   # Add Catalyst Center CA to system trust store
   ```

3. **Use proper certificate:**
   - Install valid SSL certificate on Catalyst Center
   - Ensure certificate matches hostname/IP

### Firewall/Proxy Issues

**Solutions:**

1. **Check firewall rules** - Allow HTTPS (443) to Catalyst Center
2. **Configure proxy if needed:**
   ```bash
   export HTTPS_PROXY=http://proxy.example.com:8080
   ```

## Resource Creation Issues

### Resource Already Exists

**Error:**
```
Error: resource already exists
```

**Solutions:**

1. **Import existing resource:**
   ```bash
   terraform import catalystcenter_area.example "Global/San Jose"
   ```

2. **Use experimental flag (use with caution):**
   ```hcl
   provider "catalystcenter" {
     allow_existing_on_create = true  # Experimental!
   }
   ```

3. **Remove from Catalyst Center** before creating with Terraform

### Dependency Issues

**Error:**
```
Error: parent site not found
Error: resource depends on non-existent resource
```

**Solutions:**

1. **Ensure dependencies are created first:**
   ```hcl
   resource "catalystcenter_area" "parent" {
     name = "Parent"
   }
   
   resource "catalystcenter_building" "child" {
     parent_name = "Global/${catalystcenter_area.parent.name}"
   }
   ```

2. **Use explicit depends_on:**
   ```hcl
   resource "catalystcenter_building" "example" {
     depends_on = [catalystcenter_area.parent]
   }
   ```

### Validation Errors

**Error:**
```
Error: invalid configuration: value does not match expected format
```

**Solutions:**

1. **Check attribute types and formats**
2. **Review documentation** for valid values
3. **Use proper data types:**
   ```hcl
   resource "catalystcenter_ip_pool" "example" {
     timeout_seconds = 20  # Number, not "20"
   }
   ```

### Timeout Issues

**Error:**
```
Error: timeout waiting for resource to be ready
```

**Solutions:**

1. **Increase timeout:**
   ```hcl
   provider "catalystcenter" {
     max_timeout = 300  # 5 minutes
   }
   ```

2. **Check task status in Catalyst Center UI**
3. **Verify Catalyst Center is not overloaded**

## State Management Issues

### State Drift

**Issue:** Terraform state doesn't match actual configuration

**Solutions:**

1. **Refresh state:**
   ```bash
   terraform refresh
   ```

2. **Detect drift:**
   ```bash
   terraform plan
   ```

3. **Fix drift:**
   - Update Terraform configuration to match reality, OR
   - Apply Terraform configuration to fix Catalyst Center

### State Lock Issues

**Error:**
```
Error: Error acquiring the state lock
```

**Solutions:**

1. **Wait for other process to complete**
2. **Force unlock (use with caution):**
   ```bash
   terraform force-unlock <lock-id>
   ```

### Corrupted State

**Solutions:**

1. **Use version control** - rollback to previous state
2. **Use remote state** - restore from backup
3. **Manually fix** state file (advanced)

## Performance Issues

### Slow Plan/Apply

**Solutions:**

1. **Use targeted operations:**
   ```bash
   terraform apply -target=catalystcenter_area.example
   ```

2. **Parallelize when safe:**
   ```bash
   terraform apply -parallelism=10
   ```

3. **Break into smaller configurations**

4. **Use modules** for better organization

### API Rate Limiting

**Error:**
```
Error: too many requests
Error: rate limit exceeded
```

**Solutions:**

1. **Reduce parallelism:**
   ```bash
   terraform apply -parallelism=1
   ```

2. **Add delays between operations**
3. **Contact Cisco support** for rate limit adjustments

## Debugging

### Enable Debug Logging

Set environment variable for verbose output:

```bash
export TF_LOG=DEBUG
terraform plan
```

Log levels:
- `TRACE` - Most verbose
- `DEBUG` - Debug messages
- `INFO` - Informational messages
- `WARN` - Warning messages
- `ERROR` - Error messages only

### Save Logs to File

```bash
export TF_LOG=DEBUG
export TF_LOG_PATH=terraform.log
terraform apply
```

### Provider Debug Mode

```bash
export TF_LOG_PROVIDER=DEBUG
terraform apply
```

### Inspect State

```bash
# Show all resources
terraform state list

# Show specific resource
terraform state show catalystcenter_area.example

# Export state to JSON
terraform show -json > state.json
```

### Test API Directly

```bash
# Get auth token
TOKEN=$(curl -k -X POST "https://$CC_URL/dna/system/api/v1/auth/token" \
  -u "$CC_USERNAME:$CC_PASSWORD" | jq -r .Token)

# Make API call
curl -k -X GET "https://$CC_URL/dna/intent/api/v1/site" \
  -H "X-Auth-Token: $TOKEN"
```

## Common Error Messages

### "Error: Provider produced inconsistent result after apply"

**Cause:** API returns different data than expected

**Solutions:**
1. Check Catalyst Center version compatibility
2. Report issue on GitHub with debug logs
3. Try refreshing state

### "Error: context deadline exceeded"

**Cause:** Operation timeout

**Solutions:**
1. Increase `max_timeout` in provider config
2. Check Catalyst Center performance
3. Verify network connectivity

### "Error: EOF"

**Cause:** Connection terminated unexpectedly

**Solutions:**
1. Check network stability
2. Verify Catalyst Center is responding
3. Review firewall/proxy logs

### "Error: invalid character '<' looking for beginning of value"

**Cause:** Received HTML instead of JSON (often login page)

**Solutions:**
1. Verify credentials
2. Check authentication token
3. Confirm API endpoint URL

## Getting Help

If you're still experiencing issues:

1. **Search existing issues:**
   - [GitHub Issues](https://github.com/aiseyes/terraform-provider-catalystcenter/issues)

2. **Check documentation:**
   - [Provider Documentation](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs)
   - [FAQ](FAQ.md)

3. **Gather information:**
   - Terraform version: `terraform version`
   - Provider version: Check `.terraform.lock.hcl`
   - Catalyst Center version
   - Error messages and debug logs
   - Configuration (sanitize sensitive data)

4. **Open an issue:**
   - Include all gathered information
   - Provide steps to reproduce
   - Share relevant configuration
   - Attach debug logs (sanitized)

5. **Community support:**
   - GitHub Discussions
   - Terraform Community Forum

## Prevention Tips

1. **Always run `terraform plan`** before `apply`
2. **Use version control** for all configurations
3. **Enable remote state** with locking
4. **Test in dev/staging** before production
5. **Keep provider updated** to latest version
6. **Document custom configurations**
7. **Monitor Catalyst Center health**
8. **Regular backups** of state files
9. **Use CI/CD pipelines** for consistency
10. **Review logs regularly**

## Additional Resources

- [Terraform Debugging Guide](https://www.terraform.io/docs/internals/debugging.html)
- [Catalyst Center API Documentation](https://developer.cisco.com/docs/dna-center/)
- [Provider GitHub Issues](https://github.com/aiseyes/terraform-provider-catalystcenter/issues)
- [Getting Started Guide](Getting-Started.md)
- [FAQ](FAQ.md)
