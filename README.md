# Cynteo Alert Bridge Documentation

Welcome to the Cynteo Alert Bridge documentation! This site provides everything you need to deploy, configure, and troubleshoot your Azure Monitor to SolarWinds Service Desk integration.

---

## 🚀 Quick Links

- **[Get Started](./getting-started/quickstart)** - Deploy in 15 minutes
- **[Troubleshooting](./troubleshooting/common-issues)** - Fix common issues
- **[All Guides](./guides/)** - Configuration and customization

---

## 📚 Documentation Structure

### Getting Started
New to Alert Bridge? Start here!

- **[Quick Start Guide](./getting-started/quickstart)** - Complete deployment walkthrough
- **[SolarWinds Setup](./getting-started/solarwinds-setup)** - Generate API token and configure SolarWinds
- **[Azure Monitor Setup](./getting-started/azure-monitor-setup)** - Configure alerts and action groups

### Configuration Guides
Customize your integration.

- **[Configure Alert Action Group](./guides/configure-alert-action-group)** - Set up Azure Monitor action groups
- **[Priority Mapping](./guides/priority-mapping)** - Map severity to priority
- **[Severity Filtering](./guides/severity-filtering)** - Ignore specific alert levels
- **[Custom Categories](./guides/custom-categories)** - Use your SolarWinds categories

### Reference
Technical details and specifications.

- **[Incident Fields](./reference/incident-fields)** - What data gets sent to SolarWinds
- **[Alert Schema](./reference/alert-schema)** - Common Alert Schema explained
- **[Environment Variables](./reference/environment-variables)** - Configuration options
- **[Security](./reference/security)** - Security architecture and compliance

### Troubleshooting
Solve problems quickly.

- **[Common Issues](./troubleshooting/common-issues)** - Most frequent problems and solutions
- **[Deployment Errors](./troubleshooting/deployment-errors)** - Fix deployment failures
- **[Alert Not Creating Tickets](./troubleshooting/alert-not-creating-tickets)** - Debug incident creation

---

## 🎯 Common Tasks

### I want to...

**Deploy Alert Bridge**  
→ [Quick Start Guide](./getting-started/quickstart)

**Get my SolarWinds API token**  
→ [SolarWinds Setup](./getting-started/solarwinds-setup.md#generating-api-token)

**Connect my alerts to SolarWinds**  
→ [Configure Action Group](./guides/configure-alert-action-group)

**Change priority mapping**  
→ [Priority Mapping Guide](./guides/priority-mapping)

**Ignore low-severity alerts**  
→ [Severity Filtering](./guides/severity-filtering)

**Test my integration**  
→ [Quick Start - Step 4](./getting-started/quickstart.md#step-4-test-it)

**Fix alerts not creating tickets**  
→ [Troubleshooting: Alerts Not Creating Incidents](./troubleshooting/common-issues.md#alerts-not-creating-incidents)

**Update my API token**  
→ [API Token Rotation](./troubleshooting/common-issues.md#api-token-rotation)

---

## 💡 How Alert Bridge Works

```
┌──────────────────┐
│ Azure Resource   │ (e.g., Virtual Machine)
└────────┬─────────┘
         │
         │ Metric exceeds threshold
         ↓
┌──────────────────┐
│ Azure Monitor    │
│ Alert Rule       │
└────────┬─────────┘
         │
         │ Webhook
         ↓
┌──────────────────┐
│ Action Group     │
│ (Common Schema)  │
└────────┬─────────┘
         │
         │ HTTPS POST
         ↓
┌──────────────────┐
│ Alert Bridge     │
│ Logic App        │
│                  │
│ • Deduplication  │
│ • Enrichment     │
│ • Mapping        │
└────────┬─────────┘
         │
         │ SolarWinds API
         ↓
┌──────────────────┐
│ SolarWinds       │
│ Service Desk     │
│                  │
│ Incident Created │
└──────────────────┘
```

**Key Features:**
1. **Smart Deduplication** - Same alert updates one ticket
2. **Auto Resolution** - Tickets close when alerts clear
3. **Rich Context** - Incidents include metrics, links, history
4. **Configurable** - Map severity, filter, customize

---

## 📋 Requirements

### Azure

- Azure subscription
- Contributor role (to deploy resources)
- Azure Monitor alerts configured
- At least one resource being monitored

### SolarWinds

- SolarWinds Service Desk (Samanage) account
- API access enabled
- Permission to generate API tokens
- Valid requester email

---

## 💰 Pricing

### Plans

| Plan | Alerts/Month | Price |
|------|-------------|-------|
| Starter | 1,000 | $49/mo |
| Professional | 5,000 | $149/mo |
| Business | 20,000 | $399/mo |
| Enterprise | Unlimited | Custom |

### Free Trial

- **30 days** free trial
- All features included
- No credit card required
- Upgrade anytime

### What Counts as an Alert?

Each unique alert that fires counts as one alert:
- Alert fires → 1 alert
- Same alert fires again → 1 alert
- Alert resolves → 0 additional alerts

**Example:** Alert fires 10 times in a month = 10 alerts

---

## 🔒 Security

Alert Bridge is designed with security in mind:

- ✅ **No Data Storage** - Processes alerts in real-time
- ✅ **Encrypted Secrets** - API tokens in Azure Key Vault
- ✅ **Managed Identity** - No hardcoded credentials
- ✅ **Private Deployment** - Runs in your subscription
- ✅ **Audit Logs** - Full trace of all operations
- ✅ **SOC 2 Compliant** - Enterprise-ready

[Read Security Details](./reference/security)

---

## 🆘 Getting Help

### Documentation

Most questions are answered in our docs:
- [Getting Started](./getting-started/)
- [Guides](./guides/)
- [Troubleshooting](./troubleshooting/)

### Support

**Email:** support@cynteocloud.com

**Response Times:**
- Starter/Professional: < 24 hours
- Business: < 12 hours
- Enterprise: < 4 hours

**Before contacting:**
1. Check [Common Issues](./troubleshooting/common-issues)
2. Gather error messages and screenshots
3. Note what you've already tried

---

## 📊 What's New

### Version 1.4.5 (Latest)

**Fixed:**
- ✅ Incident resolution now correctly updates SolarWinds state
- ✅ Smart comment deduplication prevents ticket spam
- ✅ Improved error handling for API timeouts

**Added:**
- ✅ Severity filtering (ignore specific alert levels)
- ✅ Configurable comment interval
- ✅ Enhanced alert context in incidents

[Full Changelog](./reference/changelog)

---

## 🗺️ Roadmap

**Coming Soon:**
- ServiceNow support
- ConnectWise support
- Jira Service Management support
- Custom field mapping
- Alert aggregation (multiple alerts → one ticket)
- SLA tracking integration

[View Full Roadmap](./reference/roadmap)

---

## 📞 Contact

- **Documentation:** [https://docs.cynteocloud.com](https://docs.cynteocloud.com)
- **Support:** support@cynteocloud.com
- **Sales:** sales@cynteocloud.com
- **Website:** [https://cynteocloud.com](https://cynteocloud.com)

---

## 📄 Legal

- [Terms of Service](./legal/terms-of-service)
- [Privacy Policy](./legal/privacy-policy)
- [SLA](./legal/sla)
- [Data Processing Agreement](./legal/dpa)

---

**Ready to get started?**

**[→ Quick Start Guide](./getting-started/quickstart)**

---

*Last updated: October 29, 2025*  
*Documentation version: 1.0*
