# Cynteo Alert Bridge Documentation

Welcome to the Cynteo Alert Bridge documentation! This site provides everything you need to deploy, configure, and troubleshoot your Azure Monitor to SolarWinds Service Desk integration.

## 🚀 Quick Links

- **[View Documentation](https://cynteocloud.github.io/cab-public-docs/)** - Live documentation site
- **[Get Started](./content/getting-started/quickstart)** - Deploy in 15 minutes
- **[Troubleshooting](./content/troubleshooting/common-issues)** - Fix common issues
- **[All Guides](./content/guides/)** - Configuration and customization

---

## 📚 Local Development

This documentation site is built with [Hugo](https://gohugo.io/).

### Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) (v0.121.0 or later)
- Git

### Running Locally

```bash
# Clone the repository
git clone https://github.com/cynteocloud/cab-public-docs.git
cd cab-public-docs

# Start Hugo development server
hugo server -D

# Open browser to http://localhost:1313
```

### Building for Production

```bash
# Build static site
hugo --gc --minify

# Output will be in ./public/
```

---

## 📂 Repository Structure

```
cab-public-docs/
├── content/              # All documentation content
│   ├── _index.md        # Homepage
│   ├── getting-started/ # Getting started guides
│   ├── guides/          # Configuration guides
│   ├── reference/       # Technical reference
│   └── troubleshooting/ # Troubleshooting guides
├── .github/
│   └── workflows/
│       └── hugo.yml     # GitHub Actions deployment
├── hugo.toml            # Hugo configuration
└── README.md            # This file
```

---

## 🚀 Deployment

This site automatically deploys to GitHub Pages when changes are pushed to the `main` branch using GitHub Actions.

### Manual Deployment

If you need to manually deploy:

1. Make sure GitHub Pages is enabled in repository settings
2. Set source to "GitHub Actions"
3. Push changes to `main` branch
4. GitHub Actions will automatically build and deploy

---

## 📝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

---

## 📊 What's New

### Version 1.4.5 (Latest)

**Fixed:**
- ✅ Incident resolution now correctly updates SolarWinds state
- ✅ Smart comment deduplication prevents ticket spam
- ✅ Improved error handling for API timeouts

**Added:**
- 🎯 New priority mapping options
- 📊 Enhanced logging and diagnostics

---

## 💬 Support

- **Documentation:** [cab-docs.cynteocloud.com](https://cab-docs.cynteocloud.com)
- **Email:** support@cynteocloud.com
- **Response Time:** < 24 hours (< 4 hours for Enterprise)

---

## 📄 License

© 2025 Cynteo Cloud. All rights reserved.
