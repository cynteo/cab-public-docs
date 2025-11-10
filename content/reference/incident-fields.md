---
title: "Incident Fields Overview"
description: "Understanding what data Cynteo Alert Bridge sends to your ITSM platform"
weight: 1
---

# Incident Fields Overview

Learn what data Cynteo Alert Bridge extracts from Azure alerts and sends to your ITSM platform.

---

## Standard Fields

Cynteo Alert Bridge maps Azure Monitor alert data to standard ITSM incident fields:

| Alert Data | Incident Field | Description |
|------------|----------------|-------------|
| Alert rule name | Title/Name | Incident title |
| Alert severity | Priority | Mapped to platform priority levels |
| Alert details + metrics | Description | Rich formatted description |
| Alert category | Category | Incident categorization |
| Alert state | State/Status | New, In Progress, Resolved |

---

## Alert Information Included

Every incident contains comprehensive alert context:

### Alert Identification
- Alert name and rule
- Alert ID (for deduplication)
- When alert fired
- When alert resolved (if applicable)

### Resource Information
- Affected Azure resource(s)
- Resource type (VM, database, app service, etc.)
- Resource group and subscription
- Resource location/region

### Alert Details
- Severity level (Sev0-Sev4)
- Signal type (Metric, Log, Activity Log)
- Alert condition and threshold
- Current metric value (for metric alerts)
- Alert description and context

### Links and References
- Direct link to Azure Portal alert
- Link to affected resource
- Link to metric charts (if applicable)

---

## Platform-Specific Field Mapping

For detailed field mapping specific to your ITSM platform:

- **[SolarWinds Service Desk](/platforms/solarwinds/incident-fields)** - Complete SolarWinds field reference
- **ServiceNow** - Coming soon
- **Jira Service Management** - Coming soon

---

## Customization

During deployment, you can configure:

- **Priority Mapping** - How alert severities map to platform priorities
- **Category Structure** - Custom categories and subcategories
- **Assignee/Group** - Auto-assignment rules
- **Requester** - Default requester or reporter

Contact [support@cynteocloud.com](mailto:support@cynteocloud.com) for configuration details.

---

## Data Privacy

Cynteo Alert Bridge only sends metadata and metrics:

✅ **What IS sent:**
- Alert details and metrics
- Resource names and IDs
- Resource tags and properties
- Diagnostic information

❌ **What is NOT sent:**
- Azure subscription credentials
- Resource access keys or secrets
- Connection strings
- VM passwords
- Storage account keys
- Key Vault secrets

See [Security Overview](/reference/security) for more information.

---

## See Also

- [Alert Schema](/reference/alert-schema) - Input from Azure Monitor
- [Architecture Overview](/reference/architecture) - How it works
- [Security](/reference/security) - Data privacy and security

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)
