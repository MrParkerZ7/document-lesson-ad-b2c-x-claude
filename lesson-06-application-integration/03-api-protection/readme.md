# 🔌 API Protection with Azure AD B2C

## 🔑 Overview

Protecting your APIs with Azure AD B2C ensures that only authenticated and authorized users can access your backend services. The API validates access tokens issued by B2C.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔌 API Protection Flow                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   👤 User                 🖥️ Client App              🔌 Protected API│
│      │                         │                           │         │
│      │  1. Login               │                           │         │
│      │────────────────────────►│                           │         │
│      │                         │                           │         │
│      │  2. Access Token        │                           │         │
│      │◄────────────────────────│                           │         │
│      │                         │                           │         │
│      │                         │  3. API Request           │         │
│      │                         │  Authorization: Bearer    │         │
│      │                         │  {access_token}           │         │
│      │                         │──────────────────────────►│         │
│      │                         │                           │         │
│      │                         │                    4. Validate      │
│      │                         │                       Token         │
│      │                         │                      ✅ ❌          │
│      │                         │                           │         │
│      │                         │  5. Response / 401        │         │
│      │                         │◄──────────────────────────│         │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Token Validation Steps

| Step | Description | Implementation |
|------|-------------|----------------|
| 1. **Extract Token** | Get token from Authorization header | Parse `Bearer {token}` |
| 2. **Validate Signature** | Verify with B2C public keys | Use JWKS endpoint |
| 3. **Check Issuer** | Must be your B2C tenant | Compare `iss` claim |
| 4. **Check Audience** | Must match your API | Compare `aud` claim |
| 5. **Check Expiration** | Token not expired | Compare `exp` claim |
| 6. **Check Scopes** | Has required permissions | Check `scp` claim |

## 🔐 .NET API Protection

### Configuration

```csharp
// appsettings.json
{
  "AzureAdB2C": {
    "Instance": "https://contoso.b2clogin.com",
    "Domain": "contoso.onmicrosoft.com",
    "ClientId": "your-api-client-id",
    "SignUpSignInPolicyId": "B2C_1_SignUpSignIn"
  }
}

// Program.cs
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApi(builder.Configuration.GetSection("AzureAdB2C"));

// Enable authorization
builder.Services.AddAuthorization();

var app = builder.Build();

app.UseAuthentication();
app.UseAuthorization();
```

### Protected Controller

```csharp
[Authorize]
[ApiController]
[Route("api/[controller]")]
public class DataController : ControllerBase
{
    [HttpGet]
    [RequiredScope("read")]  // Require 'read' scope
    public IActionResult Get()
    {
        // Access user claims
        var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
        return Ok(new { message = "Protected data", userId });
    }

    [HttpPost]
    [RequiredScope("write")]  // Require 'write' scope
    public IActionResult Post([FromBody] DataModel data)
    {
        return Ok(new { message = "Data saved" });
    }
}
```

## 📜 Node.js API Protection

```javascript
const express = require('express');
const jwt = require('jsonwebtoken');
const jwksRsa = require('jwks-rsa');

const app = express();

// JWKS client for signature validation
const jwksClient = jwksRsa({
  jwksUri: `https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_SignUpSignIn/discovery/v2.0/keys`
});

// Get signing key
function getKey(header, callback) {
  jwksClient.getSigningKey(header.kid, (err, key) => {
    callback(null, key.publicKey || key.rsaPublicKey);
  });
}

// Token validation middleware
const validateToken = (req, res, next) => {
  const authHeader = req.headers.authorization;

  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return res.status(401).json({ error: 'No token provided' });
  }

  const token = authHeader.split(' ')[1];

  jwt.verify(token, getKey, {
    audience: 'your-api-client-id',
    issuer: `https://contoso.b2clogin.com/{tenant-id}/v2.0/`,
    algorithms: ['RS256']
  }, (err, decoded) => {
    if (err) {
      return res.status(401).json({ error: 'Invalid token' });
    }
    req.user = decoded;
    next();
  });
};

// Protected endpoint
app.get('/api/data', validateToken, (req, res) => {
  res.json({ message: 'Protected data', user: req.user.sub });
});
```

## 🔍 Token Validation Endpoints

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔍 B2C Metadata Endpoints                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   OpenID Configuration (Discovery Document):                        │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/    │   │
│   │  {policy}/v2.0/.well-known/openid-configuration             │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                              │                                       │
│                              ▼                                       │
│   Returns:                                                           │
│   • issuer                  → Token issuer URL                      │
│   • jwks_uri               → URL to get public keys                 │
│   • token_endpoint         → Token endpoint                         │
│   • authorization_endpoint → Auth endpoint                          │
│                                                                      │
│   JWKS (JSON Web Key Set):                                          │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/    │   │
│   │  {policy}/discovery/v2.0/keys                               │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                              │                                       │
│                              ▼                                       │
│   Returns:                                                           │
│   • keys[] → Array of public keys for signature verification        │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🛡️ Scope-Based Authorization

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🛡️ Scope-Based Access Control                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Define Scopes in API Registration:                                │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  • api://myapi/read   - Read access                          │   │
│   │  • api://myapi/write  - Write access                         │   │
│   │  • api://myapi/admin  - Admin access                         │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   Token scp claim example:                                          │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  "scp": "read write"                                         │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   API Authorization:                                                 │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  GET /api/data    → Requires "read" scope                    │   │
│   │  POST /api/data   → Requires "write" scope                   │   │
│   │  DELETE /api/data → Requires "admin" scope                   │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔗 Client App Requesting Scopes

```javascript
// Client app requests specific scopes
const tokenRequest = {
  scopes: [
    "https://contoso.onmicrosoft.com/api/read",
    "https://contoso.onmicrosoft.com/api/write"
  ],
  account: currentAccount
};

const response = await msalInstance.acquireTokenSilent(tokenRequest);
// Access token now contains: scp: "read write"
```

## ⚠️ Common Security Mistakes

| Mistake | Risk | Solution |
|---------|------|----------|
| **Not validating signature** | Token forgery | Always verify with JWKS |
| **Skipping issuer check** | Accept tokens from any IdP | Validate `iss` claim |
| **Ignoring audience** | Token misuse across APIs | Check `aud` matches your API |
| **Not checking expiration** | Using stale tokens | Verify `exp` < current time |
| **Trusting all scopes** | Over-privileged access | Check specific scopes needed |
| **Logging tokens** | Token theft | Never log full tokens |

## 📊 Token Claims in API

| Claim | Description | Use Case |
|-------|-------------|----------|
| `sub` | User's object ID | User identification |
| `oid` | Object ID (same as sub) | User lookup |
| `scp` | Granted scopes | Authorization |
| `tfp` | Policy used | Multi-policy handling |
| `iss` | Token issuer | Validation |
| `aud` | Intended audience | Validation |
| `exp` | Expiration time | Validation |
| Custom | Extension attributes | Business logic |

## 🎯 Exam Tips

> **Remember:**
> - APIs validate **access tokens**, not ID tokens
> - Always validate: **signature, issuer, audience, expiration**
> - Use **JWKS endpoint** to get public keys for signature verification
> - **Scopes** (`scp` claim) determine what the token allows
> - The **audience** (`aud`) should match your API's client ID
> - Different policies have **different JWKS endpoints**
> - Use middleware/libraries instead of manual validation

## 💡 Key Takeaways

1. 🔌 APIs validate access tokens using B2C public keys
2. 🔍 Use the JWKS endpoint to retrieve signing keys
3. ✅ Always validate signature, issuer, audience, and expiration
4. 🛡️ Implement scope-based authorization for fine-grained access
5. 📚 Use platform libraries (Microsoft.Identity.Web, passport-azure-ad)
6. ⚠️ Never trust tokens without proper validation
