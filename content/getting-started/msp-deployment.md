---
title: "MSP Deployment Guide"
description: "Deploy Cynteo Alert Bridge to customer tenants as an MSP"
weight: 3
---

# MSP Deployment Guide

Complete guide for Managed Service Providers deploying Cynteo Alert Bridge to customer Azure tenants.

---

## Overview

As an MSP, you can deploy Cynteo Alert Bridge to each customer tenant to automatically route their Azure alerts to their ITSM tickets.

**Key Points:**
- Deploy **one instance per customer tenant**
- Each deployment is isolated to that customer's Azure subscription
- Route tickets using board numbers or ticket owners (platform-specific)
- Centralized billing through MSP account

---

## Deployment Architecture

### Single-Tenant Deployment

Each customer gets their own isolated deployment:

```
Customer A Tenant
├── Cynteo Alert Bridge Instance A
└── Routes to Customer A's ITSM Board

Customer B Tenant
├── Cynteo Alert Bridge Instance B
└── Routes to Customer B's ITSM Board

Customer C Tenant
├── Cynteo Alert Bridge Instance C
└── Routes to Customer C's ITSM Board
```

**Why One Per Tenant:**
- Complete isolation between customers
- Customer-specific configuration
- Separate billing and usage tracking
- Independent lifecycle management

---

## Prerequisites

For each customer deployment, you need:

- [ ] Customer's Azure tenant access (Contributor role minimum)
- [ ] Customer's ITSM platform credentials
  - API token with incident management permissions
  - Board number or ticket owner identifier
- [ ] Customer approval for Azure resource deployment
- [ ] Customer's contact email for notifications

---

## Deployment Process

### Step 1: Access Customer Tenant

1. **Azure Portal** → Click your profile
2. **Switch directory** → Select customer tenant
3. Verify you're in the correct tenant (check tenant name in top-right)

### Step 2: Deploy from Marketplace

1. Search for **"Cynteo Alert Bridge"** in Azure Marketplace
2. Click **"Get It Now"** → **"Continue"**
3. **Fill in deployment form:**

**Basics:**
- **Subscription:** Customer's Azure subscription
- **Resource Group:** Create new: `rg-cab-{customer-name}`
- **Region:** Customer's primary region
- **Application Name:** `cab-{customer-name}-prod`

**Platform Configuration:**
- **Platform Type:** SolarWinds / ConnectWise (Q1 2026) / etc.
- **API Token:** Customer's ITSM API token
- **API URL:** Customer's ITSM instance URL
- **Board Number:** Customer's specific board/queue
  - **SolarWinds:** Not applicable (uses categories)
  - **ConnectWise:** Board number for incident routing
- **Requester Email:** `azure-alerts@{customer-domain}.com`

**Alert Configuration:**
- **Priority Mapping:** Customer-specific preferences
- **Category:** Customer's ITSM category structure
- **Environment Tag:** `[{CUSTOMER-NAME}]` (helps identify in your monitoring)

**Notifications:**
- **Email:** Your MSP monitoring email (get notified of issues)

4. **Review + Create** → **Deploy**

### Step 3: Configure Action Groups

Create action groups in customer tenant:

1. **Azure Portal** → **Monitor** → **Action groups**
2. Create action group with Cynteo Alert Bridge webhook
3. Name: `alert-bridge-{customer-name}`
4. Link to customer's alert rules

See [Configure Action Groups](/guides/configure-alert-action-group) for detailed steps.

### Step 4: Verify

1. Trigger a test alert in customer tenant
2. Verify incident created in customer's ITSM with correct board/owner
3. Confirm alert resolution updates the incident
4. Document customer-specific configuration

---

## Platform-Specific Routing

### SolarWinds Service Desk

**Routing Method:** Categories and requester

```
Configuration:
- Category: Customer-specific category
- Subcategory: "Azure Monitor" or custom
- Requester: Customer-specific email
```

No board number needed - routing via categories and assignment rules.

### ConnectWise (Coming Q1 2026)

**Routing Method:** Board number and ticket owner

```
Configuration:
- Board Number: Customer's service board ID (e.g., "1", "5", "10")
- Ticket Owner: Customer's default owner or team
- Company: Customer identifier in ConnectWise
```

Each customer's alerts route to their specific board.

### ServiceNow (Planned)

**Routing Method:** Assignment group

```
Configuration:
- Assignment Group: Customer-specific group ID
- Caller: Customer contact
- Configuration Item: Optional CMDB integration
```

---

## Multi-Customer Management

### Naming Conventions

Use consistent naming for easy management:

```
Resource Group: rg-cab-{customer-name}
Logic App: logicapp-cab-{customer-name}
Key Vault: kv-cab-{customer-name}-{random}
Storage: stcab{customer-name}{random}
```

### Tagging Strategy

Tag all resources for tracking:

```json
{
  "Customer": "Acme Corp",
  "MSP": "YourMSPName",
  "Environment": "Production",
  "BillingCode": "ACME-123",
  "CostCenter": "Managed Services"
}
```

### Documentation

For each customer deployment, document:
- Customer name and tenant ID
- Deployment date
- ITSM platform and configuration
- Board number / routing information
- Primary contact
- Usage tier (Trial, Basic, Professional, etc.)

---

## Billing & Usage

### MSP Billing Model

- Each customer deployment billed separately
- Usage tracked per customer tenant
- Consolidated MSP billing available (contact sales)
- Volume discounts for 10+ customers

### Usage Monitoring

Monitor each customer's usage:

1. Go to customer's resource group
2. View managed application
3. Check usage metrics
4. Receive notifications at 80% and 100% of plan limits

### Plan Recommendations

| Customer Size | Alerts/Month | Recommended Plan |
|---------------|--------------|------------------|
| Small (1-10 resources) | < 500 | Trial → Basic |
| Medium (10-50 resources) | 500-3,000 | Professional |
| Large (50+ resources) | 3,000-15,000 | Business |
| Enterprise | 15,000+ | Enterprise |

---

## Best Practices

### 1. Standardized Configuration

Create a standard configuration template for all customers:

```
✅ Standard Categories: "Infrastructure" / "Azure Monitor"
✅ Standard Priority Mapping: Sev0→Critical, Sev1→High, etc.
✅ Standard Naming: Consistent resource names
```

Customize only when customer requires it.

### 2. Testing Process

For each deployment:
1. Deploy to test resource group first
2. Fire test alert
3. Verify incident created correctly
4. Test resolution workflow
5. Document and hand off to customer

### 3. Runbook

Create a deployment runbook with:
- Deployment checklist
- Customer information template
- Testing procedures
- Handoff documentation
- Support escalation process

### 4. Monitoring

Set up MSP-wide monitoring:
- Create dashboard showing all customer deployments
- Alert on any customer deployment failures
- Track usage across all customers
- Monthly usage reports per customer

---

## Customer Onboarding

### Onboarding Checklist

For each new customer:

**Pre-Deployment:**
- [ ] Customer tenant access confirmed
- [ ] ITSM platform details collected
- [ ] API token generated with proper permissions
- [ ] Board number / routing info confirmed
- [ ] Customer approval obtained

**Deployment:**
- [ ] Cynteo Alert Bridge deployed
- [ ] Test alert successful
- [ ] Incident created with correct routing
- [ ] Resolution tested
- [ ] Usage tier confirmed

**Post-Deployment:**
- [ ] Customer training provided
- [ ] Documentation delivered
- [ ] Support contact information shared
- [ ] Monitoring enabled
- [ ] Billing activated

### Customer Handoff

Provide customers with:
1. **What was deployed** - Resource list and purpose
2. **How it works** - Architecture overview
3. **What they can see** - Read-only access to resources
4. **Support process** - How to get help
5. **Usage information** - Current plan and limits

---

## Troubleshooting

### Common MSP Scenarios

**Issue: Customer has multiple tenants**
- Deploy separate instance in each tenant
- Each tenant billed separately
- Cannot share single deployment across tenants

**Issue: Customer wants multiple ITSM boards**
- Deploy multiple instances with different board configurations
- Name clearly: `cab-customer-board1`, `cab-customer-board2`
- Use action groups to route different alerts to different instances

**Issue: Customer wants to change configuration**
- Configuration cannot be changed post-deployment
- Must re-deploy with new settings
- Or contact support for configuration updates

---

## Security & Compliance

### Customer Data Isolation

- Each customer deployment is completely isolated
- No data shared between customer instances
- Customer credentials stored in their Key Vault only
- Customer controls their Azure resources

### MSP Access

As the MSP, you have:
- ✅ Access to deploy and configure
- ✅ Read-only access to managed application
- ❌ No access to modify deployed resources
- ❌ No access to customer ITSM credentials (after deployment)

### Compliance

- Each deployment SOC 2 compliant
- Customer data stays in customer tenant
- GDPR compliant (customer is data controller)
- No cross-customer data flow

---

## Pricing for MSPs

### Volume Discounts

| Total Customers | Discount |
|-----------------|----------|
| 1-9 | Standard pricing |
| 10-24 | 10% discount |
| 25-49 | 15% discount |
| 50+ | Custom pricing |

### MSP Program

Join our MSP Partner Program:
- **Priority support** - Dedicated MSP support channel
- **Training** - MSP-specific training and resources
- **Co-marketing** - Joint marketing opportunities
- **Revenue share** - Earn on customer referrals

**Apply:** [partners@cynteocloud.com](mailto:partners@cynteocloud.com)

---

## Support for MSPs

### MSP Support

- **Email:** [msp-support@cynteocloud.com](mailto:msp-support@cynteocloud.com)
- **Response Time:** < 4 hours
- **Slack Channel:** Available for MSP partners
- **Monthly Office Hours:** Technical Q&A sessions

### Resources

- **MSP Portal:** (Coming soon) - Manage all customer deployments
- **API Access:** Programmatic deployment via ARM templates
- **Reporting:** Usage across all customers
- **Billing Dashboard:** Consolidated billing view

---

## See Also

- [Quick Start Guide](/getting-started/quickstart) - Standard deployment
- [Platform Guides](/platforms/) - Platform-specific configuration
- [Security Overview](/reference/security) - Security architecture

---

**MSP Questions?** Contact [msp-support@cynteocloud.com](mailto:msp-support@cynteocloud.com)

