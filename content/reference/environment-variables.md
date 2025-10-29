---
title: "Environment Variables"
description: "Configuration options and environment variables for Cynteo Alert Bridge"
weight: 4
---

# Environment Variables

Configuration reference for customizing Cynteo Alert Bridge behavior.

---

## Required Variables

These are set during deployment and must be configured:

| Variable | Type | Description | Example |
|----------|------|-------------|---------|
| `SOLARWINDS_API_TOKEN` | Secret | SolarWinds API token | `Bearer abc123...` |
| `SOLARWINDS_BASE_URL` | String | SolarWinds instance URL | `https://api.samanage.com` |
| `REQUESTER_EMAIL` | String | Default requester email | `azure@company.com` |

---

## Optional Variables

Customize incident creation behavior:

### Incident Mapping

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `INCIDENT_CATEGORY` | String | `"Infrastructure"` | Default category name |
| `INCIDENT_SUBCATEGORY` | String | `"Azure Monitor"` | Default subcategory |
| `ASSIGNEE_GROUP` | String | `null` | Auto-assign to team |
| `INCIDENT_PREFIX` | String | `"Azure Alert:"` | Prefix for incident names |

### Priority Mapping

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `SEV0_PRIORITY` | String | `"Critical"` | Priority for Sev0 |
| `SEV1_PRIORITY` | String | `"High"` | Priority for Sev1 |
| `SEV2_PRIORITY` | String | `"Medium"` | Priority for Sev2 |
| `SEV3_PRIORITY` | String | `"Low"` | Priority for Sev3 |
| `SEV4_PRIORITY` | String | `"Low"` | Priority for Sev4 |

### Filtering

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `SEVERITY_FILTER` | String | `null` | Only process these severities (comma-separated) |
| `IGNORE_RESOLVED` | Boolean | `false` | Don't update incidents on resolution |
| `MIN_SEVERITY` | String | `null` | Minimum severity to process |

### Advanced Options

| Variable | Type | Default | Description |
|----------|------|---------|-------------|
| `DEDUP_WINDOW_HOURS` | Number | `24` | How long to track alerts for dedup |
| `MAX_DESCRIPTION_LENGTH` | Number | `32000` | Truncate descriptions |
| `TIMEZONE` | String | `"UTC"` | Timezone for timestamps |
| `ENABLE_DEBUG_LOGGING` | Boolean | `false` | Verbose logging |

---

## Configuration Examples

### Example 1: Custom Priority Mapping

```json
{
  "SEV0_PRIORITY": "Critical",
  "SEV1_PRIORITY": "High",
  "SEV2_PRIORITY": "Medium",
  "SEV3_PRIORITY": "Low",
  "SEV4_PRIORITY": "Low"
}
```

### Example 2: Severity Filtering

Only create incidents for critical and high severity:

```json
{
  "SEVERITY_FILTER": "Sev0,Sev1"
}
```

Or use minimum severity:

```json
{
  "MIN_SEVERITY": "Sev2"
}
```

### Example 3: Custom Categories

```json
{
  "INCIDENT_CATEGORY": "Cloud Services",
  "INCIDENT_SUBCATEGORY": "Azure Monitoring",
  "ASSIGNEE_GROUP": "Cloud Operations Team"
}
```

### Example 4: Multiple Environments

For different environments (dev/staging/prod), use different prefixes:

**Production:**
```json
{
  "INCIDENT_PREFIX": "[PROD] Azure Alert:",
  "ASSIGNEE_GROUP": "Production Support"
}
```

**Staging:**
```json
{
  "INCIDENT_PREFIX": "[STAGING] Azure Alert:",
  "ASSIGNEE_GROUP": "Dev Team"
}
```

---

## How to Update Variables

### Via Azure Portal

1. Go to Logic App resource
2. Click **Configuration** (under Settings)
3. Find the variable
4. Update the value
5. Click **Save**
6. Logic App will restart automatically

### Via Azure CLI

```bash
az logicapp config appsettings set \
  --name your-logic-app \
  --resource-group your-rg \
  --settings "SEV1_PRIORITY=Critical"
```

### Via ARM Template

```json
{
  "type": "Microsoft.Logic/workflows",
  "properties": {
    "parameters": {
      "SEV1_PRIORITY": {
        "value": "Critical"
      }
    }
  }
}
```

---

## Validation

### Check Current Configuration

1. Go to Logic App
2. **Overview** → **JSON View**
3. Look for `parameters` section
4. Verify values

### Test Changes

After updating variables:

1. Trigger a test alert
2. Check Logic App run history
3. Verify incident created correctly
4. Confirm new values applied

---

## Security Considerations

### Secret Management

- ❌ **Never** commit API tokens to git
- ✅ **Always** use Key Vault references
- ✅ Use Managed Identity when possible

### Key Vault Integration

Reference secrets from Key Vault:

```json
{
  "SOLARWINDS_API_TOKEN": "@Microsoft.KeyVault(SecretUri=https://your-vault.vault.azure.net/secrets/solarwinds-token/)"
}
```

---

## Troubleshooting

### Variable Not Taking Effect

1. **Check Syntax** - Ensure correct JSON format
2. **Restart Logic App** - May need manual restart
3. **Clear Cache** - Disable/enable Logic App
4. **Check Run History** - Verify new value in inputs

### Invalid Values

Common mistakes:

| Issue | Solution |
|-------|----------|
| Priority not recognized | Must match SolarWinds values exactly |
| Category not found | Create category in SolarWinds first |
| Boolean as string | Use `true`/`false` not `"true"`/`"false"` |
| Timezone invalid | Use IANA timezone names |

---

## See Also

- [Priority Mapping Guide](../guides/priority-mapping) - Detailed mapping options
- [Severity Filtering Guide](../guides/severity-filtering) - Filter configuration
- [SolarWinds Setup](../getting-started/solarwinds-setup) - Category creation

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

