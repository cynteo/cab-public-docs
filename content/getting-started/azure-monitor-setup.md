---
title: "Azure Monitor Configuration"
description: "Configure Azure Monitor alerts to work with Cynteo Alert Bridge"
weight: 3
---

# Azure Monitor Configuration

Configure Azure Monitor alerts to work with Cynteo Alert Bridge.

---

## Overview

Cynteo Alert Bridge integrates with Azure Monitor using **Action Groups** and **Common Alert Schema**. This guide shows you how to configure your alerts properly.

---

## Prerequisites

- Azure subscription with Monitor alerts configured
- Contributor access to create/modify alert rules
- Cynteo Alert Bridge deployed ([Quick Start](/getting-started/quickstart))

---

## How It Works

```
Azure Resource → Azure Monitor → Alert Rule → Action Group → Cynteo Alert Bridge → Your ITSM Platform
```

When an Azure resource triggers an alert:
1. **Azure Monitor** evaluates the condition
2. **Alert fires** and notifies the action group
3. **Action group** sends alert data to Cynteo Alert Bridge via webhook
4. **Cynteo Alert Bridge** processes the alert and creates/updates an incident
5. **ITSM platform** receives the incident with full context

---

## Step 1: Create Action Group

### 1.1 Navigate to Action Groups

1. Open [Azure Portal](https://portal.azure.com)
2. Search for **"Monitor"**
3. Click **"Alerts"** in left menu
4. Click **"Action groups"**
5. Click **"+ Create"**

### 1.2 Configure Basics

**Subscription:** Your Azure subscription  
**Resource Group:** Same as Cynteo Alert Bridge (recommended)  
**Region:** Global (default)  
**Action group name**  
**Display name**

### 1.3 Add Webhook Action

Click **"Next: Actions"**

**Action Details:**
- **Action type:** Webhook  
- **Name:** `Send to ITSM`  
- **URI:** Your Cynteo Alert Bridge webhook URL

**How to get the webhook URL:**
1. Go to **Azure Portal** → **Resource groups**
2. Open your Cynteo Alert Bridge resource group
3. Click the **Managed Application** resource
4. Go to **Overview** → **Outputs** tab
5. Copy the **"WebhookURL"** value

⚠️ **CRITICAL:** Check **"Enable the common alert schema"** checkbox

This is required for Cynteo Alert Bridge to properly parse alerts!

### 1.4 Review and Create

Click **"Review + create"** → **"Create"**

---

## Step 2: Configure Existing Alerts

### Add Action Group to Alert Rule

For each alert you want sent to your ITSM platform:

1. **Monitor** → **Alerts** → **Alert rules**
2. Select an alert rule
3. Click **"Edit"** (or **"Manage actions"**)
4. Under **"Action groups"**, click **"+ Add action group"**
5. Select your Cynteo Alert Bridge action group
6. Click **"Save"**

**Repeat for all relevant alert rules.**

---

## Step 3: Create New Alert Rule (Example)

### Example: CPU Alert for VM

#### 3.1 Create Alert Rule

1. Navigate to a **Virtual Machine**
2. Click **"Alerts"** in left menu
3. Click **"+ Create"** → **"Alert rule"**

#### 3.2 Configure Condition

**Signal:** Percentage CPU  
**Threshold:** Static  
**Operator:** Greater than  
**Threshold value:** 80  
**Aggregation type:** Average  
**Evaluation period:**
- Aggregation granularity: 5 minutes
- Frequency: 5 minutes

#### 3.3 Configure Actions

**Action group:** Select `alert-bridge-itsm`

#### 3.4 Configure Details

**Severity:** Select appropriate severity (Sev0-Sev3)  
**Alert rule name:** `High CPU Usage`  
**Description:** `Alert when CPU exceeds 80%`  
**Resource group:** Your resource group  
**Enable rule:** ✅ Yes

**Click "Review + create" → "Create"**

---

## Alert Severity Mapping

Azure Monitor severity maps to ITSM platform priority:

| Azure Severity | Typical ITSM Priority | Use Case |
|----------------|---------------------|----------|
| Sev0 | High | Critical outages |
| Sev1 | High | Major issues |
| Sev2 | Medium | Performance degradation |
| Sev3 | Low | Informational |

Priority mapping is configured during Cynteo Alert Bridge deployment. See [Priority Mapping](/guides/priority-mapping) for details.

---

## Common Alert Schema

### Why Common Alert Schema is Required

Cynteo Alert Bridge requires **Common Alert Schema** to properly parse alerts.

**✅ Correct (Common Alert Schema):**
```json
{
  "schemaId": "azureMonitorCommonAlertSchema",
  "data": {
    "essentials": {
      "alertId": "...",
      "severity": "Sev1",
      "monitorCondition": "Fired"
    }
  }
}
```

**❌ Incorrect (Legacy Schema):**
```json
{
  "status": "Activated",
  "context": {
    "severity": "High"
  }
}
```

### How to Enable

**When creating action group:**
- ✅ Check **"Enable the common alert schema"**

**For existing action groups:**
1. Edit action group
2. Edit webhook action
3. ✅ Check **"Enable the common alert schema"**
4. Save

---

## Alert Types Supported

Cynteo Alert Bridge supports all Azure Monitor alert types:

### Metric Alerts
- Virtual machine metrics (CPU, memory, disk)
- App Service metrics (response time, errors)
- Storage metrics (capacity, transactions)
- **ANY** Azure resource metric

### Log Alerts
- Log Analytics queries
- Application Insights queries
- Custom log searches

### Activity Log Alerts
- Resource health events
- Service health notifications
- Administrative operations

### Resource Health Alerts
- Resource availability changes
- Platform-initiated events

---

## Best Practices

### 1. Use Descriptive Alert Names

**Good:**
```
"Production Web App - High Response Time"
"Database Server - Low Memory"
"Storage Account - High Transactions"
```

**Bad:**
```
"Alert 1"
"Test"
"CPU"
```

Alert name becomes the incident title in your ITSM platform!

### 2. Set Appropriate Severity

- **Sev0:** Service down, data loss, security breach
- **Sev1:** Major functionality impaired
- **Sev2:** Degraded performance, non-critical issues
- **Sev3:** Informational, capacity planning

### 3. Add Helpful Descriptions

Alert descriptions appear in the incident details:

```
"Alert triggers when average CPU exceeds 80% for 5 minutes. 
Indicates potential capacity issues. Check recent deployments 
and scale up if needed."
```

### 4. Use Dynamic Thresholds (when applicable)

For variable workloads, use **dynamic thresholds** instead of static:

- Automatically adapts to patterns
- Reduces false positives
- Better for seasonal/cyclical workloads

### 5. Configure Alert Suppression

Prevent alert storms:

1. Edit alert rule
2. **Advanced options** → **Alert suppression**
3. Enable for appropriate duration (e.g., 5 minutes)

This prevents multiple incidents for rapid-fire alerts.

---

## Testing Your Alerts

### Method 1: Lower Threshold Temporarily

1. Edit alert rule
2. Lower threshold to trigger immediately (e.g., CPU > 1%)
3. Save
4. Wait 1-2 minutes
5. Verify incident created in your ITSM platform
6. Restore original threshold

### Method 2: Use Test Action Group

1. Action group → **"Test"** button
2. Select sample alert type
3. Click **"Test"**
4. Check your ITSM platform for the test incident

---

## Troubleshooting

### Alert Fires But No Incident

**Check:**
1. Action group uses **common alert schema** ✅
2. Webhook URL is correct
3. Logic App run history shows success
4. [Troubleshooting Guide](/troubleshooting/alert-not-creating-tickets)

### Incident Missing Information

**Cause:** Legacy alert schema used

**Fix:** Enable common alert schema in action group

### Duplicate Incidents

**Cause:** Multiple action groups sending same alert

**Fix:** Remove duplicate action groups from alert rule

---

## Advanced Configuration

### Multi-Subscription Setup

**Option A: Deploy Cynteo Alert Bridge in each subscription**
- Isolated per subscription
- Separate billing per deployment

**Option B: Use one Cynteo Alert Bridge for all subscriptions**
1. Deploy Cynteo Alert Bridge in "hub" subscription
2. Get webhook URL
3. Create action groups in each subscription pointing to same URL
4. All alerts go to same ITSM platform instance

### Filtering by Resource Group

Cynteo Alert Bridge processes ALL alerts sent to it. To filter which alerts create incidents:

1. Configure [severity filtering](/guides/severity-filtering) during deployment
2. Create separate action groups for different resource types or severities
3. Contact support to adjust filtering rules
4. Use your ITSM platform's rules for additional filtering

### What's Included in Incidents

Cynteo Alert Bridge includes in every incident:
- Alert name, severity, and description
- Affected resource details (name, type, region, subscription)
- Metric values or log query results
- Time stamps (when fired, when resolved)
- Direct links to Azure Portal for investigation
- Alert ID (for deduplication)

See [Incident Fields](/reference/incident-fields) for complete details.

---

## Next Steps

Now that Azure Monitor is configured:

- **[Test Your Integration](/getting-started/quickstart#step-4-test-integration)** - Verify end-to-end flow
- **[Priority Mapping](/guides/priority-mapping)** - Understand severity-to-priority mapping
- **[Severity Filtering](/guides/severity-filtering)** - Filter which alerts create incidents
- **[Platform-Specific Guides](/platforms/)** - Platform-specific configuration details

### For MSPs

Managing customer tenants? See the [MSP Deployment Guide](/getting-started/msp-deployment) for multi-customer architecture and best practices.

---

**Need help?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

