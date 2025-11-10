---
title: "Architecture Overview"
description: "Understanding how Cynteo Alert Bridge works and deploys"
weight: 8
---

# Architecture Overview

Understand the technical architecture of Cynteo Alert Bridge and how it integrates Azure Monitor with your ITSM platform.

---

## Deployment Model

### Azure Managed Application

Cynteo Alert Bridge deploys as an **Azure Managed Application**, providing a fully managed solution:

```
Customer Azure Subscription
└── Resource Group (Customer-managed)
    └── Managed Application (Cynteo-managed)
        ├── Logic App (Processing engine)
        ├── Function App (Business logic)
        ├── Key Vault (Credential storage)
        └── Storage Account (Operational data)
```

### What This Means

**For Customers:**
- ✅ Solution deploys to YOUR Azure subscription
- ✅ Read-only visibility to all resources
- ✅ Full control over deployment lifecycle (deploy/delete)
- ✅ Transparent billing through Azure
- ❌ Cannot modify solution code or configuration
- ❌ Cannot access internal workflows or secrets

**For Cynteo:**
- ✅ Manages solution updates and improvements
- ✅ Ensures consistent behavior across customers
- ✅ Controls access to proprietary logic
- ❌ No access to your ITSM credentials (encrypted)
- ❌ No access to your Azure resources

---

## Data Flow

### Alert Processing Flow

```
1. Azure Monitor Alert Fires
        ↓
2. Action Group Webhook Triggered
        ↓
3. Logic App Receives Alert (via HTTPS)
        ↓
4. Function App Processes Alert
   - Validates schema
   - Checks for existing incident (deduplication)
   - Enriches with resource metadata
   - Applies configuration (priority mapping, filtering)
        ↓
5. API Call to ITSM Platform
   - Creates new incident OR
   - Updates existing incident
        ↓
6. Response Logged
        ↓
7. Incident Appears in ITSM
```

### Resolution Flow

```
1. Azure Monitor Alert Resolves
        ↓
2. Action Group Webhook Triggered (resolution)
        ↓
3. Logic App Receives Resolution
        ↓
4. Function App Processes Resolution
   - Finds existing incident
   - Prepares resolution note
        ↓
5. API Call to ITSM Platform
   - Updates incident state to Resolved
   - Adds resolution comment
        ↓
6. Incident Marked Resolved in ITSM
```

---

## Components

### Logic App

**Purpose:** Workflow orchestration and webhook endpoint

**What it does:**
- Receives webhooks from Azure Monitor
- Triggers Function App processing
- Handles retries and error handling
- Provides execution visibility

**Customer Access:** Read-only (cannot view workflow details)

### Function App

**Purpose:** Business logic and ITSM integration

**What it does:**
- Validates alert schema
- Deduplication logic
- Priority and category mapping
- ITSM API communication
- Error handling and logging

**Customer Access:** Read-only (cannot view code or configuration)

### Key Vault

**Purpose:** Secure credential storage

**What it stores:**
- ITSM API token (encrypted)
- Webhook signatures
- Other sensitive configuration

**Customer Access:** Read-only (cannot view secrets)

### Storage Account

**Purpose:** Operational data and deduplication tracking

**What it stores:**
- Alert deduplication cache (24-48 hours)
- Execution logs
- Temporary processing data

**Customer Access:** Read-only (cannot view contents)

**Data Retention:** 30 days maximum, then auto-deleted

---

## Security Architecture

### Data at Rest

- **Key Vault:** AES-256 encryption
- **Storage Account:** AES-256 encryption
- **Managed Disks:** BitLocker encryption

### Data in Transit

- **Azure Monitor → Logic App:** HTTPS (TLS 1.2+)
- **Logic App → Function App:** Internal (encrypted)
- **Function App → ITSM:** HTTPS (TLS 1.2+)
- **All API calls:** Encrypted end-to-end

### Network Security

- **No Public Endpoints** - Optional (can use private endpoints)
- **Azure Firewall** - Outbound traffic control available
- **NSG Support** - Network security groups for additional protection

---

## Scalability & Performance

### Processing Capacity

| Plan | Concurrent Alerts | Processing Time | Throughput |
|------|-------------------|-----------------|------------|
| Trial/Basic | 10 | < 3 seconds | 1,000/month |
| Professional | 25 | < 3 seconds | 5,000/month |
| Business | 50 | < 2 seconds | 20,000/month |
| Enterprise | 100+ | < 2 seconds | Unlimited |

### Auto-Scaling

- Function App scales automatically based on load
- Logic App handles concurrent executions
- No manual scaling required
- Elastic capacity during alert storms

---

## High Availability

### Availability SLA

- **Service SLA:** 99.9% uptime
- **Azure Logic Apps SLA:** 99.9%
- **Azure Functions SLA:** 99.95%

### Redundancy

- Multi-instance deployment
- Automatic failover
- Regional availability
- Retry logic for transient failures

### Disaster Recovery

**RTO (Recovery Time Objective):** < 1 hour  
**RPO (Recovery Point Objective):** 0 (stateless processing)

**Recovery Process:**
- Automatic recovery for most failures
- Manual redeployment if needed (< 10 minutes)
- No data loss (alert data comes from Azure Monitor)

---

## Configuration Management

### Deployment-Time Configuration

All configuration happens during deployment:
- ITSM API credentials
- Priority mapping
- Category structure
- Filtering rules
- Notification settings

### Post-Deployment Changes

**To change configuration after deployment:**

1. **Option A: Redeploy**
   - Deploy new instance with updated configuration
   - Update action groups to point to new webhook
   - Delete old instance

2. **Option B: Contact Support**
   - Email [support@cynteocloud.com](mailto:support@cynteocloud.com)
   - Provide desired changes
   - Cynteo applies configuration updates

---

## Monitoring & Observability

### What Customers Can Monitor

- **Deployment Health:** Green/Red status in Azure Portal
- **Usage Metrics:** Alert count, plan utilization
- **Billing:** Cost tracking via Azure Cost Management
- **Incident Success:** Check ITSM platform for created incidents

### What Customers Cannot See

- Detailed execution logs with alert payloads
- Internal workflow execution details
- Proprietary processing logic
- Deduplication cache contents

### Operational Monitoring

Cynteo monitors:
- Solution performance across all deployments
- Error rates and failures
- API latency to ITSM platforms
- Security and compliance

---

## Updates & Maintenance

### Automatic Updates

- **Minor Updates:** Applied automatically, no downtime
- **Major Updates:** Scheduled maintenance windows (rare)
- **Security Patches:** Applied immediately when critical
- **Customer Notification:** Advance notice for any changes

### Version Management

- All customers automatically updated to latest version
- Backward compatibility maintained
- No customer action required
- Changelog available at [/reference/changelog](/reference/changelog)

---

## Compliance

### Certifications

- **SOC 2 Type II** - Audited annually
- **GDPR Compliant** - Privacy by design
- **Azure Security Benchmark** - Microsoft best practices

### Data Residency

- Resources deployed to YOUR Azure region
- Data processed in your chosen region
- No cross-region data transfer (unless configured)
- Meets regional compliance requirements

---

## See Also

- [Security Overview](/reference/security) - Detailed security information
- [Incident Fields](/reference/incident-fields) - What data is sent
- [Alert Schema](/reference/alert-schema) - Input format
- [API Reference](/reference/api) - Technical integration details

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

