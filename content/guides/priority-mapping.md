---
title: "Priority Mapping"
description: "Configure how Azure alert severities map to ITSM platform priorities"
weight: 2
---

# Priority Mapping

Learn how to map Azure Monitor alert severities to your ITSM platform's priority levels.

---

## Overview

Azure Monitor uses **severities** (Sev0-Sev4), while most ITSM platforms use **priorities** (e.g., Critical, High, Medium, Low). Cynteo Alert Bridge maps between these automatically.

---

## Default Mapping

Out of the box, Cynteo Alert Bridge uses this mapping:

| Azure Severity | Default Priority | Description |
|----------------|---------------------|-------------|
| **Sev0** | Critical | System down, immediate action required |
| **Sev1** | High | Major functionality impaired |
| **Sev2** | Medium | Moderate impact, workaround available |
| **Sev3** | Low | Minor issue, minimal impact |
| **Sev4** | Low | Informational, no immediate action |

---

## Customizing Priority Mapping

Priority mapping is configured during deployment. The solution can be customized to match your ITSM platform's priority structure.

**To request a priority mapping change**, contact [support@cynteocloud.com](mailto:support@cynteocloud.com) with your desired mapping.

---

## ITSM Priority Values

Your ITSM platform may have different priority names. Common priority structures include:

**Standard Priorities:**
- Critical / P1
- High / P2
- Medium / P3
- Low / P4

Your organization may use custom priority names. The mapping is configured to match your specific platform's priority structure during deployment.

---

## Custom Priority Examples

Cynteo Alert Bridge supports various priority mapping strategies:

### Example 1: Escalated Priorities
All critical and error alerts mapped to high priority for faster response.

### Example 2: Custom Priority Names
Organizations using custom priority names (e.g., "P1 - Critical", "P2 - Urgent") can configure the mapping accordingly.

### Example 3: Simplified Mapping
Some organizations prefer to use only two priority levels (Critical and Normal) for simplicity.

### Example 4: Environment-Specific
Different environments (Production vs Development) can use different priority mappings to reflect business impact.

---

## Verifying Priority Mapping

When an Azure alert fires, you can verify the priority mapping worked correctly:

### Check the ITSM Incident

1. Open your ITSM platform
2. Find the incident created from the alert
3. Verify the priority field matches expectations

For example, an Azure Sev1 alert should create a High priority incident (based on default mapping).

---

## Priority-Based Routing

Different priority levels can trigger different response workflows in your ITSM platform:

### Auto-Assignment Rules

Most ITSM platforms support automatic assignment based on priority:
   - **Critical Priority** → Route to On-Call Team
   - **High Priority** → Route to Infrastructure Team
   - **Medium/Low Priority** → Route to Operations Team

This ensures the right team receives alerts based on severity.

---

## Advanced Priority Scenarios

Cynteo Alert Bridge supports advanced priority mapping strategies including:

- **Resource Type Based** - Different resources can have different priority mappings
- **Environment Based** - Production alerts prioritized higher than development
- **Time-Based** - After-hours alerts automatically escalated
- **Tag-Based** - Resource tags influence priority assignment

Contact [support@cynteocloud.com](mailto:support@cynteocloud.com) to discuss custom mapping requirements.

---

## Platform-Specific Details

For detailed priority configuration for your platform:
- **[SolarWinds Priority Mapping](/platforms/solarwinds/categories)** - SolarWinds-specific details

---

## See Also

- [Configuration Options](/reference/environment-variables) - Available settings
- [Severity Filtering](/guides/severity-filtering) - Filter by severity
- [Incident Categories](/guides/categories) - Category configuration

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

