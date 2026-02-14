# 🔄 Migration Strategies

## 🔑 Overview

Migrating to Azure AD B2C requires careful planning whether you're moving from a legacy system, another identity provider, or upgrading from user flows to custom policies.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔄 Migration Scenarios                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│   │  Legacy     │     │  Other IdP  │     │  User Flows │          │
│   │  Database   │     │  (Auth0,    │     │     to      │          │
│   │  (SQL,      │     │  Okta,      │     │   Custom    │          │
│   │  etc.)      │     │  Firebase)  │     │   Policies  │          │
│   └──────┬──────┘     └──────┬──────┘     └──────┬──────┘          │
│          │                   │                   │                  │
│          └───────────────────┼───────────────────┘                  │
│                              │                                       │
│                              ▼                                       │
│                    ┌─────────────────┐                              │
│                    │  Azure AD B2C   │                              │
│                    │                 │                              │
│                    │  👤 👤 👤 👤 👤  │                              │
│                    └─────────────────┘                              │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📊 Migration Approaches

| Approach | Description | Best For |
|----------|-------------|----------|
| **Big Bang** | Migrate all users at once | Small user bases |
| **Just-in-Time (JIT)** | Migrate on first login | Large user bases |
| **Staged/Phased** | Migrate in batches | Risk-averse orgs |
| **Parallel Run** | Run both systems | Critical systems |

## 🔄 Just-in-Time Migration

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔄 JIT Migration Flow                             │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   User attempts sign-in to Azure AD B2C                             │
│                     │                                                │
│                     ▼                                                │
│   ┌─────────────────────────────────────────┐                       │
│   │  Does user exist in B2C?                │                       │
│   └─────────────────┬───────────────────────┘                       │
│                     │                                                │
│          ┌──────────┴──────────┐                                    │
│          │                     │                                     │
│          ▼                     ▼                                     │
│     ┌─────────┐          ┌─────────┐                                │
│     │   Yes   │          │   No    │                                │
│     │Normal   │          │Migration│                                │
│     │ Login   │          │  Flow   │                                │
│     └─────────┘          └────┬────┘                                │
│                               │                                      │
│                               ▼                                      │
│               ┌───────────────────────────────┐                     │
│               │  Call Legacy System API       │                     │
│               │  Validate credentials         │                     │
│               └───────────────┬───────────────┘                     │
│                               │                                      │
│                    ┌──────────┴──────────┐                          │
│                    │                     │                           │
│                    ▼                     ▼                           │
│              ┌─────────┐          ┌─────────┐                       │
│              │ Valid   │          │ Invalid │                       │
│              │         │          │         │                       │
│              └────┬────┘          └────┬────┘                       │
│                   │                    │                             │
│                   ▼                    ▼                             │
│   ┌───────────────────────┐    ┌────────────────┐                   │
│   │ Create B2C account    │    │ Show error     │                   │
│   │ with legacy data      │    │ message        │                   │
│   │ Issue B2C token       │    │                │                   │
│   └───────────────────────┘    └────────────────┘                   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### JIT Migration Custom Policy

```xml
<!-- Technical Profile for Legacy System Lookup -->
<TechnicalProfile Id="REST-LegacyLookup">
  <DisplayName>Legacy System Validation</DisplayName>
  <Protocol Name="Proprietary"
            Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine" />
  <Metadata>
    <Item Key="ServiceUrl">https://legacy.contoso.com/api/validate</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="AuthenticationType">Bearer</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_LegacyApiKey" />
  </CryptographicKeys>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="email" />
    <InputClaim ClaimTypeReferenceId="password" PartnerClaimType="password" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="legacyUserId" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="email" />
    <OutputClaim ClaimTypeReferenceId="migrationRequired" DefaultValue="true" />
  </OutputClaims>
</TechnicalProfile>
```

## 📦 Bulk User Import

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📦 Bulk Migration Process                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Step 1: Export from Legacy System                                 │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  {                                                           │   │
│   │    "users": [                                                │   │
│   │      {                                                       │   │
│   │        "email": "user@example.com",                         │   │
│   │        "displayName": "John Doe",                           │   │
│   │        "passwordHash": "optional_if_force_reset",           │   │
│   │        "customAttributes": { "loyaltyId": "123" }           │   │
│   │      }                                                       │   │
│   │    ]                                                         │   │
│   │  }                                                           │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                              │                                       │
│                              ▼                                       │
│   Step 2: Transform to Graph API Format                             │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  POST /users                                                 │   │
│   │  {                                                           │   │
│   │    "accountEnabled": true,                                   │   │
│   │    "displayName": "John Doe",                               │   │
│   │    "identities": [{                                          │   │
│   │      "signInType": "emailAddress",                          │   │
│   │      "issuer": "contoso.onmicrosoft.com",                   │   │
│   │      "issuerAssignedId": "user@example.com"                 │   │
│   │    }],                                                       │   │
│   │    "passwordProfile": {                                      │   │
│   │      "forceChangePasswordNextSignIn": true,                 │   │
│   │      "password": "TempPassword123!"                         │   │
│   │    }                                                         │   │
│   │  }                                                           │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                              │                                       │
│                              ▼                                       │
│   Step 3: Import via Graph API (batch or individual)                │
│                              │                                       │
│                              ▼                                       │
│   Step 4: Notify users to reset passwords                           │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Graph API Batch Import Script

```powershell
# PowerShell example for bulk user import
$users = Import-Csv "users.csv"

foreach ($user in $users) {
    $body = @{
        accountEnabled = $true
        displayName = $user.DisplayName
        identities = @(
            @{
                signInType = "emailAddress"
                issuer = "contoso.onmicrosoft.com"
                issuerAssignedId = $user.Email
            }
        )
        passwordProfile = @{
            forceChangePasswordNextSignIn = $true
            password = (New-Guid).ToString() + "Aa1!"
        }
    } | ConvertTo-Json -Depth 10

    Invoke-MgGraphRequest -Method POST -Uri "/v1.0/users" -Body $body
}
```

## 🔀 User Flows to Custom Policies

```
┌─────────────────────────────────────────────────────────────────────┐
│              🔀 User Flow → Custom Policy Migration                  │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   When to Migrate:                                                   │
│   ✅ Need REST API integration                                       │
│   ✅ Complex conditional logic                                       │
│   ✅ Multi-step verification                                         │
│   ✅ Custom claims transformation                                    │
│   ✅ Account linking                                                 │
│                                                                      │
│   Migration Steps:                                                   │
│                                                                      │
│   1. Download Custom Policy Starter Pack                            │
│      └── https://github.com/Azure-Samples/active-directory-b2c-     │
│          custom-policy-starterpack                                  │
│                                                                      │
│   2. Configure Base Settings                                        │
│      └── Replace tenant names, app IDs                              │
│                                                                      │
│   3. Create Policy Keys                                             │
│      └── TokenSigningKeyContainer, TokenEncryptionKeyContainer      │
│                                                                      │
│   4. Upload in Order                                                │
│      └── Base → Localization → Extensions → Relying Party           │
│                                                                      │
│   5. Test with "Run now"                                            │
│      └── Verify all flows work correctly                            │
│                                                                      │
│   6. Update Applications                                            │
│      └── Point to new policy names (B2C_1A_xxx)                     │
│                                                                      │
│   7. Run Parallel (Optional)                                        │
│      └── Keep user flow active during transition                    │
│                                                                      │
│   8. Deprecate User Flow                                            │
│      └── Remove after confirmed working                             │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔐 Password Migration Options

| Option | Security | User Experience |
|--------|----------|-----------------|
| **Force Reset** | High | User resets on first login |
| **Password Hash Migration** | Medium | Seamless (if supported) |
| **JIT Validation** | High | Legacy validates, B2C stores |
| **Temporary Password** | Medium | User gets temp password via email |

### Password Hash Considerations

```
┌─────────────────────────────────────────────────────────────────────┐
│                 🔐 Password Hash Migration                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Azure AD B2C does NOT directly import password hashes.            │
│                                                                      │
│   Options:                                                           │
│                                                                      │
│   1. JIT Migration with Legacy Validation                           │
│      • User enters password                                         │
│      • B2C calls legacy API to validate                             │
│      • If valid, create B2C account with same password              │
│      • Future logins use B2C directly                               │
│                                                                      │
│   2. Force Password Reset                                           │
│      • Import users without passwords                               │
│      • Send password reset emails                                   │
│      • Users set new passwords in B2C                               │
│                                                                      │
│   3. Azure AD Password Hash Sync (Hybrid only)                      │
│      • For hybrid Azure AD scenarios                                │
│      • Not applicable for B2C consumer scenarios                    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Migration Checklist

| Phase | Task | Status |
|-------|------|--------|
| **Planning** | Identify user count | ☐ |
| **Planning** | Map attributes to B2C | ☐ |
| **Planning** | Choose migration approach | ☐ |
| **Planning** | Define rollback plan | ☐ |
| **Prep** | Set up B2C tenant | ☐ |
| **Prep** | Configure policies | ☐ |
| **Prep** | Build migration API (if JIT) | ☐ |
| **Prep** | Test with sample users | ☐ |
| **Execute** | Run pilot migration | ☐ |
| **Execute** | Monitor for issues | ☐ |
| **Execute** | Full migration | ☐ |
| **Post** | Verify user access | ☐ |
| **Post** | Decommission legacy | ☐ |

## ⚠️ Common Migration Challenges

| Challenge | Solution |
|-----------|----------|
| **Password hashes incompatible** | Use JIT migration or force reset |
| **Custom attributes missing** | Create extension attributes |
| **Social account mapping** | Map by email or create linking flow |
| **Large user volume** | Use batch API with rate limiting |
| **Downtime requirements** | Use parallel run strategy |
| **User communication** | Send migration emails in advance |

## 🎯 Exam Tips

> **Remember:**
> - **JIT migration** validates against legacy on first login
> - **Bulk import** uses Microsoft Graph API
> - Password hashes **cannot be directly imported**
> - Custom policies needed for **JIT migration**
> - Upload order: **Base → Extensions → Relying Party**
> - User Flows use **B2C_1_**, Custom Policies use **B2C_1A_**

## 💡 Key Takeaways

1. 🔄 Choose migration approach based on user count and risk tolerance
2. 📦 Bulk import uses Graph API, not password hashes
3. 🔐 JIT migration preserves user passwords through legacy validation
4. 🔀 Custom policies enable advanced migration scenarios
5. 📋 Plan thoroughly with rollback strategies
6. ⚠️ Test extensively before full migration
