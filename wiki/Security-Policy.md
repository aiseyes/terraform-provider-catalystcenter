# Security Policy

This document outlines security procedures and general policies for the Terraform Provider for Cisco Catalyst Center.

## Table of Contents

- [Reporting a Security Bug](#reporting-a-security-bug)
- [Disclosure Policy](#disclosure-policy)
- [Security Best Practices](#security-best-practices)
- [Comments on this Policy](#comments-on-this-policy)

## Reporting a Security Bug

The team and community take all security bugs in this project seriously. Thank you for improving the security of this project. We appreciate your efforts and responsible disclosure and will make every effort to acknowledge your contributions.

### How to Report

**Do NOT report security vulnerabilities through public GitHub issues.**

Report security bugs by emailing: **oss-security@cisco.com**

### What to Include

When reporting a security vulnerability, please include:

- Type of vulnerability (e.g., authentication bypass, SQL injection, XSS)
- Full paths of source file(s) related to the vulnerability
- Location of the affected source code (tag/branch/commit or direct URL)
- Step-by-step instructions to reproduce the issue
- Proof-of-concept or exploit code (if possible)
- Impact of the vulnerability
- Potential remediation steps

### Response Timeline

- **48 hours**: Initial acknowledgment of your report
- **48-72 hours**: Detailed response with next steps
- **Ongoing**: Regular updates on progress towards a fix

The lead maintainer will acknowledge your email within 48 hours and will send a more detailed response within 48 hours indicating the next steps in handling your report. After the initial reply to your report, the security team will endeavor to keep you informed of the progress towards a fix and full announcement, and may ask for additional information or guidance.

## Disclosure Policy

When the security team receives a security bug report, they will assign it to a primary handler. This person will coordinate the fix and release process, involving the following steps:

1. **Confirm the problem** and determine the affected versions
2. **Audit code** to find any potential similar problems
3. **Prepare fixes** for all releases still under maintenance
4. **Release fixes** as quickly as possible to all supported versions
5. **Publish security advisory** detailing the vulnerability and fix

### Coordinated Disclosure

We follow a coordinated disclosure process:

- The reporter will be kept informed throughout the fix process
- We will coordinate the disclosure timeline with the reporter
- Public disclosure will only happen after a fix is available
- Credit will be given to the reporter (unless they prefer to remain anonymous)

## Security Best Practices

### For Users

When using this provider, follow these security best practices:

#### 1. Protect Your Credentials

**Never** hardcode credentials in your Terraform files:

```hcl
# ❌ BAD - Don't do this
provider "catalystcenter" {
  username = "admin"
  password = "MyPassword123"
  url      = "https://10.1.1.1"
}
```

**Use environment variables instead:**

```hcl
# ✅ GOOD
provider "catalystcenter" {
  # Uses CC_USERNAME, CC_PASSWORD, CC_URL environment variables
}
```

```bash
export CC_USERNAME="admin"
export CC_PASSWORD="MyPassword123"
export CC_URL="https://10.1.1.1"
```

#### 2. Use Terraform Cloud or Secure Secrets Management

For production use, consider:

- [Terraform Cloud](https://app.terraform.io/) with encrypted variables
- [HashiCorp Vault](https://www.vaultproject.io/)
- [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)
- [Azure Key Vault](https://azure.microsoft.com/en-us/services/key-vault/)
- [Google Secret Manager](https://cloud.google.com/secret-manager)

#### 3. Secure State Files

Terraform state files may contain sensitive data:

- Use remote state with encryption (S3 with encryption, Terraform Cloud)
- Never commit state files to version control
- Add `*.tfstate*` to `.gitignore`
- Use state locking to prevent concurrent modifications

#### 4. Enable TLS/SSL

Always use HTTPS for Catalyst Center connections:

```hcl
provider "catalystcenter" {
  url      = "https://10.1.1.1"  # Use HTTPS
  insecure = false                 # Verify SSL certificates
}
```

For development/testing only, you can use:

```hcl
provider "catalystcenter" {
  url      = "https://10.1.1.1"
  insecure = true  # Only for dev/test!
}
```

#### 5. Principle of Least Privilege

- Use service accounts with minimal required permissions
- Regularly rotate credentials
- Implement role-based access control (RBAC)
- Audit access logs regularly

#### 6. Keep Provider Updated

- Regularly update to the latest provider version
- Subscribe to security advisories
- Review changelogs for security fixes

#### 7. Review Generated Plans

Always review the Terraform plan before applying:

```bash
terraform plan  # Review changes carefully
terraform apply # Only after reviewing
```

### For Developers

If you're contributing to the provider:

#### 1. Never Commit Secrets

- Never commit passwords, API keys, or tokens
- Use environment variables in tests
- Review commits for accidentally included secrets
- Use `.gitignore` to exclude sensitive files

#### 2. Input Validation

- Validate all user inputs
- Sanitize data before using in API calls
- Use parameterized queries
- Implement proper error handling

#### 3. Secure API Communication

- Always use HTTPS for API calls
- Validate SSL certificates (except when explicitly disabled)
- Implement proper timeout and retry logic
- Handle authentication securely

#### 4. Dependency Management

- Regularly update dependencies
- Review security advisories for dependencies
- Use `go mod tidy` to clean unused dependencies
- Run security scanners on dependencies

#### 5. Code Review

- All code changes require review
- Security-sensitive changes need extra scrutiny
- Use automated security scanning tools
- Follow secure coding guidelines

## Supported Versions

Security updates are provided for:

- Current major version (latest release)
- Previous major version (for 6 months after new major release)

Older versions may receive security updates on a case-by-case basis.

## Security Contacts

- Security Issues: oss-security@cisco.com
- General Questions: GitHub Issues (for non-security issues)
- Community: GitHub Discussions

## Security Scanning

This project uses:

- Dependabot for dependency updates
- GitHub Security Advisories
- Automated vulnerability scanning

## Acknowledgments

We thank all security researchers who responsibly disclose vulnerabilities to help make this project more secure.

Security researchers who report vulnerabilities will be acknowledged in:
- Security advisories
- Release notes
- Project documentation (if they wish)

## Comments on this Policy

If you have suggestions on how this process could be improved, please submit a pull request or open an issue.

## Additional Resources

- [Terraform Security Best Practices](https://www.terraform.io/docs/cloud/guides/recommended-practices/index.html#security)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [Cisco Security](https://www.cisco.com/c/en/us/about/security-center.html)
