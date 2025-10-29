# Contributing to Cynteo Alert Bridge

Thank you for your interest in contributing! This guide will help you add new ITSM platform integrations.

## Adding a New Platform

### Prerequisites
- Understanding of the target ITSM platform's API
- Azure subscription for testing
- Azure Functions Core Tools v4
- Python 3.12+
- Access to Cynteo Alert Bridge management plane

### Step-by-Step Guide

#### 1. Create Platform Directory (5 minutes)

```bash
mkdir -p platforms/[platform-name]/arm-templates
cd platforms/[platform-name]
```

**Create platform configuration file:**
```bash
# Create platform_config.json
cat > platform_config.json << 'EOF'
{
  "displayName": "[Platform Name]",
  "description": "Integrates Azure Monitor alerts with [Platform Name].",
  "enabled": true,
  "armTemplatePath": "platforms/[platform-name]/arm-templates/deployment_template.json",
  "createUiDefinitionPath": "platforms/[platform-name]/arm-templates/createUiDefinition.json",
  "corsOrigin": "https://[platform-name]-connector.cynteocloud.com",
  "apiBaseUrl": "https://api.[platform-name].com",
  "requiredParams": ["apiToken", "boardNumber"],
  "optionalParams": {
    "incidentCategory": "Infrastructure",
    "priorityMapping": {
      "Sev0": "High",
      "Sev1": "High",
      "Sev2": "Medium",
      "Sev3": "Low"
    }
  },
  "marketplaceOfferId": "cynteo-alert-bridge-[platform-name]"
}
EOF
```

#### 2. Build Logic App Workflow (4-6 hours)

Create `logic-app/workflow.json` with:

- **Trigger:** HTTP webhook (receives Azure Monitor alerts)
- **Actions:**
  - Parse alert JSON
  - Get ITSM credentials from Key Vault
  - Create ticket in ITSM platform
  - Report usage to Function App

**Key points:**
- Use managed identity for Key Vault access
- Handle API authentication (OAuth, API key, etc.)
- Map Azure severity to platform priority
- Report to: `https://cto-eus2-fa-cab-bbg3bxcbhkapfjdz.eastus2-01.azurewebsites.net/api/metering`

**Example usage report payload:**
```json
{
  "customerId": "@parameters('customerId')",
  "marketplaceSubscriptionId": "@parameters('marketplaceSubscriptionId')",
  "azureSubscriptionId": "@subscription().subscriptionId",
  "alertId": "@{triggerBody()?['data']?['context']?['id']}",
  "severity": "@{triggerBody()?['data']?['context']?['severity']}",
  "timestamp": "@{utcNow()}"
}
```

#### 3. Create ARM Template (2-4 hours)

Create `arm-templates/deployment_template.json` that deploys:
- Logic App
- Storage Account
- Key Vault
- Connections

**Parameters to collect:**
- Platform URL (e.g., `https://yourcompany.servicenow.com`)
- API credentials
- Board/project identifiers
- Customer ID
- Marketplace subscription ID

**Template Structure:**
```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "customerId": {"type": "string"},
    "apiToken": {"type": "securestring"},
    "boardNumber": {"type": "string"}
  },
  "resources": [
    {
      "type": "Microsoft.Logic/workflows",
      "name": "[concat('logic-', parameters('customerId'), '-[platform-name]')]",
      "properties": {
        "definition": {
          "triggers": {
            "When_a_HTTP_request_is_received": {
              "type": "Request",
              "kind": "Http"
            }
          }
        }
      }
    }
  ]
}
```

#### 4. Create UI Definition (2-3 hours)

Create `arm-templates/createUiDefinition.json` with:
- Platform credential inputs
- Validation
- Help text
- Tooltips

#### 5. Test (2-4 hours)

**Test Platform Registry:**
```bash
# Test platform discovery
curl -X GET "https://cto-eus2-fa-cab-bbg3bxcbhkapfjdz.eastus2-01.azurewebsites.net/api/debug"
```

**Test Onboard Endpoint:**
```bash
# Test platform onboarding
curl -X POST "https://cto-eus2-fa-cab-bbg3bxcbhkapfjdz.eastus2-01.azurewebsites.net/api/onboard" \
  -H "Content-Type: application/json" \
  -d '{
    "customerId": "test123",
    "platform": "[platform-name]",
    "apiToken": "test-token",
    "boardNumber": "12345"
  }'
```

**Deploy ARM Template:**
```bash
# Deploy ARM template to test subscription
az deployment group create \
  --resource-group test-rg \
  --template-file arm-templates/deployment_template.json \
  --parameters @test-parameters.json

# Trigger test alert
# Verify ticket created in ITSM platform
# Verify usage tracked in Cosmos DB
```

#### 6. Documentation (1-2 hours)

Create `README.md` with:
- Platform-specific setup instructions
- API requirements
- Troubleshooting guide
- Screenshots

#### 7. Partner Center Offer (4-6 hours)

Create marketplace offer following [docs/partner-center/PARTNER_CENTER_SETUP_GUIDE.md](docs/partner-center/PARTNER_CENTER_SETUP_GUIDE.md)

### Platform Integration Checklist

- [ ] Logic App workflow created
- [ ] ARM template deploys successfully
- [ ] UI definition tested
- [ ] Platform credentials stored securely in Key Vault
- [ ] Tickets created successfully
- [ ] Usage reporting works
- [ ] Documentation complete
- [ ] Tests pass
- [ ] Partner Center offer configured

### Code Style

- **Python:** Follow PEP 8
- **JSON:** 2-space indentation
- **ARM Templates:** Use parameters for all configurable values
- **Documentation:** Clear, concise, with examples

### Testing

All new platforms must include:
- Unit tests (if applicable)
- Integration test script
- Manual test checklist

### Pull Request Process

1. Fork the repo
2. Create feature branch: `git checkout -b platform/[platform-name]`
3. Make changes
4. Test thoroughly
5. Update documentation
6. Submit PR with:
   - Clear description
   - Test results
   - Screenshots of working integration

### Questions?

- Open an issue
- Email: support@cynteocloud.com

Thank you for contributing! 🚀
