---
title: "SolarWinds Service Desk"
description: "Complete integration guide for SolarWinds Service Desk"
weight: 1
---

# SolarWinds Service Desk Integration

Complete guide for integrating Cynteo Alert Bridge with SolarWinds Service Desk.

---

## Overview

Cynteo Alert Bridge seamlessly integrates with SolarWinds Service Desk (formerly Samanage) to automatically:
- Create incidents from Azure Monitor alerts
- Update incidents as alerts continue or escalate
- Resolve incidents when alerts clear
- Add context-rich comments throughout the lifecycle

---

## Getting Started

### Step 1: SolarWinds Prerequisites

Ensure your SolarWinds instance is ready:
- Active SolarWinds Service Desk account
- Administrator access to generate API tokens
- Incident categories configured (optional)
- Assignment groups set up (optional)

### Step 2: Generate API Token

Follow our detailed guide to generate your API token:

[Generate SolarWinds API Token →](/platforms/solarwinds/api-token)

### Step 3: Configure Categories

Set up categories for organized incident tracking:

[Configure Categories →](/platforms/solarwinds/categories)

### Step 4: Deploy Alert Bridge

With your API token ready, deploy Alert Bridge from Azure Marketplace.

[Deployment Guide →](/getting-started/quickstart)

---

## SolarWinds-Specific Features

### Incident Field Mapping

Alert Bridge populates these SolarWinds fields:

| SolarWinds Field | Source | Customizable |
|------------------|--------|--------------|
| Name | Alert rule name | ✅ Prefix configurable |
| Priority | Alert severity | ✅ Mapping configurable |
| Description | Alert details | ❌ Auto-generated |
| Category | Configuration | ✅ During deployment |
| Subcategory | Configuration | ✅ During deployment |
| Requester | Configuration | ✅ During deployment |
| Assignee Group | Configuration | ✅ During deployment |

### Priority Mapping

Default severity-to-priority mapping:

| Azure Severity | SolarWinds Priority |
|----------------|---------------------|
| Sev0 (Critical) | Critical |
| Sev1 (Error) | High |
| Sev2 (Warning) | Medium |
| Sev3 (Informational) | Low |
| Sev4 (Verbose) | Low |

Custom mappings can be configured during deployment.

### Category Structure

SolarWinds uses a two-level category hierarchy:
```
Category
  └── Subcategory
```

Default:
```
Infrastructure
  └── Azure Monitor
```

You can specify custom categories during deployment.

---

## Documentation

### Setup Guides
- [Initial Setup](/platforms/solarwinds/setup) - Complete SolarWinds configuration
- [API Token Management](/platforms/solarwinds/api-token) - Generate and rotate tokens
- [Categories & Priorities](/platforms/solarwinds/categories) - Configure incident organization

### Troubleshooting
- [Common Issues](/troubleshooting/common-issues) - SolarWinds-specific problems
- [API Errors](/platforms/solarwinds/troubleshooting) - SolarWinds API error codes
- [Authentication Issues](/platforms/solarwinds/troubleshooting#authentication) - Token problems

---

## API Information

### SolarWinds API Endpoints

Alert Bridge uses these SolarWinds API endpoints:

- **Incidents API:** `https://api.samanage.com/incidents.json`
- **Comments API:** `https://api.samanage.com/incidents/{id}/comments.json`
- **Categories API:** `https://api.samanage.com/categories.json`

### Rate Limits

| SolarWinds Plan | Requests/Hour |
|-----------------|---------------|
| Trial | 100 |
| Standard | 1,000 |
| Premium | 5,000 |
| Enterprise | Custom |

Alert Bridge automatically queues requests to stay within limits.

---

## Support

### SolarWinds Resources
- [SolarWinds Service Desk Documentation](https://help.samanage.com)
- [SolarWinds API Documentation](https://help.samanage.com/s/article/API-Quick-Start-Guide)
- [SolarWinds Support](https://www.solarwinds.com/support)

### Cynteo Support
- **Email:** [support@cynteocloud.com](mailto:support@cynteocloud.com)
- **Documentation:** [cab-docs.cynteocloud.com](https://cab-docs.cynteocloud.com)

---

**Ready to get started?** [Set up SolarWinds →](/platforms/solarwinds/setup)

