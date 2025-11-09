---
title: "ConnectWise Manage & PSA"
description: "ConnectWise integration for MSPs (Coming Q4 2025 - Q1 2026)"
weight: 2
---

# ConnectWise Manage & PSA Integration

**Status:** Coming Q4 2025 - Q1 2026

---

## Overview

Cynteo Alert Bridge will integrate with ConnectWise Manage and ConnectWise PSA, providing MSPs with seamless Azure alert routing to customer tickets.

---

## Planned Features

### Board-Based Routing

Route alerts to specific service boards:
- Specify board number per deployment
- Different boards for different customers
- Support for all board types (Service, Project, Sales)

### Ticket Owner Assignment

Automatic owner assignment:
- Assign to specific technician
- Assign to team/queue
- Round-robin distribution

### Company Integration

Map Azure alerts to ConnectWise companies:
- Automatic company lookup
- Contact association
- SLA application

### Priority & Status Sync

Full ticket lifecycle:
- Azure severity → ConnectWise priority
- Alert fired → Ticket opened
- Alert resolved → Ticket resolved
- Bi-directional status sync

---

## MSP Use Cases

### Multi-Customer Management

**Deploy one instance per customer:**

```
MSP Account
├── Customer A Deployment → Board 1
├── Customer B Deployment → Board 2
└── Customer C Deployment → Board 3
```

**Benefits:**
- Isolated deployments per customer
- Customer-specific configurations
- Separate billing and tracking
- Clear ticket ownership

### Configuration Example

```
Customer: Acme Corp
├── ConnectWise Company: Acme Corp (ID: 123)
├── Board: Service Board (Board 1)
├── Owner: Cloud Team
└── SLA: 4-hour response
```

---

## Current Status

### What's Being Built

- [x] Architecture design complete
- [x] API integration planned
- [ ] Development in progress (Q4 2025)
- [ ] Beta testing (Q1 2026)
- [ ] General availability (Q1 2026)

### Join Beta Program

Get early access to ConnectWise integration!

**Email:** [beta@cynteocloud.com](mailto:beta@cynteocloud.com)

**Include:**
- MSP name
- Number of customers
- ConnectWise version
- Interested timeline

---

## Estimated Features

### Ticket Field Mapping

| Azure Alert Data | ConnectWise Field |
|------------------|-------------------|
| Alert name | Summary |
| Alert severity | Priority |
| Alert details | Initial description |
| Resource info | Internal notes |
| Resolution | Resolution notes |

### Supported Configurations

- **Boards:** All service, project, and sales boards
- **Statuses:** New, In Progress, Resolved, Closed
- **Priorities:** Critical, High, Medium, Low (customizable)
- **Types:** Service Request, Incident, Problem

---

## Want to Influence Development?

We're actively designing the ConnectWise integration and want MSP input!

**Share your requirements:**
- Email: [product@cynteocloud.com](mailto:product@cynteocloud.com)
- Subject: "ConnectWise Integration Feedback"
- Include your must-have features

---

## In the Meantime

### Current Option: SolarWinds

While ConnectWise integration is being developed, consider:
- [SolarWinds Service Desk](/platforms/solarwinds/) - Fully supported now
- Similar MSP capabilities
- Easy to switch to ConnectWise later

---

## Stay Updated

- **Roadmap:** [View full roadmap](/reference/roadmap)
- **Changelog:** [Latest updates](/reference/changelog)
- **Newsletter:** Subscribe at [cynteocloud.com](https://cynteocloud.com)

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

