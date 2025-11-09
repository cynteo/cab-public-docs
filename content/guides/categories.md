---
title: "Incident Categories"
description: "Configure incident categorization for your ITSM platform"
weight: 4
---

# Incident Categories

Learn how to configure custom categories for incidents created by Cynteo Alert Bridge.

---

## Overview

Most ITSM platforms use categories to organize incidents:
- Filter and search incidents
- Generate category-based reports
- Route incidents to specific teams
- Apply category-specific SLAs

By default, Cynteo Alert Bridge uses general categories suitable for cloud monitoring. You can customize these during deployment to match your organization's categorization system.

---

## Default Categories

**Default configuration:**
- **Category:** Infrastructure
- **Subcategory:** Azure Monitor / Cloud Monitoring

---

## Custom Categories

Categories are configured during deployment to match your ITSM platform's structure.

**Requirements:**
1. Categories must already exist in your ITSM platform
2. Names must match exactly (case-sensitive)
3. Follow your platform's categorization hierarchy

---

## Category Strategy Examples

### Option 1: By Platform
```
Cloud Services
  ├── Azure
  ├── AWS
  └── Google Cloud
```

### Option 2: By Service Type
```
Infrastructure
  ├── Compute
  ├── Storage
  └── Networking
```

### Option 3: By Environment
```
Production
  └── Cloud Monitoring
Development  
  └── Cloud Monitoring
```

---

## Category-Based Routing

Use categories to automatically route incidents to the right team:

- **Infrastructure** → Infrastructure Team
- **Applications** → Application Support
- **Security** → Security Operations
- **Production** → On-Call Team

This is configured in your ITSM platform's assignment rules, not in Cynteo Alert Bridge.

---

## Platform-Specific Guides

For detailed category configuration for your specific ITSM platform:

- **[SolarWinds Service Desk](/platforms/solarwinds/categories)** - Complete SolarWinds category guide
- **ServiceNow** - Coming soon
- **Jira Service Management** - Coming soon

---

## Configuration

To use custom categories, specify them during deployment or contact [support@cynteocloud.com](mailto:support@cynteocloud.com).

---

## See Also

- [Configuration Options](/reference/environment-variables) - All settings
- [Priority Mapping](/guides/priority-mapping) - Priority configuration

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

