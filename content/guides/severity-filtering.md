---
title: "Severity Filtering"
description: "Filter which alert severities create SolarWinds incidents"
weight: 3
---

# Severity Filtering

Configure Alert Bridge to only create incidents for specific severity levels.

---

## Overview

Not all alerts need a ticket. Use severity filtering to:
- Reduce ticket noise
- Focus on critical issues
- Save SolarWinds API calls
- Improve team efficiency

---

## Azure Alert Severities

Azure Monitor supports 5 severity levels:

| Severity | Description | Typical Use |
|----------|-------------|-------------|
| **Sev0** | Critical | Service outage, data loss |
| **Sev1** | Error | Major degradation, multiple users affected |
| **Sev2** | Warning | Performance issues, single user affected |
| **Sev3** | Informational | Potential issues, proactive monitoring |
| **Sev4** | Verbose | Debug info, detailed diagnostics |

---

## Filtering Options

### Option 1: Whitelist Specific Severities

Only create incidents for specific severities:

```json
{
  "SEVERITY_FILTER": "Sev0,Sev1"
}
```

**Result:** Only Sev0 and Sev1 alerts create incidents. Sev2/3/4 are ignored.

### Option 2: Minimum Severity Threshold

Create incidents for this severity and above:

```json
{
  "MIN_SEVERITY": "Sev2"
}
```

**Result:** Sev0, Sev1, and Sev2 create incidents. Sev3/4 are ignored.

### Option 3: No Filtering (Default)

All severities create incidents:

```json
{
  "SEVERITY_FILTER": null,
  "MIN_SEVERITY": null
}
```

---

## Configuration Examples

### Example 1: Critical Only

Only create incidents for service outages:

```json
{
  "SEVERITY_FILTER": "Sev0",
  "SEV0_PRIORITY": "Critical"
}
```

**Use Case:** Production environments where only critical issues need tickets.

### Example 2: Critical and High

Create incidents for major issues:

```json
{
  "SEVERITY_FILTER": "Sev0,Sev1",
  "SEV0_PRIORITY": "Critical",
  "SEV1_PRIORITY": "High"
}
```

**Use Case:** Balance between noise reduction and comprehensive tracking.

### Example 3: Warning and Above

Exclude informational alerts:

```json
{
  "MIN_SEVERITY": "Sev2"
}
```

**Use Case:** Keep informational alerts for monitoring but don't create tickets.

### Example 4: Different Filters by Environment

**Production Logic App:**
```json
{
  "SEVERITY_FILTER": "Sev0,Sev1,Sev2",
  "INCIDENT_PREFIX": "[PROD]"
}
```

**Development Logic App:**
```json
{
  "SEVERITY_FILTER": "Sev0",
  "INCIDENT_PREFIX": "[DEV]"
}
```

---

## How to Configure

### Via Azure Portal

1. **Open Logic App** in Azure Portal
2. Go to **Configuration** (under Settings)
3. Add or update filter variables:
   - `SEVERITY_FILTER` or
   - `MIN_SEVERITY`
4. Click **Save**
5. Logic App restarts automatically

### Via Azure CLI

```bash
# Set whitelist filter
az logicapp config appsettings set \
  --name your-logic-app \
  --resource-group your-rg \
  --settings "SEVERITY_FILTER=Sev0,Sev1"

# Set minimum severity
az logicapp config appsettings set \
  --name your-logic-app \
  --resource-group your-rg \
  --settings "MIN_SEVERITY=Sev2"
```

### Via ARM Template

```json
{
  "type": "Microsoft.Logic/workflows",
  "properties": {
    "parameters": {
      "SEVERITY_FILTER": {
        "value": "Sev0,Sev1"
      }
    }
  }
}
```

---

## Testing Filters

### 1. Fire Test Alerts

Create test alerts at different severities:

```bash
# Create Sev0 alert (should create incident)
az monitor metrics alert create \
  --name "test-sev0" \
  --severity 0 \
  --condition "avg Percentage CPU > 1"

# Create Sev3 alert (should be filtered)
az monitor metrics alert create \
  --name "test-sev3" \
  --severity 3 \
  --condition "avg Percentage CPU > 1"
```

### 2. Check Logic App Runs

1. Go to Logic App → **Runs history**
2. Filtered alerts should show **skipped** or **succeeded** with no SolarWinds action

### 3. Verify SolarWinds

1. Check SolarWinds for new incidents
2. Only non-filtered severities should appear

---

## Advanced Filtering

### Custom Logic App Modifications

For complex filtering requirements, modify the Logic App workflow:

#### Filter by Resource Type

```json
{
  "condition": {
    "or": [
      {
        "equals": [
          "@triggerBody()?['data']?['essentials']?['severity']",
          "Sev0"
        ]
      },
      {
        "and": [
          {"equals": ["@triggerBody()?['data']?['essentials']?['severity']", "Sev1"]},
          {"contains": ["@triggerBody()?['data']?['essentials']?['alertTargetIDs']", "virtualMachines"]}
        ]
      }
    ]
  }
}
```

**Result:** All Sev0 + Sev1 VM alerts only

#### Filter by Time of Day

Only create incidents during business hours:

```json
{
  "condition": {
    "or": [
      {"equals": ["@triggerBody()?['data']?['essentials']?['severity']", "Sev0"]},
      {
        "and": [
          {"greaterOrEquals": ["@int(formatDateTime(utcNow(), 'HH'))", 8]},
          {"lessOrEquals": ["@int(formatDateTime(utcNow(), 'HH'))", 17]}
        ]
      }
    ]
  }
}
```

**Result:** Sev0 24/7, other severities only 8 AM - 5 PM

#### Filter by Resource Tags

Only create incidents for tagged resources:

```json
{
  "condition": {
    "contains": [
      "@string(triggerBody()?['data']?['alertContext']?['properties'])",
      "CreateTicket=true"
    ]
  }
}
```

---

## Filter Strategy Best Practices

### Start Strict, Relax Later

1. **Week 1:** `Sev0` only
2. **Week 2:** Add `Sev1` if needed
3. **Week 3:** Add `Sev2` if missing important alerts

### Different Filters by Environment

| Environment | Recommended Filter |
|-------------|-------------------|
| **Production** | Sev0, Sev1, Sev2 |
| **Staging** | Sev0, Sev1 |
| **Development** | Sev0 only |
| **Test** | None (no Alert Bridge) |

### Monitor Filtered Alerts

Even if not creating tickets, monitor filtered alerts:

1. Set up Azure Monitor dashboard
2. Track Sev3/Sev4 trends
3. Adjust filters if patterns emerge

---

## Common Scenarios

### Scenario 1: Ticket Overload

**Problem:** Too many low-priority tickets

**Solution:**
```json
{
  "SEVERITY_FILTER": "Sev0,Sev1"
}
```

### Scenario 2: Missing Critical Alerts

**Problem:** Some critical alerts not creating tickets

**Solution:** Check that alerts are marked Sev0 or Sev1 in Azure

### Scenario 3: After-Hours Noise

**Problem:** Too many tickets outside business hours

**Solution:** Use time-based filtering (see Advanced Filtering above)

---

## Troubleshooting

### Filter Not Working

1. **Check syntax** - Comma-separated, no spaces: `"Sev0,Sev1"`
2. **Case-sensitive** - Use `Sev0` not `sev0` or `SEV0`
3. **Restart Logic App** - Disable/enable to clear cache

### All Alerts Still Creating Tickets

1. Verify filter variable exists in Logic App config
2. Check Logic App run history shows filter being applied
3. Ensure no typos: `SEVERITY_FILTER` not `SEVERITYFILTER`

### Wrong Alerts Filtered

1. Check Azure alert rule severity configuration
2. Verify severity in Logic App run history inputs
3. Confirm filter matches intended severity values

---

## See Also

- [Priority Mapping](./priority-mapping) - Map severities to priorities
- [Environment Variables](../reference/environment-variables) - All config options
- [Azure Monitor Setup](../getting-started/azure-monitor-setup) - Alert configuration

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

