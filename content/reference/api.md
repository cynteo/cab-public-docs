---
title: "API Documentation"
description: "API reference for Cynteo Alert Bridge webhooks and integrations"
weight: 5
---

# API Documentation

Technical reference for integrating with Cynteo Alert Bridge.

---

## Webhook Endpoint

Alert Bridge exposes a webhook endpoint that accepts Azure Monitor alerts.

### Endpoint URL

After deployment, your Logic App provides a webhook URL:

```
https://prod-XX.region.logic.azure.com:443/workflows/{workflow-id}/triggers/manual/paths/invoke?api-version=2016-10-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig={signature}
```

**🔒 Security:** Keep this URL secret - it contains an authentication signature.

---

## Request Format

### Headers

```http
POST /workflows/{id}/triggers/manual/paths/invoke HTTP/1.1
Host: prod-XX.region.logic.azure.com
Content-Type: application/json
User-Agent: Azure-Alerts/1.0
```

### Body

Alert Bridge expects Azure Monitor Common Alert Schema:

```json
{
  "schemaId": "azureMonitorCommonAlertSchema",
  "data": {
    "essentials": {
      "alertId": "string",
      "alertRule": "string",
      "severity": "Sev0|Sev1|Sev2|Sev3|Sev4",
      "signalType": "Metric|Log|Activity Log",
      "monitorCondition": "Fired|Resolved",
      "monitoringService": "string",
      "alertTargetIDs": ["string"],
      "originAlertId": "string",
      "firedDateTime": "ISO8601 string",
      "resolvedDateTime": "ISO8601 string",
      "description": "string"
    },
    "alertContext": {
      // Alert type specific context
    }
  }
}
```

See [Alert Schema Reference](./alert-schema) for complete details.

---

## Response Format

### Success Response

```http
HTTP/1.1 202 Accepted
Content-Type: application/json

{
  "status": "accepted",
  "incidentId": "12345",
  "incidentNumber": "INC-001234",
  "action": "created|updated",
  "timestamp": "2025-10-29T10:00:00Z"
}
```

### Error Responses

#### 400 Bad Request

```json
{
  "error": {
    "code": "InvalidSchema",
    "message": "Alert schema validation failed",
    "details": "Missing required field: alertRule"
  }
}
```

#### 401 Unauthorized

```json
{
  "error": {
    "code": "InvalidSignature",
    "message": "Request signature validation failed"
  }
}
```

#### 500 Internal Server Error

```json
{
  "error": {
    "code": "SolarWindsError",
    "message": "Failed to create incident in SolarWinds",
    "details": "API returned 401 Unauthorized"
  }
}
```

---

## Rate Limits

### Azure Logic Apps Limits

| Tier | Requests/Min | Requests/Day |
|------|--------------|--------------|
| Consumption | 60 | 86,400 |
| Standard | 600 | Unlimited |

### SolarWinds API Limits

| Tier | Requests/Hour |
|------|---------------|
| Free | 100 |
| Standard | 1,000 |
| Premium | 5,000 |

**Note:** Alert Bridge automatically queues requests to stay within limits.

---

## Testing the API

### Using cURL

```bash
curl -X POST "https://your-logic-app-url" \
  -H "Content-Type: application/json" \
  -d @test-alert.json
```

### Using PowerShell

```powershell
$uri = "https://your-logic-app-url"
$body = Get-Content -Path "test-alert.json" -Raw

Invoke-RestMethod -Uri $uri -Method Post -Body $body -ContentType "application/json"
```

### Using Postman

1. Create new POST request
2. URL: Your Logic App webhook URL
3. Headers: `Content-Type: application/json`
4. Body: Paste test alert JSON
5. Send

### Test Alert JSON

```json
{
  "schemaId": "azureMonitorCommonAlertSchema",
  "data": {
    "essentials": {
      "alertId": "/subscriptions/test/providers/Microsoft.AlertsManagement/alerts/test-123",
      "alertRule": "Test Alert",
      "severity": "Sev2",
      "signalType": "Metric",
      "monitorCondition": "Fired",
      "monitoringService": "Platform",
      "alertTargetIDs": ["/subscriptions/test/resourceGroups/test/providers/Microsoft.Compute/virtualMachines/test-vm"],
      "originAlertId": "test-origin-123",
      "firedDateTime": "2025-10-29T10:00:00.0000000Z",
      "description": "This is a test alert"
    },
    "alertContext": {
      "properties": {},
      "conditionType": "SingleResourceMultipleMetricCriteria",
      "condition": {
        "allOf": [{
          "metricName": "Percentage CPU",
          "metricValue": 95.5,
          "operator": "GreaterThan",
          "threshold": "90"
        }]
      }
    }
  }
}
```

---

## Custom Integrations

### Direct API Calls

You can call the webhook from any system that can make HTTP requests:

**Node.js:**
```javascript
const axios = require('axios');

const alert = {
  schemaId: "azureMonitorCommonAlertSchema",
  data: { /* alert data */ }
};

await axios.post('https://your-logic-app-url', alert);
```

**Python:**
```python
import requests

alert = {
    "schemaId": "azureMonitorCommonAlertSchema",
    "data": { } # alert data
}

response = requests.post('https://your-logic-app-url', json=alert)
```

**Go:**
```go
import (
    "bytes"
    "net/http"
    "encoding/json"
)

alert := map[string]interface{}{
    "schemaId": "azureMonitorCommonAlertSchema",
    "data": map[string]interface{}{ /* alert data */ },
}

body, _ := json.Marshal(alert)
http.Post("https://your-logic-app-url", "application/json", bytes.NewBuffer(body))
```

---

## Webhook Security

### URL Signature

The webhook URL contains a signature (`sig` parameter) that validates requests. Do not share this URL publicly.

### IP Whitelisting

Optionally restrict access to Azure Monitor IP ranges:

```json
{
  "allowedIPs": [
    "13.66.141.0/24",
    "13.67.8.0/24",
    "13.69.64.0/24"
    // Add more Azure IP ranges
  ]
}
```

### Regenerate Signature

To rotate the webhook URL:

1. Go to Logic App → **Logic app designer**
2. Click trigger
3. Click **Regenerate**
4. Update action groups with new URL

---

## Monitoring & Logs

### View API Calls

1. Logic App → **Overview**
2. **Runs history** shows all webhook calls
3. Click a run to see:
   - Input (alert JSON)
   - Output (SolarWinds response)
   - Execution time
   - Errors

### Enable Diagnostic Logs

```bash
az monitor diagnostic-settings create \
  --name logic-app-logs \
  --resource /subscriptions/{sub}/resourceGroups/{rg}/providers/Microsoft.Logic/workflows/{name} \
  --logs '[{"category": "WorkflowRuntime", "enabled": true}]' \
  --workspace /subscriptions/{sub}/resourceGroups/{rg}/providers/Microsoft.OperationalInsights/workspaces/{workspace}
```

---

## See Also

- [Alert Schema](./alert-schema) - Input format details
- [Incident Fields](./incident-fields) - Output format
- [Configure Action Groups](../guides/configure-alert-action-group) - Setup guide

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

