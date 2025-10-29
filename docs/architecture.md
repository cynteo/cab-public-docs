# Architecture Overview

Cynteo Alert Bridge uses a **centralized management plane** with **distributed customer deployments** to provide scalable, multi-platform ITSM integration.

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│  CYNTEO TENANT (Management Plane)                       │
│                                                          │
│  ┌──────────────────┐   ┌──────────────────┐          │
│  │  Function App    │   │  Cosmos DB       │          │
│  │                  │   │                  │          │
│  │  /marketplace    │   │  MarketplaceDB   │          │
│  │  /onboard        │   │  - Tenants       │          │
│  │  /metering       │   │  - Usage         │          │
│  │  /submit-meter   │   │                  │          │
│  └──────────────────┘   └──────────────────┘          │
│                                                          │
└─────────────────────────────────────────────────────────┘
                    │
        ┌───────────┴───────────┐
        │                       │
        ▼                       ▼
┌─────────────────┐   ┌─────────────────┐
│  Customer A     │   │  Customer B     │
│                 │   │                 │
│  Logic App      │   │  Logic App      │
│  → SolarWinds   │   │  → ConnectWise  │
│                 │   │                 │
│  Storage        │   │  Storage        │
│  Key Vault      │   │  Key Vault      │
└─────────────────┘   └─────────────────┘
```

## Components

### Management Plane (Cynteo Tenant)
- **Function App:** Handles all backend operations for ALL platforms
- **Cosmos DB:** Stores tenant and usage data for ALL platforms
- **One instance serves all customers across all platforms**

### Customer Deployments (Per Customer, Per Platform)
- **Logic App:** Platform-specific ITSM integration (SolarWinds/ConnectWise/etc.)
- **Storage Account:** Workflow state
- **Key Vault:** Customer's ITSM credentials
- **Deployed in customer's tenant**

## Data Flow

1. **Alert Fires** in Azure Monitor (customer tenant)
2. **Logic App Receives** webhook
3. **Logic App Creates** ITSM ticket (platform-specific API call)
4. **Logic App Reports** usage to centralized Function App (Cynteo tenant)
5. **Function App Stores** usage in Cosmos DB
6. **Hourly Job Aggregates** and reports to Azure Marketplace

## Platform Registry System

Cynteo Alert Bridge uses a **Platform Registry System** that enables **code re-use** and **rapid platform expansion** without code changes.

### How It Works

- **Platform Registry**: Auto-discovers platform configurations from `platforms/` directory
- **Template Loader**: Dynamically loads ARM templates for specific platforms
- **Configuration-Driven**: Platform behavior defined in JSON configuration files
- **Zero Code Changes**: Adding new platforms requires only configuration files

### Key Benefits

- ✅ **Single Function App** serves all platforms (SolarWinds, ConnectWise, Jira, etc.)
- ✅ **Code Re-use** across all platforms
- ✅ **Rapid Platform Addition** through configuration
- ✅ **Platform Isolation** prevents cross-platform bugs
- ✅ **Unlimited Scalability** without code changes

## Dual-Identity Architecture

### System-Assigned Identity
- **Purpose**: Cynteo internal operations
- **Access**: Cosmos DB, Key Vault, Storage
- **Scope**: Cynteo tenant only

### User-Assigned Identity
- **Purpose**: Customer deployments
- **Access**: Deploy resources in customer subscriptions
- **Scope**: Customer tenant deployments

## Data Schema

### Tenants Container
```javascript
{
  "subscriptionId": "marketplace-uuid",
  "customerId": "customer-name",
  "platform": "solarwinds",  // or "connectwise", "jira", etc.
  "planId": "professional",
  "status": "Active",
  "deploymentId": "azure-deployment-id",
  "deploymentDate": "2025-10-20T12:00:00Z"
}
```

### Usage Container
```javascript
{
  "id": "usage-event-uuid",
  "marketplaceSubscriptionId": "marketplace-uuid",
  "azureSubscriptionId": "azure-sub-id",
  "customerId": "customer-name",
  "alertId": "/subscriptions/.../alerts/alert-1",
  "severity": "2",
  "timestamp": 1729428000,
  "reported": false,
  "reportedAt": null,
  "usageEventId": null
}
```

## Security

- **Credentials:** Customer ITSM credentials stored in their Key Vault (customer tenant)
- **Management plane:** Uses managed identity to access Cosmos DB
- **Logic Apps:** Use managed identity to access Key Vault
- **Metering:** Shared secret authenticates usage reports
- **Data residency:** Usage data in Cynteo tenant (US East US 2), customer resources in their tenant

## Monitoring

- **Application Insights:** All Function App logs and metrics
- **Cosmos DB Metrics:** RU consumption, throttling, latency
- **Logic App Runs:** Success/failure rates per customer
- **Usage Dashboard:** Track alerts by customer, platform, plan

## Cost Breakdown

**Cynteo tenant (monthly):**
- Function App (Premium P1v2): $146
- Cosmos DB (400 RU/s shared): $24
- Application Insights: $50
- **Total: ~$220/month for ALL platforms**

**Customer tenant (per deployment):**
- Logic App (Consumption): ~$5-20/month depending on alert volume
- Storage Account: ~$1/month
- Key Vault: ~$1/month
- **Customer pays via Azure subscription**

## Scalability

- **Cosmos DB:** Scale RU/s as customer base grows
- **Function App:** Auto-scales based on load
- **Logic Apps:** Consumption plan scales automatically
- **Expected:** Can handle 1,000+ customers on single management plane

## Adding a New Platform

To add a new ITSM platform:

1. **No changes to management plane** (Function App, Cosmos DB stay the same)
2. **Create platform folder:** `platforms/[platform-name]/`
3. **Create platform_config.json:** Define platform metadata and parameters
4. **Create ARM template:** Deploy Logic App to customer tenant
5. **Create UI definition:** Collect platform credentials from customer
6. **Deploy:** New platform is automatically available!

## Support

For technical support or questions about the architecture:
- **Email:** support@cynteocloud.com
- **Documentation:** https://cab-docs.cynteocloud.com
