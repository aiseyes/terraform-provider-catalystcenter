# Wiki Documentation

This directory contains the wiki documentation for the Terraform Provider for Cisco Catalyst Center.

## Wiki Pages

- **[Home.md](Home.md)** - Main wiki landing page with overview and quick links
- **[Getting-Started.md](Getting-Started.md)** - Installation, configuration, and first examples
- **[Resources-and-Data-Sources.md](Resources-and-Data-Sources.md)** - Complete reference of available resources
- **[Contributing.md](Contributing.md)** - Guidelines for contributing to the project
- **[Development.md](Development.md)** - Developer guide for working on the provider
- **[Security-Policy.md](Security-Policy.md)** - Security procedures and reporting
- **[Code-of-Conduct.md](Code-of-Conduct.md)** - Community guidelines and standards
- **[Troubleshooting.md](Troubleshooting.md)** - Common issues and solutions
- **[FAQ.md](FAQ.md)** - Frequently asked questions

## How to Use

### For GitHub Wiki

If you want to use these files in a GitHub Wiki:

1. Go to your repository's Wiki tab
2. Clone the wiki repository:
   ```bash
   git clone https://github.com/aiseyes/terraform-provider-catalystcenter.wiki.git
   ```
3. Copy the markdown files from this directory to the wiki repository
4. Commit and push to the wiki:
   ```bash
   cd terraform-provider-catalystcenter.wiki
   git add *.md
   git commit -m "Add comprehensive wiki documentation"
   git push origin master
   ```

### As Standalone Documentation

These files can also be used as standalone documentation within the repository. They are written in standard Markdown and can be:

- Viewed directly in GitHub
- Converted to other formats (HTML, PDF)
- Served via documentation generators (MkDocs, Docusaurus, etc.)

## Maintaining the Wiki

When updating the wiki:

1. Keep content accurate and up-to-date with the provider
2. Maintain consistency in formatting and style
3. Include code examples where helpful
4. Link between related pages
5. Update the table of contents when adding new sections

## Contributing

Improvements to the wiki documentation are welcome! Please:

1. Follow the existing structure and style
2. Ensure all links work correctly
3. Test code examples
4. Keep content clear and concise
5. Submit pull requests with documentation updates

## Structure

The wiki is organized into the following categories:

### Getting Started
- Installation and setup
- Basic configuration
- First examples

### Usage
- Resources and data sources
- Common patterns
- Advanced configurations

### Development
- Building the provider
- Adding resources
- Testing

### Community
- Contributing guidelines
- Code of conduct
- Security policy

### Support
- Troubleshooting guide
- FAQ
- Getting help

## Additional Resources

- [Official Documentation](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest/docs)
- [GitHub Repository](https://github.com/aiseyes/terraform-provider-catalystcenter)
- [Examples](https://github.com/aiseyes/terraform-provider-catalystcenter/tree/main/examples)
- [Changelog](https://github.com/aiseyes/terraform-provider-catalystcenter/blob/main/CHANGELOG.md)
