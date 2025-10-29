# Quick Start Guide

Get up and running with Cynteo Alert Bridge in under 10 minutes.

## Prerequisites

- Azure subscription with Contributor permissions
- ITSM platform credentials (SolarWinds, ConnectWise, etc.)
- Azure Monitor alerts configured

## Step 1: Deploy from Azure Marketplace

1. **Search Azure Marketplace**
   - Go to [Azure Portal](https://portal.azure.com)
   - Search for "Cynteo Alert Bridge"
   - Select your ITSM platform (SolarWinds, ConnectWise, etc.)

2. **Configure Deployment**
   - Select your Azure subscription
   - Choose a resource group
   - Enter your ITSM credentials
   - Review and accept terms

3. **Deploy**
   - Click "Create" to start deployment
   - Wait 5-10 minutes for resources to be created

## Step 2: Configure ITSM Credentials

### **SolarWinds Service Desk**
1. **Get API Token**
   - Log into SolarWinds Service Desk
   - Go to Settings → API
   - Generate a new API token
   - Copy the token

2. **Get Board Number**
   - Go to Settings → Boards
   - Note the board number for incidents

3. **Configure in Azure**
   - Enter API token in deployment parameters
   - Enter board number
   - Save configuration

### **ConnectWise Manage** (Coming Q1 2026)
1. **Get API Key**
   - Log into ConnectWise Manage
   - Go to System → API Keys
   - Generate a new API key
   - Copy the key

2. **Get Company ID**
   - Go to System → Company
   - Note the company ID

3. **Configure in Azure**
   - Enter API key in deployment parameters
   - Enter company ID
   - Save configuration

## Step 3: Set Up Azure Monitor Alerts

1. **Create Alert Rule**
   - Go to Azure Monitor → Alerts
   - Click "Create" → "Alert rule"
   - Select your resource
   - Configure conditions

2. **Configure Action Group**
   - Create new action group
   - Add webhook action
   - Use the Logic App webhook URL from deployment

3. **Test Alert**
   - Trigger a test alert
   - Verify ticket creation in ITSM platform

## Step 4: Verify Integration

### **Check Logic App**
1. Go to Azure Portal → Logic Apps
2. Find your deployed Logic App
3. Check run history for successful executions

### **Check ITSM Platform**
1. Log into your ITSM platform
2. Look for new tickets created from Azure alerts
3. Verify ticket details and priority mapping

## Troubleshooting

### **Common Issues**

#### **Deployment Fails**
- Check Azure subscription permissions
- Verify ITSM credentials are correct
- Ensure resource group exists

#### **Logic App Not Triggered**
- Verify webhook URL is correct
- Check action group configuration
- Test webhook manually

#### **Tickets Not Created**
- Verify ITSM credentials in Key Vault
- Check Logic App run history for errors
- Verify API permissions in ITSM platform

### **Get Help**
- **Email:** support@cynteocloud.com
- **Documentation:** https://cab-docs.cynteocloud.com
- **Issues:** [GitHub Issues](https://github.com/cynteo/cynteo-alert-bridge-docs/issues)

## Next Steps

- [Platform-Specific Setup](platforms/)
- [Troubleshooting Guide](troubleshooting)
- [API Reference](api-reference)
- [Architecture Overview](architecture)

## Support

For technical support or questions:
- **Email:** support@cynteocloud.com
- **Documentation:** https://cab-docs.cynteocloud.com
