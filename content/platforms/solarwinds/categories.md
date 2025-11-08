---
title: "Categories & Priorities"
description: "Configure SolarWinds categories and priority mappings"
weight: 3
---

# SolarWinds Categories & Priorities

Learn how to configure categories and priority mappings for incidents created by Alert Bridge.

---

## Categories

### What are Categories?

SolarWinds uses a two-level categorization system to organize incidents:

```
Category
  └── Subcategory
```

**Example:**
```
Infrastructure
  ├── Azure Monitor
  ├── Cloud Services
  └── On-Premises Systems
```

### Default Categories

Alert Bridge uses these defaults:
- **Category:** Infrastructure
- **Subcategory:** Azure Monitor

### Custom Categories

You can specify custom categories during deployment. Categories must already exist in your SolarWinds instance.

**To create categories in SolarWinds:**

1. Log into SolarWinds Service Desk
2. Go to **Setup** → **Incidents** → **Categories**
3. Click **"+ Add Category"**
4. Create your category and subcategories
5. Note the exact names (case-sensitive)

---

## Priorities

### Priority Mapping

Alert Bridge maps Azure alert severities to SolarWinds priorities:

| Azure Severity | Default Priority | Customizable |
|----------------|------------------|--------------|
| Sev0 (Critical) | Critical | ✅ Yes |
| Sev1 (Error) | High | ✅ Yes |
| Sev2 (Warning) | Medium | ✅ Yes |
| Sev3 (Informational) | Low | ✅ Yes |
| Sev4 (Verbose) | Low | ✅ Yes |

### Custom Priority Mapping

Priority mappings can be customized during deployment to match your organization's SolarWinds priority structure.

**Common customizations:**
- All critical alerts → Critical priority
- Production alerts → Higher priority than dev
- Different priorities by resource type

Contact [support@cynteocloud.com](mailto:support@cynteocloud.com) to configure custom mappings.

---

## Checking Your SolarWinds Configuration

### View Available Priorities

1. Log into SolarWinds
2. Go to **Setup** → **Incidents** → **Priorities**
3. Note your priority names

Common SolarWinds priority structures:
- Critical, High, Medium, Low
- P1, P2, P3, P4
- Urgent, High, Normal, Low

### View Available Categories

1. Log into SolarWinds
2. Go to **Setup** → **Incidents** → **Categories**
3. Review your category hierarchy

---

## Assignment Groups

### Auto-Assignment

SolarWinds can automatically assign incidents to teams based on category and priority.

**Example Assignment Rule:**
```
IF category = "Infrastructure"
AND priority = "Critical"
THEN assign to "On-Call Team"
```

This is configured in SolarWinds, not Alert Bridge.

### Configuring Assignment

Alert Bridge can optionally specify an assignment group during incident creation. This is configured during deployment.

**Benefits:**
- Automatic routing to correct team
- No manual triage needed
- Consistent incident handling

---

## Best Practices

### Category Strategy

**Option 1: By Platform**
```
Cloud Services
  ├── Azure
  ├── AWS
  └── Google Cloud
```

**Option 2: By Service**
```
Infrastructure
  ├── Compute
  ├── Storage
  └── Networking
```

**Option 3: By Environment**
```
Production
  └── Azure Monitor
Development  
  └── Azure Monitor
```

### Priority Strategy

**Recommendation:** Keep priority mapping simple and consistent
- Critical issues → High priority
- Warnings → Medium priority  
- Informational → Low priority

Adjust based on your operational needs and SLA requirements.

---

## See Also

- [SolarWinds Setup](./setup) - Initial configuration
- [API Token Guide](./api-token) - Token management
- [Priority Mapping Guide](../../guides/priority-mapping) - Advanced mapping options

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

