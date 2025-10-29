# SolarWinds Service Desk Setup

Complete setup guide for integrating Azure Monitor alerts with SolarWinds Service Desk.

## Prerequisites

- SolarWinds Service Desk account with API access
- Azure subscription with Contributor permissions
- Azure Monitor alerts configured

## Step 1: Get SolarWinds Credentials

### **API Token**
1. **Log into SolarWinds Service Desk**
   - Go to your SolarWinds Service Desk instance
   - Log in with your administrator account

2. **Navigate to API Settings**
   - Go to Settings → API
   - Click "Generate New Token"
   - Copy the generated token (save it securely)

3. **Verify API Access**
   - Test the token with a simple API call
   - Ensure you have permissions to create incidents

### **Board Number**
1. **Go to Boards Settings**
   - Navigate to Settings → Boards
   - Find the board you want to use for incidents
   - Note the board number

2. **Verify Board Permissions**
   - Ensure your API token has access to this board
   - Test creating a test incident

## Step 2: Deploy from Azure Marketplace

1. **Search for Cynteo Alert Bridge**
   - Go to [Azure Portal](https://portal.azure.com)
   - Search for "Cynteo Alert Bridge SolarWinds"
   - Click "Get It Now"

2. **Configure Deployment**
   - **Subscription:** Select your Azure subscription
   - **Resource Group:** Choose or create a resource group
   - **Region:** Select your preferred region
   - **Customer ID:** Enter a unique identifier for your organization
   - **API Token:** Enter your SolarWinds API token
   - **Board Number:** Enter your SolarWinds board number

3. **Review and Deploy**
   - Review all configuration settings
   - Accept the terms and conditions
   - Click "Create" to start deployment

## Step 3: Configure Azure Monitor Alerts

### **Create Alert Rule**
1. **Go to Azure Monitor**
   - Navigate to Azure Monitor → Alerts
   - Click "Create" → "Alert rule"

2. **Select Resource**
   - Choose the Azure resource to monitor
   - Select the appropriate scope

3. **Configure Conditions**
   - Set up alert conditions (CPU, memory, etc.)
   - Configure severity levels
   - Set up frequency and duration

### **Configure Action Group**
1. **Create Action Group**
   - Go to Azure Monitor → Action Groups
   - Click "Create" → "Action group"

2. **Add Webhook Action**
   - Click "Add" → "Webhook"
   - Enter the Logic App webhook URL from deployment
   - Test the webhook connection

3. **Link to Alert Rule**
   - Select the action group in your alert rule
   - Save the alert rule

## Step 4: Test Integration

### **Trigger Test Alert**
1. **Create Test Alert**
   - Manually trigger an alert condition
   - Or use Azure Monitor's test alert feature

2. **Verify Logic App Execution**
   - Go to Azure Portal → Logic Apps
   - Find your deployed Logic App
   - Check the run history for successful execution

3. **Check SolarWinds**
   - Log into SolarWinds Service Desk
   - Look for the new incident
   - Verify incident details and priority

## Configuration Options

### **Priority Mapping**
Configure how Azure Monitor severity maps to SolarWinds priority:

| Azure Severity | SolarWinds Priority | Default |
|----------------|-------------------|---------|
| Sev0 | Critical | High |
| Sev1 | High | High |
| Sev2 | Medium | Medium |
| Sev3 | Low | Low |

### **Incident Category**
Set the default category for created incidents:
- **Default:** Infrastructure
- **Custom:** Set your preferred category

### **Assignee Group**
Configure automatic assignment:
- **Default:** None (manual assignment)
- **Custom:** Set your preferred assignee group

## Troubleshooting

### **Common Issues**

#### **Deployment Fails**
- **Check Azure Permissions:** Ensure you have Contributor access
- **Verify Resource Group:** Make sure the resource group exists
- **Check ITSM Credentials:** Verify API token and board number

#### **Logic App Not Triggered**
- **Verify Webhook URL:** Check the webhook URL in action group
- **Test Webhook:** Use a tool like Postman to test the webhook
- **Check Logic App Status:** Ensure the Logic App is enabled

#### **Tickets Not Created**
- **Verify API Token:** Check if the token has expired
- **Check Board Permissions:** Ensure API token can access the board
- **Review Logic App Logs:** Check for specific error messages

#### **Incorrect Priority Mapping**
- **Check Configuration:** Verify priority mapping settings
- **Update Logic App:** Modify the Logic App workflow if needed

### **Debug Steps**

1. **Check Logic App Run History**
   ```bash
   # View Logic App runs
   az logic workflow run list --resource-group <rg-name> --workflow-name <logic-app-name>
   ```

2. **Test SolarWinds API**
   ```bash
   # Test API connection
   curl -H "Authorization: Bearer <your-api-token>" \
        https://api.samanage.com/incidents.json
   ```

3. **Verify Webhook URL**
   ```bash
   # Test webhook endpoint
   curl -X POST <webhook-url> \
        -H "Content-Type: application/json" \
        -d '{"test": "data"}'
   ```

## Advanced Configuration

### **Custom Fields**
Configure custom fields in SolarWinds:
1. **Set up Custom Fields** in SolarWinds Service Desk
2. **Update Logic App** to include custom field values
3. **Test Custom Field** mapping

### **Bidirectional Sync**
Enable two-way synchronization:
1. **Configure Webhook** in SolarWinds for status updates
2. **Update Logic App** to handle status changes
3. **Test Status Sync** by updating incident status

### **Multiple Boards**
Support multiple SolarWinds boards:
1. **Create Multiple Deployments** for different boards
2. **Configure Alert Rules** to route to appropriate boards
3. **Set up Board-Specific** Logic Apps

## Support

For technical support or questions about SolarWinds integration:
- **Email:** support@cynteocloud.com
- **Documentation:** https://docs.cynteocloud.com
- **SolarWinds API:** [SolarWinds Service Desk API Documentation](https://help.samanage.com/api)

## Related Documentation

- [Quick Start Guide](../quick-start.md)
- [Troubleshooting Guide](../troubleshooting.md)
- [API Reference](../api-reference.md)
- [Architecture Overview](../architecture.md)
