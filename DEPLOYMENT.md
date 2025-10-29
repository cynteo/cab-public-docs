# Deploying Documentation to GitHub Pages

This guide explains how to publish the Cynteo Alert Bridge documentation to GitHub Pages.

---

## Prerequisites

- GitHub account
- New public repository created
- Git installed locally

---

## Step-by-Step Deployment

### Step 1: Create New Public Repository

1. Go to [GitHub](https://github.com)
2. Click **"New repository"**
3. **Repository name:** `azure-alert-bridge-docs` (or your choice)
4. **Description:** `Cynteo Alert Bridge - Official Documentation`
5. **Visibility:** Public
6. ✅ Check **"Initialize with README"**
7. Click **"Create repository"**

---

### Step 2: Copy Documentation Files

From your current project:

```bash
# Navigate to your project root
cd C:\Users\tyler\Desktop\GitHubProjects\cynteo\azure-alert-bridge

# Copy public-docs folder to a temporary location
cp -r public-docs /tmp/docs-deploy
```

---

### Step 3: Clone New Repository

```bash
# Clone the new repository
git clone https://github.com/YOUR-USERNAME/azure-alert-bridge-docs.git
cd azure-alert-bridge-docs

# Copy documentation files
cp -r /tmp/docs-deploy/* .

# Verify files
ls -la
```

You should see:
```
- _config.yml
- README.md
- index.md
- getting-started/
- guides/
- reference/
- troubleshooting/
```

---

### Step 4: Commit and Push

```bash
# Add all files
git add .

# Commit
git commit -m "Initial documentation deployment"

# Push to GitHub
git push origin main
```

---

### Step 5: Enable GitHub Pages

1. Go to your repository on GitHub
2. Click **"Settings"** tab
3. Scroll to **"Pages"** in left sidebar
4. Under **"Source":**
   - Branch: `main`
   - Folder: `/ (root)`
5. Click **"Save"**

**Wait 2-3 minutes for deployment...**

---

### Step 6: Verify Deployment

Your site will be available at:
```
https://YOUR-USERNAME.github.io/azure-alert-bridge-docs
```

**Test these pages:**
- Homepage: `/`
- Quick Start: `/getting-started/quickstart`
- Troubleshooting: `/troubleshooting/common-issues`

---

## Configuration

### Update Site URL

Edit `_config.yml`:

```yaml
# Before
url: "https://cynteocloud.github.io"

# After (use your actual URL)
url: "https://YOUR-USERNAME.github.io"
baseurl: "/azure-alert-bridge-docs" # If using project pages
```

Commit and push:
```bash
git add _config.yml
git commit -m "Update site URL"
git push
```

---

### Custom Domain (Optional)

#### If You Own a Domain

1. **Add CNAME file**
   ```bash
   echo "docs.cynteocloud.com" > CNAME
   git add CNAME
   git commit -m "Add custom domain"
   git push
   ```

2. **Configure DNS**
   Add CNAME record:
   ```
   docs  →  YOUR-USERNAME.github.io
   ```

3. **Enable in GitHub**
   - Settings → Pages
   - Custom domain: `docs.cynteocloud.com`
   - ✅ Check "Enforce HTTPS"

---

## Updating Documentation

### Making Changes

1. **Edit files locally**
   ```bash
   cd public-docs
   # Edit files...
   ```

2. **Test locally (optional)**
   ```bash
   # Install Jekyll (one-time)
   gem install bundler jekyll

   # Serve locally
   bundle exec jekyll serve

   # View at http://localhost:4000
   ```

3. **Commit and push**
   ```bash
   git add .
   git commit -m "Update documentation"
   git push
   ```

4. **Wait 1-2 minutes**
   GitHub Pages automatically rebuilds

---

## File Structure

```
azure-alert-bridge-docs/
├── _config.yml              # Jekyll configuration
├── index.md                 # Homepage
├── README.md                # GitHub repo README
├── DEPLOYMENT.md            # This file
├── getting-started/
│   ├── quickstart.md
│   ├── solarwinds-setup.md
│   └── azure-monitor-setup.md
├── guides/
│   ├── configure-alert-action-group.md
│   ├── priority-mapping.md
│   ├── severity-filtering.md
│   └── custom-categories.md
├── reference/
│   ├── incident-fields.md
│   ├── alert-schema.md
│   ├── environment-variables.md
│   └── security.md
└── troubleshooting/
    ├── common-issues.md
    ├── deployment-errors.md
    └── alert-not-creating-tickets.md
```

---

## Themes

### Change Theme

Edit `_config.yml`:

```yaml
# Options:
theme: jekyll-theme-cayman      # Current (clean, modern)
theme: jekyll-theme-minimal     # Simple, sidebar
theme: jekyll-theme-slate       # Dark theme
theme: jekyll-theme-modernist   # Minimalist
theme: jekyll-theme-architect   # Bold headers
```

Commit and push to apply.

### Custom Theme

For more control, use a custom Jekyll theme:

1. Remove `theme:` line from `_config.yml`
2. Add `_layouts/default.html`
3. Add custom CSS in `assets/css/`

---

## SEO Optimization

### Update Meta Tags

Edit `_config.yml`:

```yaml
title: Cynteo Alert Bridge Documentation
description: Connect Azure Monitor to SolarWinds automatically
author: Cynteo Cloud

# Add social images
og_image: /assets/images/og-image.png
twitter_card: summary_large_image
```

### Add Images

```bash
mkdir -p assets/images
# Add your logo, screenshots, etc.
```

### Sitemap

Automatically generated at `/sitemap.xml`

### Google Analytics (Optional)

Edit `_config.yml`:

```yaml
google_analytics: UA-XXXXXXXXX-X
```

---

## Troubleshooting

### Pages Not Building

**Check build status:**
- Repository → Actions tab
- Look for failed workflows

**Common issues:**
- Syntax error in `_config.yml`
- Missing frontmatter in `.md` files
- Unsupported plugin

### 404 Errors

**Check:**
- URL matches file structure
- File extension is `.md`
- No typos in links

### Broken Links

**Fix relative links:**
```markdown
# Bad
[Quick Start](quickstart.md)

# Good
[Quick Start](./getting-started/quickstart.md)
```

---

## Maintenance

### Regular Updates

- ✅ Update documentation when features change
- ✅ Fix broken links monthly
- ✅ Update version numbers
- ✅ Review analytics to improve content

### Backups

GitHub is your backup! But also:
```bash
# Clone to local backup
git clone https://github.com/YOUR-USERNAME/azure-alert-bridge-docs.git backup/
```

---

## Going Live Checklist

Before announcing your docs site:

- [ ] All pages load without errors
- [ ] Links tested (use link checker tool)
- [ ] Images load correctly
- [ ] Mobile responsive (test on phone)
- [ ] Search works (if enabled)
- [ ] Custom domain configured (if using)
- [ ] SSL certificate active
- [ ] Analytics configured
- [ ] Contact email correct
- [ ] Footer links work

---

## Next Steps

1. **Add more guides** as features are released
2. **Add screenshots** to getting-started pages
3. **Add FAQ** page
4. **Add changelog** with version history
5. **Set up analytics** to track popular pages
6. **Add search** (Algolia or Google Custom Search)

---

## Resources

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Jekyll Documentation](https://jekyllrb.com/docs/)
- [Markdown Guide](https://www.markdownguide.org/)
- [Jekyll Themes](https://pages.github.com/themes/)

---

**Questions?** Contact your development team or [GitHub Support](https://support.github.com)

