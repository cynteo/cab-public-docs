---
title: "Quick Start Guide"
description: "Get Cynteo Alert Bridge running in 15 minutes"
weight: 1
---

# Quick Start Guide

Get Cynteo Alert Bridge running in 15 minutes.

---

## Prerequisites

Before you begin, make sure you have:

- [ ] Azure subscription (with Owner or Contributor role)
- [ ] SolarWinds Service Desk account
- [ ] SolarWinds API token ([Generate here](./solarwinds-setup.md#generating-api-token))
- [ ] At least one Azure Monitor alert configured

---

## Step 1: Deploy from Azure Marketplace

### 1.1 Find Cynteo Alert Bridge

1. Go to [Azure Marketplace](https://azuremarketplace.microsoft.com)
2. Search for **"Cynteo Alert Bridge for SolarWinds"**
3. Click **"Get It Now"**
4. Click **"Continue"** to go to Azure Portal

### 1.2 Fill in Deployment Form

**Basics Tab:**
- **Subscription:** Select your Azure subscription
- **Resource Group:** Create new (e.g., `rg-alert-bridge`) or select existing
- **Region:** Same region as your resources (e.g., East US)
- **Application Name:** Give it a friendly name (e.g., `alert-bridge-prod`)

**SolarWinds Configuration Tab:**
- **API Token:** Paste your SolarWinds API token
- **Base URL:** Usually `https://api.samanage.com` (default)
- **Requester Email:** Email for created incidents (e.g., `azure-monitor@yourcompany.com`)

**Alert Configuration Tab:**
- **Priority Mapping:**
  - Sev0 → High
  - Sev1 → High
  - Sev2 → Medium
  - Sev3 → Low
- **Category:** Infrastructure (or your preferred category)
- **Subcategory:** Azure Monitor (or your preferred subcategory)

**Notifications Tab:**
- **Email:** Your email for usage notifications

### 1.3 Review and Create

1. Click **"Review + create"**
2. Review settings
3. Click **"Create"**
4. Wait ~5 minutes for deployment

---

## Step 2: Get Webhook URL

Once deployment completes:

1. Go to your resource group
2. Find the Logic App (name: `logicapp-*`)
3. Click **"Overview"**
4. Copy the **"Webhook URL"** (you'll need this next)

**Example URL:**
```
https://prod-123.eastus.logic.azure.com:443/workflows/.../triggers/manual/paths/invoke?...
```

---

## Step 3: Configure Azure Monitor Alert Action Group

### 3.1 Create Action Group

1. In Azure Portal, search for **"Monitor"**
2. Click **"Alerts" → "Action groups"**
3. Click **"+ Create"**

**Basics:**
- **Subscription:** Your subscription
- **Resource Group:** Same as Alert Bridge
- **Action group name:** `alert-bridge-solarwinds`
- **Display name:** `SolarWinds`

**Actions:**
- **Action type:** Webhook
- **Name:** `Send to SolarWinds`
- **Webhook URL:** Paste URL from Step 2
- **Enable common alert schema:** ✅ **YES** (Important!)

**Click "Review + create" → "Create"**

### 3.2 Add to Alert Rules

For each alert you want sent to SolarWinds:

1. Go to **Monitor → Alerts → Alert rules**
2. Select an alert rule
3. Click **"Edit"**
4. Under **"Actions"**, click **"+ Action group"**
5. Select `alert-bridge-solarwinds`
6. Click **"Save"**

---

## Step 4: Test It!

### 4.1 Trigger a Test Alert

**Option A: Use existing alert**
- Wait for an alert to naturally fire

**Option B: Create test alert**
1. Monitor → Alerts → + Create → Alert rule
2. Select a resource (e.g., VM)
3. Condition: CPU > 1% (will fire immediately)
4. Actions: Select `alert-bridge-solarwinds`
5. Save and wait 1-2 minutes

### 4.2 Verify in SolarWinds

1. Go to SolarWinds Service Desk
2. Check **Incidents** list
3. You should see a new incident:
   - **Title:** `Azure Alert: [Your Alert Name]`
   - **Priority:** Based on severity mapping
   - **Description:** Rich HTML with alert details

**If you don't see it:** Check [Troubleshooting Guide](../troubleshooting/alert-not-creating-tickets)

---

## Step 5: Test Alert Resolution

### 5.1 Resolve the Alert

- If you created a test alert with CPU > 1%, delete the alert rule
- If using real alert, wait for condition to clear

### 5.2 Verify in SolarWinds

1. Go to the incident created in Step 4.2
2. Wait ~2 minutes
3. Incident should show:
   - **State:** Resolved
   - **Resolution Description:** "Resolved automatically by Azure Monitor..."
   - **Comment:** Details about resolution

---

## ✅ You're Done!

Your Azure Monitor alerts are now automatically creating SolarWinds incidents!

---

## Next Steps

### Customize Your Integration

- **[Priority Mapping](../guides/priority-mapping)** - Adjust severity-to-priority mapping
- **[Severity Filtering](../guides/severity-filtering)** - Ignore specific alert severities
- **[Custom Categories](../guides/custom-categories)** - Use your SolarWinds categories
- **[Comment Deduplication](../guides/comment-deduplication)** - Control update frequency

### Monitor Usage

- View usage in Azure Portal (Resource Group → Managed Application)
- Receive email notifications at 80% and 100% of plan limits
- Upgrade plan if needed

### Get Support

- **Documentation:** [All Guides](../guides/)
- **Troubleshooting:** [Common Issues](../troubleshooting/common-issues)
- **Email:** support@cynteocloud.com

---

## Common Questions

**Q: Do I need to configure anything in SolarWinds?**  
A: No! Just make sure your API token has permission to create incidents.

**Q: Will this create duplicate tickets?**  
A: No, Alert Bridge tracks alerts and updates the same ticket if an alert fires multiple times.

**Q: What happens if an alert fires then resolves then fires again?**  
A: A new incident will be created for the second occurrence.

**Q: Can I use this with multiple Azure subscriptions?**  
A: Yes! Deploy Alert Bridge in each subscription, or use one deployment and add action groups in each subscription pointing to the same webhook URL.

**Q: What's included in the free trial?**  
A: Full functionality, up to your plan's alert limit, for 30 days. No credit card required.

---

**Need help?** Check our [troubleshooting guide](../troubleshooting/common-issues) or [contact support](mailto:support@cynteocloud.com).

