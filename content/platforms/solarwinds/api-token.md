---
title: "SolarWinds API Token"
description: "How to generate and manage SolarWinds Service Desk API tokens"
weight: 5
---

# SolarWinds API Token

Complete guide to generating and managing your SolarWinds Service Desk API token for Cynteo Alert Bridge.

---

## What is an API Token?

An API token is a secure authentication credential that allows Cynteo Alert Bridge to:
- ✅ Create incidents in SolarWinds
- ✅ Update existing incidents
- ✅ Add comments to incidents
- ✅ Resolve incidents when alerts clear

---

## Generating a Token

### Step 1: Log into SolarWinds

1. Go to your SolarWinds Service Desk URL
2. Log in with administrator credentials

### Step 2: Navigate to API Settings

**Option A: Modern Interface**
1. Click your **profile** (top-right)
2. Select **"Developer" or "API"**
3. Click **"Tokens"** or **"API Access"**

**Option B: Classic Interface**
1. Go to **Setup** → **Account**
2. Find **"API Tokens"** or **"Integrations"**
3. Click **"Generate New Token"**

### Step 3: Create Token

1. Click **"Generate New Token"** or **"Create Token"**
2. **Name:** `Cynteo Alert Bridge`
3. **Description:** `Azure Monitor integration`
4. **Permissions:** Select required permissions (see below)
5. Click **"Generate"** or **"Create"**

### Step 4: Copy Token

⚠️ **Important:** Copy the token NOW - you won't see it again!

```
Bearer eyJ0eXAiOiJKV1QiLCJhbGc...
```

Store securely in:
- Azure Key Vault (recommended)
- Password manager
- Secure note

---

## Required Permissions

The API token needs these permissions:

| Permission | Required | Purpose |
|------------|----------|---------|
| **Read Incidents** | ✅ Yes | Check for existing incidents (dedup) |
| **Create Incidents** | ✅ Yes | Create new incidents from alerts |
| **Update Incidents** | ✅ Yes | Update incidents when alerts fire again |
| **Add Comments** | ✅ Yes | Add alert updates as comments |
| **Resolve Incidents** | ✅ Yes | Auto-resolve when alerts clear |
| **Delete Incidents** | ❌ No | Not required (security best practice) |
| **Manage Users** | ❌ No | Not required |
| **Admin Access** | ❌ No | Not required |

---

## Token Format

### Full Token

```
Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwczovL2FwaS5zYW1hbmFnZS5jb20iLCJpYXQiOjE2MTYxNjE2MTYsImV4cCI6MTY0NzY5NzYxNiwianRpIjoiYWJjMTIzIn0.signature
```

### Token Parts

- **Prefix:** `Bearer ` (with space)
- **Type:** JWT (JSON Web Token)
- **Parts:** Header.Payload.Signature

---

## How Credentials Are Stored

Cynteo Alert Bridge automatically stores your API token securely:

**During Deployment:**
1. You provide the API token in the deployment form
2. Token is encrypted and stored in dedicated Azure Key Vault
3. Logic App references token using managed identity
4. Token never stored in plain text anywhere

**Security Features:**
- ✅ Encrypted at rest (AES-256)
- ✅ Encrypted in transit (TLS 1.2+)
- ✅ Accessed only by authorized managed identity
- ✅ Customer has read-only access to Key Vault but not data within it
- ❌ Token not visible in any configuration or logs

---

## Testing the Token

### Via cURL

```bash
curl -X GET "https://api.samanage.com/incidents.json" \
  -H "X-Samanage-Authorization: Bearer YOUR_TOKEN" \
  -H "Accept: application/json"
```

**Expected:** List of incidents (200 OK)

**Error 401:** Token invalid or expired

### Via PowerShell

```powershell
$headers = @{
    "X-Samanage-Authorization" = "Bearer YOUR_TOKEN"
    "Accept" = "application/json"
}

Invoke-RestMethod -Uri "https://api.samanage.com/incidents.json" -Headers $headers
```

---

## Token Security

### Compromised Token

If token is compromised:

1. **Immediately Revoke** in SolarWinds
2. **Generate New Token**
3. **Re-deploy or contact support** with new token

---

## Token Management

### Viewing Active Tokens

1. SolarWinds → **API Settings**
2. View list of active tokens
3. See:
   - Token name
   - Creation date
   - Last used date
   - Expiration (if set)

### Revoking Tokens

1. Find token in list
2. Click **"Revoke"** or **"Delete"**
3. Confirm revocation
4. Token immediately invalidated

### Updating Token in Cynteo Alert Bridge

After generating a new token, you'll need to update it in your Cynteo Alert Bridge deployment.

**To update the API token:**

Contact [support@cynteocloud.com](mailto:support@cynteocloud.com) with:
- Your deployment name/resource group
- Confirmation you have a new API token ready
- Any other configuration changes needed

Cynteo support will securely update the token in your deployment's Key Vault.

**Alternatively, redeploy:**
- Deploy a new instance with the new token
- Update action groups to use new webhook URL  
- Delete old instance

---

## Troubleshooting

### 401 Unauthorized

**Causes:**
- Token expired
- Token revoked
- Token format incorrect (missing "Bearer ")
- Wrong SolarWinds instance URL

**Solutions:**
1. Generate new token
2. Verify token format: `Bearer ` + token
3. Check SolarWinds base URL matches your instance

### 403 Forbidden

**Causes:**
- Token lacks required permissions
- User account disabled
- IP restrictions

**Solutions:**
1. Check token permissions in SolarWinds
2. Verify user account is active
3. Check API access restrictions

### Token Not Found in Key Vault

**Causes:**
- Secret name mismatch
- Logic App lacks Key Vault permissions
- Secret deleted

**Solutions:**
1. Contact support

---

## API Limits

Be aware of SolarWinds API rate limits

---

## See Also

- [SolarWinds Setup](/platforms/solarwinds/setup) - Complete setup guide
- [Security Overview](/reference/security) - Security best practices
- [Architecture Overview](/reference/architecture) - How it works

---

**Questions?** Contact [support@cynteocloud.com](mailto:support@cynteocloud.com)

