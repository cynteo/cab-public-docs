---
title: "Cynteo Alert Bridge"
description: "Connect Azure Monitor alerts to your ITSM platform automatically"
---

# Cynteo Alert Bridge

**Connect Azure Monitor alerts to your ITSM platform automatically**

---

## 🚀 What is Cynteo Alert Bridge?

Cynteo Alert Bridge is a fully managed integration that automatically creates, updates, and resolves ITSM incidents based on your Azure Monitor alerts.

### Key Features

- ✅ **Automatic Incident Management** - Create tickets when alerts fire, update when they continue, resolve when they clear
- ✅ **Smart Deduplication** - Multiple alerts for the same issue update one ticket, not create duplicates
- ✅ **Rich Context** - Incidents include alert details, metrics, resource information, and direct links to Azure Portal
- ✅ **Configurable Mapping** - Map Azure alert severity to SolarWinds priority levels
- ✅ **Secure & Compliant** - No credentials stored, uses Azure Managed Identity and Key Vault
- ✅ **Zero Maintenance** - Fully managed solution, automatic updates included

---

## 📋 How It Works

```
Azure Monitor Alert Fires
        ↓
Cynteo Alert Bridge
        ↓
ITSM Platform Incident Created
```

When an alert fires:
1. Azure Monitor sends webhook to Cynteo Alert Bridge
2. Cynteo Alert Bridge checks if ticket already exists for this alert
3. Creates new incident OR updates existing incident
4. When alert resolves, incident is automatically marked as resolved

---

## 🎯 Perfect For

- **IT Operations Teams** - Centralize Azure alerts in your ITSM system
- **MSPs** - Manage multiple customer Azure environments
- **DevOps Teams** - Bridge monitoring and ticketing systems
- **Enterprise IT** - Ensure SLA compliance with automated ticketing

---

## 💰 Pricing

| Plan | Alerts/Month | Price/Month | Duration |
|------|-------------|-------------|----------|
| **Trial** | Up to 1,000 | Free | 30 days |
| **Basic** | Up to 1,000 | $49 | Monthly |
| **Professional** | Up to 5,000 | $149 | Monthly |
| **Business** | Up to 20,000 | $399 | Monthly |
| **Enterprise** | Unlimited | Contact Us | Custom |

**Trial Plan:** Experience all features free for 30 days. No credit card required. Automatically converts to a paid plan or expires after trial period.

**Soft Limits:** Paid plans use soft limits. You'll receive notifications at 80% and 100% usage. No service interruption.

---

## 🚀 Quick Start

### Prerequisites

Before deployment, ensure you have:
- Azure subscription with appropriate permissions
- ITSM platform account (SolarWinds Service Desk, ServiceNow, etc.)
- API credentials for your platform
  - [SolarWinds Setup Instructions](/platforms/solarwinds/setup)
  - [Other Platforms](/platforms/) (coming soon)

### Deployment Process

1. **Find Cynteo Alert Bridge** in Azure Marketplace
2. **Start deployment** and provide required information
3. **Configure settings:**
   - Azure subscription and resource group
   - ITSM platform API credentials
   - Incident preferences (category, priority mapping)
   - Contact email for notifications
4. **Deploy** (~5-10 minutes)
5. **Connect Azure Alerts** via Action Groups

**That's it!** Azure alerts will now automatically create and manage incidents in your ITSM platform.

For detailed deployment instructions, see the [Quick Start Guide](/getting-started/quickstart) and platform-specific setup guides.

---

## 📚 Documentation

### Getting Started
- [Quick Start Guide](/getting-started/quickstart)
- [MSP Deployment Guide](/getting-started/msp-deployment) - For Managed Service Providers
- [Azure Monitor Configuration](/getting-started/azure-monitor-setup)

### Platform Setup
- [SolarWinds Service Desk](/platforms/solarwinds/) - Fully supported
- [ConnectWise](/platforms/connectwise/) - Coming Q4 2025 - Q1 2026
- ServiceNow - Coming Q2 2026
- Jira Service Management - Coming Q2 2026

### Configuration Guides
- [Configure Alert Action Group](/guides/configure-alert-action-group)
- [Priority Mapping](/guides/priority-mapping)
- [Severity Filtering](/guides/severity-filtering)
- [Incident Categories](/guides/categories)

### Reference
- [Architecture Overview](/reference/architecture) - How it works
- [Incident Fields](/reference/incident-fields)
- [Alert Schema](/reference/alert-schema)
- [Configuration Options](/reference/environment-variables)
- [Security](/reference/security)
- [API Reference](/reference/api)
- [Changelog](/reference/changelog)

### Troubleshooting
- [Common Issues](/troubleshooting/common-issues)
- [Deployment Errors](/troubleshooting/deployment-errors)
- [Alert Not Creating Tickets](/troubleshooting/alert-not-creating-tickets)

---

## 🔒 Security & Privacy

- **No Data Storage** - Alert data is processed in real-time, not stored
- **Encrypted Secrets** - API tokens stored in Azure Key Vault
- **Managed Identity** - No hardcoded credentials
- **Private Deployment** - Resources deployed to YOUR subscription
- **SOC 2 Compliant** - Meets enterprise security standards

[Read our Security Overview](/reference/security)

---

## 💬 Support

- **Documentation:** [cab-docs.cynteocloud.com](https://cab-docs.cynteocloud.com)
- **Email:** support@cynteocloud.com
- **Response Time:** < 24 hours (< 4 hours for Enterprise)

---

## 🔗 Links

- [Azure Marketplace Listing](#)
- [API Documentation](/reference/api)
- [Changelog](/reference/changelog)
- [Roadmap](/reference/roadmap)

---

**Ready to get started?** [Deploy from Azure Marketplace](#) or [Contact Sales](mailto:sales@cynteocloud.com)

