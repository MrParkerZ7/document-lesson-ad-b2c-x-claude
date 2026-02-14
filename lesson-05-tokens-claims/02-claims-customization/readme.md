# 🎫 Claims Customization

## 🔑 Overview

Claims are pieces of information about the user included in tokens. Azure AD B2C allows extensive customization of which claims appear in tokens and how they're formatted.

```
┌─────────────────────────────────────────────────────────────────────┐
│                     🎫 Claims Pipeline                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   📥 Input Sources          🔄 Processing         📤 Token Output   │
│                                                                      │
│   ┌─────────────┐          ┌─────────────┐       ┌─────────────┐   │
│   │ 👤 User     │          │ 🔄 Claims   │       │ 🎫 ID Token │   │
│   │ Attributes  │─────────►│ Transform-  │──────►│   Claims    │   │
│   └─────────────┘          │ ations      │       └─────────────┘   │
│                            │             │                          │
│   ┌─────────────┐          │             │       ┌─────────────┐   │
│   │ 🔑 Identity │─────────►│             │──────►│ 🔐 Access   │   │
│   │ Provider    │          │             │       │ Token Claims│   │
│   └─────────────┘          │             │       └─────────────┘   │
│                            │             │                          │
│   ┌─────────────┐          │             │       ┌─────────────┐   │
│   │ 📞 REST API │─────────►│             │──────►│ 🔗 SAML     │   │
│   │ Enrichment  │          └─────────────┘       │ Assertions  │   │
│   └─────────────┘                                └─────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Claim Types

| Claim Category | Source | Examples |
|----------------|--------|----------|
| **Built-in** | Azure AD B2C | `objectId`, `newUser`, `authenticationSource` |
| **User Attributes** | User profile | `displayName`, `email`, `givenName` |
| **Extension Attributes** | Custom | `extension_LoyaltyNumber`, `extension_MemberSince` |
| **IdP Claims** | Social/Enterprise | `idp`, `idp_access_token` |
| **Computed** | Transformations | Concatenated names, formatted dates |

## 🔄 Configuring Claims in User Flows

```
┌─────────────────────────────────────────────────────────────────────┐
│              📝 User Flow Claims Configuration                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Azure Portal → Azure AD B2C → User Flows → [Your Flow]           │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  User Attributes (Collected at Sign-up)                      │   │
│   │  ┌──────────────────────────────────────────────────────┐   │   │
│   │  │  ☑️ Display Name                                      │   │   │
│   │  │  ☑️ Email Address                                     │   │   │
│   │  │  ☑️ Given Name                                        │   │   │
│   │  │  ☑️ Surname                                           │   │   │
│   │  │  ☐ City                                               │   │   │
│   │  │  ☐ Country/Region                                     │   │   │
│   │  └──────────────────────────────────────────────────────┘   │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Application Claims (Returned in Token)                      │   │
│   │  ┌──────────────────────────────────────────────────────┐   │   │
│   │  │  ☑️ Display Name                                      │   │   │
│   │  │  ☑️ Email Addresses                                   │   │   │
│   │  │  ☑️ Identity Provider                                 │   │   │
│   │  │  ☑️ User's Object ID                                  │   │   │
│   │  │  ☐ Identity Provider Access Token                     │   │   │
│   │  └──────────────────────────────────────────────────────┘   │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📦 Extension Attributes

Extension attributes allow storing custom data beyond built-in attributes.

### Creating Extension Attributes

```
┌─────────────────────────────────────────────────────────────────────┐
│                  📦 Extension Attribute Setup                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Step 1: Register via b2c-extensions-app                           │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  App Registrations → b2c-extensions-app → Note App ID       │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   Step 2: Create Custom Attribute                                   │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Azure AD B2C → User Attributes → Add                       │   │
│   │                                                              │   │
│   │  Name: LoyaltyNumber                                        │   │
│   │  Data Type: String                                          │   │
│   │  Description: Customer loyalty program ID                   │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   Step 3: Use in User Flow                                          │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  User Flows → Select → User Attributes → LoyaltyNumber ☑️   │   │
│   │  User Flows → Select → Application Claims → LoyaltyNumber ☑️│   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   Storage Format: extension_{app-id-no-hyphens}_LoyaltyNumber       │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Extension Attribute Data Types

| Type | Use Case | Example |
|------|----------|---------|
| **String** | Text values | Loyalty numbers, preferences |
| **Boolean** | Yes/No flags | Newsletter opt-in |
| **Int** | Numeric values | Points balance |
| **DateTime** | Dates | Member since |

## 🔄 Claims Transformations (Custom Policies)

```xml
<!-- Example: Combine first and last name -->
<ClaimsTransformation Id="CreateDisplayName"
                       TransformationMethod="FormatStringMultipleClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="surname" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="{0} {1}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="displayName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### Common Transformation Methods

| Method | Purpose | Example Use |
|--------|---------|-------------|
| **FormatStringClaim** | Format single claim | Add prefix/suffix |
| **FormatStringMultipleClaims** | Combine claims | Full name from first + last |
| **GetCurrentDateTime** | Add timestamp | Login time |
| **CompareClaims** | Compare values | Age verification |
| **CreateStringClaim** | Create static value | App version |
| **ParseDomain** | Extract email domain | Route by company |
| **Hash** | Hash a value | Anonymize data |

## 📞 Claims Enrichment via REST API

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📞 REST API Enrichment                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   1. User Signs In                                                   │
│        │                                                             │
│        ▼                                                             │
│   2. B2C Calls REST API                                             │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  POST https://api.example.com/enrich                         │   │
│   │  {                                                           │   │
│   │    "objectId": "user-guid",                                  │   │
│   │    "email": "user@example.com"                               │   │
│   │  }                                                           │   │
│   └─────────────────────────────────────────────────────────────┘   │
│        │                                                             │
│        ▼                                                             │
│   3. API Returns Additional Claims                                  │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  {                                                           │   │
│   │    "loyaltyTier": "Gold",                                    │   │
│   │    "accountBalance": 5000,                                   │   │
│   │    "permissions": ["read", "write", "admin"]                │   │
│   │  }                                                           │   │
│   └─────────────────────────────────────────────────────────────┘   │
│        │                                                             │
│        ▼                                                             │
│   4. Claims Added to Token                                          │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔀 Claim Name Mapping

| B2C Claim Name | Standard OIDC | Token Output |
|----------------|---------------|--------------|
| `objectId` | `sub` | User identifier |
| `displayName` | `name` | Full name |
| `emails` | `email` | Email address |
| `city` | `locality` | City |
| `country` | `country` | Country |
| `postalCode` | `postal_code` | Postal code |

### Custom Claim Mapping in Relying Party

```xml
<RelyingParty>
  <TechnicalProfile Id="PolicyProfile">
    <OutputClaims>
      <!-- Map B2C claim to custom name -->
      <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="full_name" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="user_id" />
    </OutputClaims>
  </TechnicalProfile>
</RelyingParty>
```

## ⚠️ Claims Best Practices

| Practice | Description |
|----------|-------------|
| **Minimize claims** | Only include what the app needs |
| **Avoid PII** | Don't expose sensitive data unnecessarily |
| **Use extension attributes** | For custom data storage |
| **Validate on backend** | Don't trust claims blindly |
| **Consider token size** | Large tokens impact performance |

## 🎯 Exam Tips

> **Remember:**
> - **User Attributes** = collected during sign-up
> - **Application Claims** = returned in token
> - **Extension Attributes** use the `b2c-extensions-app` for storage
> - Extension attribute format: `extension_{appId}_AttributeName`
> - **Claims Transformations** are only available in custom policies
> - REST API can enrich tokens with external data
> - `PartnerClaimType` maps B2C claim names to output names

## 💡 Key Takeaways

1. 🎫 Claims are user data pieces included in tokens
2. 📝 User flows configure claims via Azure Portal checkboxes
3. 📦 Extension attributes store custom data beyond built-in fields
4. 🔄 Custom policies enable claims transformations
5. 📞 REST APIs can enrich tokens with external data
6. 🔀 Claim names can be mapped to different output names
