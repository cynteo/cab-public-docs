---
title: "Priority Mapping"
description: "Configure how Azure alert severities map to SolarWinds priorities"
weight: 2
---

# Priority Mapping

Learn how to map Azure Monitor alert severities to SolarWinds Service Desk priority levels.

---

## Overview

Azure Monitor uses **severities** (Sev0-Sev4), while SolarWinds uses **priorities** (Critical, High, Medium, Low). Alert Bridge maps between these automatically.

---

## Default Mapping

Out of the box, Alert Bridge uses this mapping:

| Azure Severity | SolarWinds Priority | Description |
|----------------|---------------------|-------------|
| **Sev0** | Critical | System down, immediate action required |
| **Sev1** | High | Major functionality impaired |
| **Sev2** | Medium | Moderate impact, workaround available |
| **Sev3** | Low | Minor issue, minimal impact |
| **Sev4** | Low | Informational, no immediate action |

---

## Customizing Priority Mapping

### Via Environment Variables

Update these variables in your Logic App configuration:

```json
{
  "SEV0_PRIORITY": "Critical",
  "SEV1_PRIORITY": "High",
  "SEV2_PRIORITY": "Medium",
  "SEV3_PRIORITY": "Low",
  "SEV4_PRIORITY": "Low"
}
```

### Steps to Update

1. **Open Logic App** in Azure Portal
2. Go to **Configuration** (under Settings)
3. Find the priority mapping variables
4. Update to match your SolarWinds priorities
5. Click **Save**
6. Logic App will restart automatically

---

## SolarWinds Priority Values

First, check what priorities are available in your SolarWinds instance:

### Via SolarWinds UI

1. Log into SolarWinds Service Desk
2. Go to **Setup** → **Incidents** → **Priorities**
3. Note the exact names (case-sensitive!)

### Via API

```bash
curl -X GET "https://api.samanage.com/incidents/priorities.json" \
  -H "X-Samanage-Authorization: Bearer YOUR_TOKEN" \
  -H "Accept: application/json"
```

Response:
```json
{
  "priorities": [
    { "name": "Critical", "priority": 1 },
    { "name": "High", "priority": 2 },
    { "name": "Medium", "priority": 3 },
    { "name": "Low", "priority": 4 }
  ]
}
```

**⚠️ Important:** Priority names must match EXACTLY (including capitalization).

---

## Custom Priority Examples

### Example 1: All High Priority

Treat all alerts as high priority:

```json
{
  "SEV0_PRIORITY": "High",
  "SEV1_PRIORITY": "High",
  "SEV2_PRIORITY": "High",
  "SEV3_PRIORITY": "High",
  "SEV4_PRIORITY": "High"
}
```

### Example 2: Skip Low Severity

Map low severity to "Info" (if available in SolarWinds):

```json
{
  "SEV0_PRIORITY": "Critical",
  "SEV1_PRIORITY": "High",
  "SEV2_PRIORITY": "Medium",
  "SEV3_PRIORITY": "Info",
  "SEV4_PRIORITY": "Info"
}
```

### Example 3: Custom Priority Names

If your SolarWinds uses custom names:

```json
{
  "SEV0_PRIORITY": "P1 - Critical",
  "SEV1_PRIORITY": "P2 - Urgent",
  "SEV2_PRIORITY": "P3 - Normal",
  "SEV3_PRIORITY": "P4 - Low",
  "SEV4_PRIORITY": "P4 - Low"
}
```

### Example 4: Simplified Mapping

Only use two priorities:

```json
{
  "SEV0_PRIORITY": "Critical",
  "SEV1_PRIORITY": "Critical",
  "SEV2_PRIORITY": "Normal",
  "SEV3_PRIORITY": "Normal",
  "SEV4_PRIORITY": "Normal"
}
```

---

## Testing Priority Mapping

### 1. Create Test Alert

Fire a test alert with specific severity:

```powershell
# In Azure Portal → Monitor → Alerts
# Manually trigger an alert rule
# Or use Azure CLI:

az monitor metrics alert create \
  --name "Test-Priority-Mapping" \
  --resource-group "your-rg" \
  --scopes "/subscriptions/.../resourceGroups/..." \
  --condition "avg Percentage CPU > 1" \
  --description "Test alert for priority mapping"
```

### 2. Check SolarWinds Incident

1. Wait 30 seconds for processing
2. Open SolarWinds Service Desk
3. Find the incident
4. Verify the priority matches your configuration

### 3. View Logic App Run

1. Go to Logic App → **Runs history**
2. Click latest run
3. Expand the "Create or Update Incident" action
4. Check the **Outputs** to see what priority was sent

---

## Priority-Based Routing

You can route different priorities to different teams:

### Using SolarWinds Assignment Rules

1. **Create Assignment Rules** in SolarWinds:
   - Priority = Critical → Assign to "On-Call Team"
   - Priority = High → Assign to "Infrastructure Team"
   - Priority = Medium/Low → Assign to "Operations Team"

2. **Configure in Alert Bridge** (optional):

```json
{
  "SEV0_PRIORITY": "Critical",
  "SEV0_ASSIGNEE_GROUP": "On-Call Team",
  "SEV1_PRIORITY": "High",
  "SEV1_ASSIGNEE_GROUP": "Infrastructure Team"
}
```

---

## Common Issues

### Priority Not Recognized

**Problem:** SolarWinds rejects the incident with "Invalid priority"

**Solutions:**
1. Check priority name spelling (case-sensitive)
2. Verify priority exists in SolarWinds
3. Check for extra spaces: `"High "` vs `"High"`

### All Incidents Get Same Priority

**Problem:** Configuration not taking effect

**Solutions:**
1. Restart Logic App after config change
2. Clear Logic App cache (disable/enable)
3. Verify variables in Logic App JSON view

### Wrong Priority Applied

**Problem:** Expected High but got Medium

**Solutions:**
1. Check Azure alert rule severity settings
2. Verify severity is being sent correctly
3. Check Logic App run history for input severity

---

## Dynamic Priority Mapping

For advanced scenarios, you can modify the Logic App workflow to:

- Map by resource type (VMs critical, storage medium)
- Map by resource tags (production critical, dev low)
- Map by time of day (night hours → higher priority)
- Map by subscription (prod high, dev low)

Contact [support@cynteocloud.com](mailto:support@cynteocloud.com) for custom mapping assistance.

---

## See Also

- [Environment Variables](../reference/environment-variables) - All configuration options
- [Severity Filtering](./severity-filtering) - Filter by severity
- [SolarWinds Setup](../getting-started/solarwinds-setup) - Priority configuration

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

