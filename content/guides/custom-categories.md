---
title: "Custom Categories"
description: "Configure custom SolarWinds categories for your Azure alerts"
weight: 4
---

# Custom Categories

Learn how to use your own SolarWinds Service Desk categories and subcategories for incidents created by Alert Bridge.

---

## Overview

By default, Alert Bridge uses:
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

## Creating Categories in SolarWinds

### Step 1: Create Category

1. Log into SolarWinds Service Desk
2. Go to **Setup** → **Incidents** → **Categories**
3. Click **"+ Add Category"**
4. Enter name: `Cloud Services`
5. Click **Save**

### Step 2: Create Subcategory

1. Under the category created above
2. Click **"+ Add Subcategory"**
3. Enter name: `Azure Alerts`
4. Click **Save**

### Step 3: Note the Exact Names

⚠️ **Important:** Write down the EXACT names (case-sensitive)

---

## Configuring Alert Bridge

### Via Environment Variables

Update these in your Logic App configuration:

```json
{
  "INCIDENT_CATEGORY": "Cloud Services",
  "INCIDENT_SUBCATEGORY": "Azure Alerts"
}
```

### Steps to Configure

1. **Open Logic App** in Azure Portal
2. Go to **Configuration** (under Settings)
3. Find or add these variables:
   - `INCIDENT_CATEGORY`
   - `INCIDENT_SUBCATEGORY`
4. Enter your SolarWinds category names
5. Click **Save**
6. Logic App restarts automatically

---

## Configuration Examples

### Example 1: Infrastructure Alerts

```json
{
  "INCIDENT_CATEGORY": "Infrastructure",
  "INCIDENT_SUBCATEGORY": "Cloud Monitoring"
}
```

### Example 2: Department-Specific

```json
{
  "INCIDENT_CATEGORY": "IT Operations",
  "INCIDENT_SUBCATEGORY": "Azure Platform"
}
```

### Example 3: Service-Based

```json
{
  "INCIDENT_CATEGORY": "Platform Services",
  "INCIDENT_SUBCATEGORY": "Monitoring & Alerting"
}
```

### Example 4: Multi-Environment Setup

**Production Logic App:**
```json
{
  "INCIDENT_CATEGORY": "Production",
  "INCIDENT_SUBCATEGORY": "Azure Alerts",
  "INCIDENT_PREFIX": "[PROD]"
}
```

**Development Logic App:**
```json
{
  "INCIDENT_CATEGORY": "Development",
  "INCIDENT_SUBCATEGORY": "Azure Alerts",
  "INCIDENT_PREFIX": "[DEV]"
}
```

---

## Dynamic Categorization

### By Resource Type

Create different categories for different Azure resources:

| Alert Target | Category | Subcategory |
|--------------|----------|-------------|
| Virtual Machines | Infrastructure | Compute |
| SQL Databases | Databases | Azure SQL |
| Storage Accounts | Storage | Azure Storage |
| App Services | Applications | Web Apps |

**Implementation:** Requires Logic App workflow modification (contact support)

### By Subscription

Route alerts from different subscriptions to different categories:

| Subscription | Category | Subcategory |
|--------------|----------|-------------|
| Production | Production Services | Azure Monitor |
| Development | Development | Azure Alerts |
| Shared Services | Shared Infrastructure | Monitoring |

### By Resource Tags

Use Azure resource tags to determine category:

**Resource Tag:**
```
Environment: Production
Service: Web
```

**Resulting Category:**
```
Category: Production
Subcategory: Web Services
```

---

## Category-Based Assignment

Use categories to auto-assign incidents to teams:

### SolarWinds Assignment Rules

1. Go to **Setup** → **Assignment Rules**
2. Create rule:
   ```
   IF category = "Infrastructure"
   AND subcategory = "Azure Monitor"
   THEN assign to "Cloud Operations Team"
   ```

### Benefits

- ✅ Automatic routing to correct team
- ✅ No manual assignment needed
- ✅ Consistent incident handling
- ✅ Clear ownership

---

## Testing Categories

### 1. Fire Test Alert

Trigger an Azure alert that will create an incident

### 2. Check Incident in SolarWinds

1. Open the created incident
2. Verify **Category** field shows your configured value
3. Verify **Subcategory** field shows your configured value

### 3. Review Logic App Run

1. Go to Logic App → **Runs history**
2. Click latest run
3. Expand "Create or Update Incident" action
4. Check **Inputs** to see category being sent:

```json
{
  "incident": {
    "category": {
      "name": "Cloud Services"
    },
    "subcategory": {
      "name": "Azure Alerts"
    }
  }
}
```

---

## Common Issues

### Category Not Found

**Problem:** SolarWinds returns error: "Category 'XYZ' not found"

**Solutions:**
1. Verify category exists in SolarWinds
2. Check spelling (case-sensitive!)
3. Check for extra spaces
4. Try listing all categories via API:

```bash
curl -X GET "https://api.samanage.com/categories.json" \
  -H "X-Samanage-Authorization: Bearer YOUR_TOKEN"
```

### Subcategory Not Under Category

**Problem:** Incident created but wrong category/subcategory

**Solutions:**
1. Ensure subcategory is nested under the correct category
2. Check SolarWinds category hierarchy
3. Verify both category and subcategory names are exact matches

### Configuration Not Applied

**Problem:** Still using default categories after config change

**Solutions:**
1. Restart Logic App (disable/enable)
2. Check variable names are correct:
   - `INCIDENT_CATEGORY` (not `CATEGORY`)
   - `INCIDENT_SUBCATEGORY` (not `SUBCATEGORY`)
3. Verify values saved in Logic App config

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

## See Also

- [SolarWinds Setup](../getting-started/solarwinds-setup) - Create categories
- [Environment Variables](../reference/environment-variables) - All config options
- [Priority Mapping](./priority-mapping) - Priority configuration

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

