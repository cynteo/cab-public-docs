---
title: "Security Overview"
description: "Security architecture and compliance information for Cynteo Alert Bridge"
weight: 2
---

# Security Overview

Learn how Cynteo Alert Bridge protects your data and maintains security compliance.

---

## Security Architecture

### Zero Data Storage

- **Low Persistence** - Alert data is processed in real-time and minimally stored
- **Stateless Processing** - Each alert is handled independently
- **No Logging of Sensitive Data** - Only operational logs without credentials

### Secure Credential Management

- **Azure Key Vault** - API tokens stored encrypted in dedicated Key Vault deployed with the solution
- **Secure Parameters** - Credentials configured as secure parameters on Logic App
- **No Plain Text Storage** - Credentials never stored in plain text
- **Managed Identity** - Solution uses managed identity for Key Vault access
- **Least Privilege** - All components use minimum required permissions

### Network Security

- **HTTPS Only** - All communication encrypted in transit
- **Webhook Validation** - Azure Monitor signatures verified

---

## Compliance

### Data Residency

- **Your Subscription** - All resources deployed to YOUR Azure subscription
- **Your Region** - Deploy to any Azure region you choose
- **Your Control** - Full RBAC and access control

### GDPR Compliance

- **No Personal Data Processing** - Only technical monitoring data
- **Data Controller** - You remain the data controller
- **Right to Delete** - Delete resources anytime
- **Privacy by Design** - Minimal data collection

---

## Access Control

### Managed Application Model

Cynteo Alert Bridge deploys as an **Azure Managed Application**, which means:

- **Customer Access:** Read-only visibility to deployed resources
- **No Direct Modification:** Customers cannot change configurations directly
- **Cynteo Management:** Cynteo Cloud manages the solution lifecycle
- **Customer Control:** Customers control deployment and can delete at any time

### What Customers Can See

✅ **Read Access:**
- Resource existence and status
- High-level metrics and health
- Deployment information
- Billing and usage data

❌ **No Access:**
- Logic App secure parameters
- Function App code and variables
- Storage account contents
- Key Vault secrets
- Run history with sensitive data

### ITSM Platform Permissions

The API token provided during deployment should have:
- ✅ Create incidents/tickets
- ✅ Update incidents/tickets
- ✅ Add comments/notes
- ❌ Delete incidents (not required)
- ❌ Manage users (not required)

---

## Audit & Monitoring

### Azure Monitor Integration

- **Activity Logs** - All configuration changes logged
- **Diagnostic Logs** - Logic App execution logs
- **Alerts** - Set up alerts on Logic App failures
- **Metrics** - Track success/failure rates

### Run History

- **30-Day Retention** - Logic App maintains 30-day history
- **Success/Failure Status** - See which alerts succeeded
- **Error Details** - Debug failed executions
- **No Sensitive Data** - PII redacted from logs

---

## Incident Response

### Vulnerability Disclosure

Report security vulnerabilities to:
- support@cynteocloud.com

---

## Compliance Documentation

Available upon request:
- Security Architecture Diagrams
- Data Flow Diagrams

Contact [info@cynteocloud.com](mailto:info@cynteocloud.com) for access.

---

## See Also

- [Incident Fields](/reference/incident-fields) - What data is sent
- [SolarWinds Setup](/platforms/solarwinds/setup) - API token permissions

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

