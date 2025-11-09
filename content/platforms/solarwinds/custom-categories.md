---
title: "Custom Categories"
description: "Configure custom SolarWinds categories for your Azure alerts"
weight: 4
---

# Custom Categories

Learn how to use your own SolarWinds Service Desk categories and subcategories for incidents created by Cynteo Alert Bridge.

---

## Overview

By default, Cynteo Alert Bridge uses:
- **Category:** Infrastructure
- **Subcategory:** Azure Monitor

You can customize these to match your organization's incident categorization system.

---

## SolarWinds Categories

### What are Categories?

Categories help organize incidents in SolarWinds:
- **Filter and Search** - Find incidents by category
- **Reporting** - Generate category-based reports
- **Routing Rules** - Auto-assign based on category
- **SLA Management** - Different SLAs per category

### Category Hierarchy

```
Category
  └── Subcategory
```

Example:
```
Cloud Services
  ├── Azure Monitor
  ├── Azure Resources
  └── AWS CloudWatch
```

---

## Setting Up Categories in SolarWinds

For Cynteo Alert Bridge to use custom categories, they must first exist in your SolarWinds instance:

### Category Structure

Categories follow a two-level hierarchy:
```
Category
  └── Subcategory
```

Example:
```
Cloud Services
  └── Azure Monitor
```

### Creating Categories

Your SolarWinds administrator can create categories:

1. **Setup** → **Incidents** → **Categories**
2. Add the desired category
3. Add subcategories under that category
4. Note the exact names (case-sensitive)

---

## Configuring Cynteo Alert Bridge

Cynteo Alert Bridge can be configured to use your custom SolarWinds categories during deployment.

**To use custom categories**, provide the exact category and subcategory names during deployment or contact [support@cynteocloud.com](mailto:support@cynteocloud.com) to update your configuration.

---

## Category Examples

Cynteo Alert Bridge supports various categorization strategies:

### Example 1: Infrastructure-Based
- **Category:** Infrastructure
- **Subcategory:** Cloud Monitoring

### Example 2: Department-Specific
- **Category:** IT Operations
- **Subcategory:** Azure Platform

### Example 3: Service-Based
- **Category:** Platform Services
- **Subcategory:** Monitoring & Alerting

### Example 4: Environment-Based
Different environments can use different categories:
- **Production:** Production / Azure Alerts
- **Development:** Development / Azure Alerts

---

## Advanced Categorization

Cynteo Alert Bridge supports dynamic categorization strategies:

### By Resource Type
Different Azure resource types can route to different categories:
- **Virtual Machines** → Infrastructure / Compute
- **SQL Databases** → Databases / Azure SQL
- **Storage Accounts** → Storage / Azure Storage
- **App Services** → Applications / Web Apps

### By Subscription
Alerts from different Azure subscriptions can use different categories for clear separation.

### By Resource Tags
Azure resource tags can influence category assignment, enabling fine-grained control based on your tagging strategy.

---

## Category-Based Assignment

Categories enable powerful routing capabilities in SolarWinds:

### Automatic Team Assignment

SolarWinds assignment rules can use categories to automatically route incidents:

**Example Rule:**
```
IF category = "Infrastructure"  
AND subcategory = "Azure Monitor"
THEN assign to "Cloud Operations Team"
```

### Benefits

- ✅ Automatic routing to the right team
- ✅ No manual triage needed
- ✅ Consistent incident handling
- ✅ Clear ownership from creation

---

## Verifying Categories

Once configured, you can verify categories are being applied correctly:

### Check SolarWinds Incidents

1. Open an incident created by Cynteo Alert Bridge
2. View the **Category** and **Subcategory** fields
3. Confirm they match your configuration

The category and subcategory determine how the incident is routed and tracked in your SolarWinds system.

---

## Best Practices

### Naming Conventions

Use clear, consistent names:
- ✅ `Cloud Services` → `Azure Monitor`
- ✅ `IT Operations` → `Cloud Alerts`
- ❌ `Misc` → `Other`
- ❌ `Test` → `Test123`

### Category Strategy

**Option 1: By Platform**
```
Cloud Services
  ├── Azure
  ├── AWS
  └── Google Cloud
```

**Option 2: By Service Type**
```
Infrastructure
  ├── Compute
  ├── Storage
  └── Networking
```

**Option 3: By Environment**
```
Production
  └── Azure Monitoring
Development
  └── Azure Monitoring
```

### Documentation

Document your category strategy:
1. Create a mapping table
2. Share with all teams
3. Include in onboarding docs
4. Review quarterly

---

## Reporting with Categories

### SolarWinds Reports

Generate reports by category:

1. Go to **Reports** → **Incidents**
2. Filter by category
3. See incident volumes, resolution times, etc.

### Useful Reports

- **Incidents by Category** - Volume trends
- **Resolution Time by Category** - Performance metrics
- **Team Performance** - By assigned category
- **SLA Compliance** - Category-specific SLAs

---

## Questions About Categories?

To configure custom categories for your deployment, contact [support@cynteocloud.com](mailto:support@cynteocloud.com) with your desired category structure.

---

## See Also

- [SolarWinds Setup](/platforms/solarwinds/setup) - SolarWinds prerequisites
- [Configuration Options](/reference/environment-variables) - Available settings
- [Priority Mapping](/guides/priority-mapping) - Priority configuration

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

