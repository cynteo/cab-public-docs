# Troubleshooting Guide

Common issues and solutions for Cynteo Alert Bridge.

## Deployment Issues

### **Deployment Fails**

#### **Symptoms**
- ARM template deployment fails
- Resources not created
- Error messages in deployment logs

#### **Solutions**
1. **Check Azure Permissions**
   ```bash
   # Verify you have Contributor access
   az role assignment list --assignee <your-email> --scope /subscriptions/<subscription-id>
   ```

2. **Verify Resource Group**
   ```bash
   # Check if resource group exists
   az group show --name <resource-group-name>
   ```

3. **Check ITSM Credentials**
   - Verify API token is valid and not expired
   - Ensure API token has required permissions
   - Test API connection manually

4. **Review Deployment Logs**
   ```bash
   # Get deployment logs
   az deployment group show --name <deployment-name> --resource-group <resource-group-name>
   ```

### **Resources Created But Not Working**

#### **Symptoms**
- Resources deployed successfully
- Logic App not triggering
- No tickets created in ITSM platform

#### **Solutions**
1. **Check Logic App Status**
   - Ensure Logic App is enabled
   - Verify webhook URL is correct
   - Check Logic App run history

2. **Verify Key Vault Secrets**
   - Check if ITSM credentials are stored correctly
   - Verify managed identity has access to Key Vault
   - Test secret retrieval

3. **Test Webhook Manually**
   ```bash
   # Test webhook endpoint
   curl -X POST <webhook-url> \
        -H "Content-Type: application/json" \
        -d '{"test": "data"}'
   ```

## Logic App Issues

### **Logic App Not Triggered**

#### **Symptoms**
- Azure Monitor alerts fire but Logic App doesn't run
- No runs in Logic App history
- Webhook not receiving requests

#### **Solutions**
1. **Check Action Group Configuration**
   - Verify webhook URL is correct
   - Ensure action group is linked to alert rule
   - Test webhook connection

2. **Verify Alert Rule**
   - Check if alert rule is enabled
   - Verify alert conditions are met
   - Test alert rule manually

3. **Check Logic App Webhook**
   - Ensure webhook is enabled
   - Verify webhook URL format
   - Test webhook with sample payload

### **Logic App Runs But Fails**

#### **Symptoms**
- Logic App triggers but fails during execution
- Error messages in run history
- Tickets not created in ITSM platform

#### **Solutions**
1. **Check Run History**
   - Go to Logic App → Run history
   - Click on failed run to see details
   - Review error messages

2. **Verify ITSM Credentials**
   - Check if API token is valid
   - Verify API permissions
   - Test API connection

3. **Check ITSM API Limits**
   - Verify API rate limits
   - Check if API is available
   - Review ITSM platform status

## ITSM Platform Issues

### **Tickets Not Created**

#### **Symptoms**
- Logic App runs successfully
- No tickets appear in ITSM platform
- API calls fail

#### **Solutions**
1. **Verify API Credentials**
   ```bash
   # Test SolarWinds API
   curl -H "Authorization: Bearer <api-token>" \
        https://api.samanage.com/incidents.json
   ```

2. **Check API Permissions**
   - Ensure API token has create incident permissions
   - Verify board access permissions
   - Check user permissions in ITSM platform

3. **Review API Documentation**
   - Check ITSM platform API documentation
   - Verify required fields and format
   - Test API calls manually

### **Incorrect Ticket Details**

#### **Symptoms**
- Tickets created but with wrong information
- Missing or incorrect priority mapping
- Wrong assignee or category

#### **Solutions**
1. **Check Priority Mapping**
   - Verify priority mapping configuration
   - Update Logic App workflow if needed
   - Test with different severity levels

2. **Verify Field Mapping**
   - Check if all required fields are mapped
   - Verify field names and formats
   - Update Logic App workflow

3. **Review ITSM Configuration**
   - Check ITSM platform settings
   - Verify board configuration
   - Update field mappings

## Permission Issues

### **Managed Identity Permissions**

#### **Symptoms**
- Access denied errors
- Cannot access Key Vault
- Cannot deploy resources

#### **Solutions**
1. **Check Key Vault Access**
   ```bash
   # Verify managed identity has Key Vault access
   az keyvault show --name <key-vault-name> --query "accessPolicies"
   ```

2. **Grant Required Permissions**
   ```bash
   # Grant Key Vault access
   az keyvault set-policy --name <key-vault-name> \
       --object-id <managed-identity-object-id> \
       --secret-permissions get list
   ```

3. **Verify Subscription Permissions**
   - Check if managed identity has Contributor role
   - Verify subscription-level permissions
   - Review role assignments

### **Customer Tenant Permissions**

#### **Symptoms**
- Cannot deploy to customer tenant
- Access denied when creating resources
- Permission errors during deployment

#### **Solutions**
1. **Check Marketplace Permissions**
   - Verify marketplace subscription is active
   - Check if publisher identity has access
   - Review marketplace permissions

2. **Verify Customer Subscription**
   - Ensure customer subscription is valid
   - Check if subscription has required permissions
   - Review subscription status

## Network Issues

### **Webhook Not Accessible**

#### **Symptoms**
- Webhook URL not reachable
- Timeout errors
- Connection refused

#### **Solutions**
1. **Check Network Connectivity**
   ```bash
   # Test webhook connectivity
   curl -I <webhook-url>
   ```

2. **Verify Firewall Rules**
   - Check if webhook is blocked by firewall
   - Review network security groups
   - Update firewall rules if needed

3. **Check Logic App Status**
   - Ensure Logic App is running
   - Verify webhook is enabled
   - Check Logic App health

### **API Connection Issues**

#### **Symptoms**
- Cannot connect to ITSM API
- Timeout errors
- SSL/TLS issues

#### **Solutions**
1. **Check API Endpoint**
   - Verify API URL is correct
   - Test API endpoint manually
   - Check if API is available

2. **Review SSL/TLS Settings**
   - Check if SSL certificates are valid
   - Verify TLS version compatibility
   - Update SSL settings if needed

3. **Check Network Configuration**
   - Review network security groups
   - Check if outbound connections are allowed
   - Verify DNS resolution

## Performance Issues

### **Slow Response Times**

#### **Symptoms**
- Logic App takes long time to execute
- Delayed ticket creation
- Timeout errors

#### **Solutions**
1. **Check Logic App Performance**
   - Review Logic App run duration
   - Check for bottlenecks in workflow
   - Optimize Logic App workflow

2. **Verify ITSM API Performance**
   - Check ITSM platform status
   - Review API response times
   - Contact ITSM platform support

3. **Review Azure Resources**
   - Check if resources are under load
   - Review resource utilization
   - Scale resources if needed

### **High Resource Usage**

#### **Symptoms**
- High CPU/memory usage
- Slow performance
- Resource throttling

#### **Solutions**
1. **Monitor Resource Usage**
   ```bash
   # Check resource utilization
   az monitor metrics list --resource <resource-id> --metric "CPU Percentage"
   ```

2. **Optimize Logic App**
   - Review Logic App workflow
   - Remove unnecessary steps
   - Optimize API calls

3. **Scale Resources**
   - Increase Logic App plan
   - Scale storage account
   - Review Cosmos DB throughput

## Getting Help

### **Support Channels**
- **Email:** support@cynteocloud.com
- **Documentation:** https://docs.cynteocloud.com
- **GitHub Issues:** [Report Issues](https://github.com/cynteo/cynteo-alert-bridge-docs/issues)

### **Information to Include**
When contacting support, include:
- Error messages and logs
- Steps to reproduce the issue
- Azure subscription details
- ITSM platform information
- Screenshots if applicable

### **Self-Service Resources**
- [Quick Start Guide](quick-start.md)
- [Platform Setup Guides](platforms/)
- [API Reference](api-reference.md)
- [Architecture Overview](architecture.md)
