# SolarWinds Service Desk Setup

Configure SolarWinds for Azure Alert Bridge integration.

---

## Prerequisites

- SolarWinds Service Desk (Samanage) account
- Administrator access to generate API tokens

---

## Generating API Token

### Step 1: Log in to SolarWinds

1. Go to your SolarWinds Service Desk portal
2. Log in with administrator credentials

### Step 2: Navigate to API Settings

1. Click your **profile icon** (top right)
2. Select **"Account Settings"** or **"Setup"**
3. Navigate to **"API & Integrations"** or **"Developer"** section

### Step 3: Generate Token

1. Click **"Generate New Token"** or **"Create API Token"**
2. **Name:** `Azure Alert Bridge`
3. **Permissions:** Ensure the following are enabled:
   - ✅ Read Incidents
   - ✅ Create Incidents
   - ✅ Update Incidents
   - ✅ Add Comments
4. Click **"Generate"** or **"Create"**

### Step 4: Save Token

**IMPORTANT:** Copy the token immediately - you won't be able to see it again!

**Token format:**
```
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

**Store securely** - This token will be entered during Azure deployment.

---

## Verifying API Access

### Test Your Token (Optional)

You can verify your token works using curl:

```bash
curl -X GET "https://api.samanage.com/incidents.json" \
  -H "X-Samanage-Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Accept: application/json"
```

**Expected response:** JSON list of incidents (or empty array if no incidents)

**Error response:** Check token and permissions

---

## SolarWinds Configuration

### Recommended Settings

#### 1. Create Dedicated Category (Optional)

1. Go to **Setup → Incidents → Categories**
2. Click **"+ Add Category"**
3. **Name:** `Azure Monitor`
4. **Save**

You can reference this during Azure Alert Bridge deployment.

#### 2. Create Dedicated Subcategory (Optional)

1. Under the category created above
2. Click **"+ Add Subcategory"**
3. **Name:** `Alerts`
4. **Save**

#### 3. Configure Priority Levels

Verify your SolarWinds has these priority levels:
- High
- Medium
- Low

These will be mapped from Azure alert severities (Sev0-Sev3).

#### 4. Set Up Requester Email

Create a system user email that will be the "requester" for auto-created incidents:

1. **Option A:** Create dedicated service account
   - Email: `azure-monitor@yourcompany.com`
   - Name: Azure Monitor Service

2. **Option B:** Use existing IT service account
   - Use your existing automation/monitoring email

---

## Permissions Required

### Minimum API Token Permissions

Your token must have:

| Permission | Required | Purpose |
|------------|----------|---------|
| Read Incidents | ✅ Yes | Check for existing incidents |
| Create Incidents | ✅ Yes | Create new incidents |
| Update Incidents | ✅ Yes | Update and resolve incidents |
| Add Comments | ✅ Yes | Add updates to incidents |
| Read Categories | ⚠️ Recommended | Validate category names |

---

## Security Best Practices

### 1. Token Rotation

- Rotate API tokens every 90 days
- Update token in Azure Key Vault when rotated
- See [Update API Token Guide](../guides/update-api-token.md)

### 2. IP Restrictions (if available)

If SolarWinds supports IP restrictions:
- Add Azure region IP ranges
- See [Azure IP Ranges](https://www.microsoft.com/download/details.aspx?id=56519)

### 3. Audit Logging

Enable audit logging in SolarWinds to track:
- Incident creations from API
- API token usage
- Unauthorized access attempts

---

## Testing Your Setup

### Create Test Incident via API

```bash
curl -X POST "https://api.samanage.com/incidents.json" \
  -H "X-Samanage-Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "incident": {
      "name": "Test Incident from Azure Alert Bridge",
      "priority": "Low",
      "description": "This is a test to verify API access",
      "requester": {
        "email": "your-email@company.com"
      }
    }
  }'
```

**Expected response:** JSON with created incident details

**If successful:** Your token is configured correctly!

---

## Common Issues

### "Unauthorized" Error

**Cause:** Invalid token or expired

**Fix:**
1. Regenerate token in SolarWinds
2. Update token in Azure Key Vault
3. Verify token has correct permissions

### "Forbidden" Error

**Cause:** Token lacks required permissions

**Fix:**
1. Go to SolarWinds API settings
2. Edit token permissions
3. Enable: Read/Create/Update Incidents, Add Comments

### "Not Found" Error on Category

**Cause:** Category name doesn't exist in SolarWinds

**Fix:**
1. Verify category name spelling
2. Create category in SolarWinds (see above)
3. Or use default "Infrastructure" category

---

## Next Steps

Now that SolarWinds is configured:

1. **[Deploy Azure Alert Bridge](./quickstart.md)** from Azure Marketplace
2. **[Configure Alert Action Groups](../guides/configure-alert-action-group.md)**
3. **[Test the integration](./quickstart.md#step-4-test-it)**

---

## API Documentation

For more details on SolarWinds API:
- [Official SolarWinds API Docs](https://help.samanage.com/s/article/API-Documentation)
- [Incident API Reference](https://help.samanage.com/s/article/Incidents-API)

---

**Need help?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

