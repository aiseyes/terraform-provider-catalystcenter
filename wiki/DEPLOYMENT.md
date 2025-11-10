# How to Deploy Wiki to GitHub

This guide explains how to deploy the wiki documentation to your GitHub repository's Wiki.

## Option 1: Using GitHub Wiki (Recommended)

GitHub provides a built-in Wiki feature for each repository. Here's how to use it:

### Step 1: Enable Wiki

1. Go to your repository on GitHub: https://github.com/aiseyes/terraform-provider-catalystcenter
2. Click on **Settings**
3. Scroll down to the **Features** section
4. Check the **Wikis** checkbox if not already enabled

### Step 2: Clone the Wiki Repository

Every GitHub Wiki is a separate Git repository:

```bash
git clone https://github.com/aiseyes/terraform-provider-catalystcenter.wiki.git
cd terraform-provider-catalystcenter.wiki
```

### Step 3: Copy Wiki Files

Copy all markdown files from the `wiki` directory:

```bash
# From the main repository root
cp wiki/*.md ../terraform-provider-catalystcenter.wiki/
```

### Step 4: Commit and Push

```bash
cd ../terraform-provider-catalystcenter.wiki
git add *.md
git commit -m "Add comprehensive wiki documentation"
git push origin master
```

### Step 5: Access Your Wiki

Visit: https://github.com/aiseyes/terraform-provider-catalystcenter/wiki

## Option 2: Keep Wiki in Main Repository

The wiki files can remain in the `wiki` directory of the main repository and be accessed directly:

### Benefits:
- Version controlled with the main repository
- Can be reviewed in pull requests
- Easy to maintain alongside code changes
- No separate repository to manage

### Access:
Users can browse the wiki files directly in the repository:
- https://github.com/aiseyes/terraform-provider-catalystcenter/tree/main/wiki

## Option 3: Documentation Website

For a more polished documentation site, consider:

### Using GitHub Pages with MkDocs

1. Install MkDocs:
   ```bash
   pip install mkdocs mkdocs-material
   ```

2. Create `mkdocs.yml`:
   ```yaml
   site_name: Terraform Provider Catalyst Center
   theme:
     name: material
   nav:
     - Home: wiki/Home.md
     - Getting Started: wiki/Getting-Started.md
     - Resources: wiki/Resources-and-Data-Sources.md
     - Contributing: wiki/Contributing.md
     - Development: wiki/Development.md
     - Security: wiki/Security-Policy.md
     - Code of Conduct: wiki/Code-of-Conduct.md
     - Troubleshooting: wiki/Troubleshooting.md
     - FAQ: wiki/FAQ.md
   ```

3. Build and deploy:
   ```bash
   mkdocs build
   mkdocs gh-deploy
   ```

### Using Docusaurus

1. Install Docusaurus:
   ```bash
   npx create-docusaurus@latest docs classic
   ```

2. Copy wiki files to `docs/docs/`
3. Configure and deploy to GitHub Pages

## Wiki Structure

The wiki includes the following pages:

```
wiki/
├── Home.md                          # Landing page (maps to Wiki home)
├── Getting-Started.md               # Installation and setup
├── Resources-and-Data-Sources.md   # Complete resource reference
├── Contributing.md                  # Contribution guidelines
├── Development.md                   # Developer guide
├── Security-Policy.md              # Security procedures
├── Code-of-Conduct.md              # Community standards
├── Troubleshooting.md              # Common issues
├── FAQ.md                           # Frequently asked questions
└── README.md                        # Wiki documentation index
```

## Updating the Wiki

### For GitHub Wiki

```bash
cd terraform-provider-catalystcenter.wiki
# Edit files
git add .
git commit -m "Update documentation"
git push origin master
```

### For Repository Wiki

```bash
cd terraform-provider-catalystcenter
# Edit files in wiki/ directory
git add wiki/
git commit -m "Update wiki documentation"
git push origin main
```

## Best Practices

1. **Keep in Sync**: Update wiki when making code changes
2. **Review Changes**: Include wiki updates in pull requests
3. **Link Between Pages**: Use relative links between wiki pages
4. **Update Examples**: Keep code examples current with the provider
5. **Version Documentation**: Tag documentation with releases

## Customization

### Sidebar (GitHub Wiki)

GitHub Wiki automatically generates a sidebar from page links in Home.md.

### Custom Sidebar

Create `_Sidebar.md` in the wiki repository:

```markdown
* [Home](Home)
* [Getting Started](Getting-Started)
* [Resources & Data Sources](Resources-and-Data-Sources)
* [Contributing](Contributing)
* [Development](Development)
* [Security Policy](Security-Policy)
* [Code of Conduct](Code-of-Conduct)
* [Troubleshooting](Troubleshooting)
* [FAQ](FAQ)
```

### Footer

Create `_Footer.md`:

```markdown
[GitHub](https://github.com/aiseyes/terraform-provider-catalystcenter) | 
[Issues](https://github.com/aiseyes/terraform-provider-catalystcenter/issues) | 
[Terraform Registry](https://registry.terraform.io/providers/CiscoDevNet/catalystcenter/latest)
```

## Maintenance

- Update wiki when adding new resources or features
- Keep troubleshooting guide current with common issues
- Add new FAQs based on user questions
- Review and update examples with each release
- Maintain links to external documentation

## Access Control

GitHub Wiki access:
- **Public repositories**: Wiki is public by default
- **Private repositories**: Wiki follows repository access rules
- **Permissions**: Same as repository permissions

To restrict wiki editing:
1. Go to repository Settings
2. Manage access permissions
3. Configure who can edit the wiki

## Resources

- [GitHub Wiki Documentation](https://docs.github.com/en/communities/documenting-your-project-with-wikis)
- [MkDocs Documentation](https://www.mkdocs.org/)
- [Docusaurus Documentation](https://docusaurus.io/)
- [Markdown Guide](https://www.markdownguide.org/)
