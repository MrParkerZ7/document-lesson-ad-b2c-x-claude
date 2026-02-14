# 🔗 Social Identity Providers

## 📋 Overview

Social Identity Providers allow users to sign in to your application using their existing social media accounts. This eliminates the need for users to create and remember another password.

```
┌─────────────────────────────────────────────────────────────────────┐
│                 🔗 Social Identity Providers                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐     │
│   │ Google  │ │Facebook │ │  Apple  │ │Microsoft│ │ Twitter │     │
│   │         │ │         │ │         │ │ Account │ │         │     │
│   └─────────┘ └─────────┘ └─────────┘ └─────────┘ └─────────┘     │
│                                                                      │
│   ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐                  │
│   │ GitHub  │ │LinkedIn │ │ Amazon  │ │ WeChat  │                  │
│   │         │ │         │ │         │ │ (China) │                  │
│   └─────────┘ └─────────┘ └─────────┘ └─────────┘                  │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📊 Supported Social Providers

| Provider | Protocol | Claims Available | Setup Complexity |
|----------|----------|------------------|------------------|
| **Google** | OAuth 2.0 / OIDC | email, name, picture | ⭐ Easy |
| **Facebook** | OAuth 2.0 | email, name, first/last name | ⭐ Easy |
| **Apple** | OIDC | email, name | ⭐⭐ Medium |
| **Microsoft** | OAuth 2.0 / OIDC | email, name, profile | ⭐ Easy |
| **Twitter/X** | OAuth 1.0a | username, name | ⭐⭐ Medium |
| **GitHub** | OAuth 2.0 | email, login, name | ⭐ Easy |
| **LinkedIn** | OAuth 2.0 | email, firstName, lastName | ⭐⭐ Medium |
| **Amazon** | OAuth 2.0 | email, name | ⭐ Easy |

## 🔧 Configuration Steps

### General Configuration Pattern

```
┌────────────────────────────────────────────────────────────────────┐
│              Social IdP Setup - General Steps                       │
├────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Step 1: Create App at Provider                                    │
│  ┌───────────────────────────────────────────────────────────┐    │
│  │  • Go to provider's developer portal                       │    │
│  │  • Create new application                                   │    │
│  │  • Configure redirect URI                                   │    │
│  │  • Get Client ID and Client Secret                          │    │
│  └───────────────────────────────────────────────────────────┘    │
│                                                                     │
│  Step 2: Configure in Azure AD B2C                                 │
│  ┌───────────────────────────────────────────────────────────┐    │
│  │  • Add Identity Provider                                    │    │
│  │  • Enter Client ID                                          │    │
│  │  • Enter Client Secret                                      │    │
│  │  • Configure scopes                                         │    │
│  └───────────────────────────────────────────────────────────┘    │
│                                                                     │
│  Step 3: Add to User Flow                                          │
│  ┌───────────────────────────────────────────────────────────┐    │
│  │  • Edit User Flow                                           │    │
│  │  • Select Identity Provider                                 │    │
│  │  • Save and test                                            │    │
│  └───────────────────────────────────────────────────────────┘    │
│                                                                     │
└────────────────────────────────────────────────────────────────────┘
```

### Redirect URI Format

```
https://<tenant-name>.b2clogin.com/<tenant-name>.onmicrosoft.com/oauth2/authresp
```

**Example:**
```
https://contoso.b2clogin.com/contoso.onmicrosoft.com/oauth2/authresp
```

## 🔍 Provider-Specific Setup

### 1️⃣ Google

| Setting | Value |
|---------|-------|
| **Developer Portal** | console.cloud.google.com |
| **Create** | OAuth 2.0 Client ID |
| **Scopes** | openid, email, profile |
| **Redirect URI** | B2C OAuth reply URL |

```
┌────────────────────────────────────────────────────────────────┐
│                 Google Setup Steps                              │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Go to Google Cloud Console                                 │
│  2. Create new project (or use existing)                       │
│  3. Enable Google+ API / People API                            │
│  4. Go to Credentials → Create OAuth 2.0 Client ID             │
│  5. Application type: Web application                          │
│  6. Add authorized redirect URI (B2C URL)                      │
│  7. Copy Client ID and Client Secret                           │
│  8. In B2C: Add Google IdP with credentials                   │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

### 2️⃣ Facebook

| Setting | Value |
|---------|-------|
| **Developer Portal** | developers.facebook.com |
| **Create** | Facebook App |
| **Product** | Facebook Login |
| **Scopes** | email, public_profile |

```
┌────────────────────────────────────────────────────────────────┐
│                 Facebook Setup Steps                            │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Go to Meta for Developers                                  │
│  2. Create new app (Consumer type)                             │
│  3. Add Facebook Login product                                 │
│  4. Settings → Basic: Get App ID and App Secret               │
│  5. Facebook Login → Settings:                                 │
│     - Add Valid OAuth Redirect URI (B2C URL)                   │
│  6. App must be in "Live" mode for production                  │
│  7. In B2C: Add Facebook IdP with credentials                 │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

### 3️⃣ Apple (Sign in with Apple)

| Setting | Value |
|---------|-------|
| **Developer Portal** | developer.apple.com |
| **Create** | Services ID + Key |
| **Protocol** | OIDC |
| **Scopes** | name, email |

```
┌────────────────────────────────────────────────────────────────┐
│                 Apple Setup Steps                               │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1. Go to Apple Developer account                              │
│  2. Register App ID (with Sign in with Apple)                  │
│  3. Register Services ID                                       │
│     - Identifier: your-service-id                              │
│     - Enable Sign in with Apple                                │
│     - Configure domain and return URL                          │
│  4. Create Key for Sign in with Apple                          │
│  5. Download the key file (.p8)                                │
│  6. In B2C: Configure Apple IdP with:                         │
│     - Service ID (as Client ID)                                │
│     - Key ID, Team ID, and P8 key content                     │
│                                                                 │
│  ⚠️ Apple requires verified domain ownership                  │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

### 4️⃣ Microsoft Account

| Setting | Value |
|---------|-------|
| **Developer Portal** | portal.azure.com |
| **Create** | App Registration |
| **Account Type** | Personal Microsoft accounts |
| **Scopes** | openid, email, profile |

## 📋 Claims Mapping

Different providers return different claims. B2C maps them to standard claims.

| Provider Claim | B2C Claim | Description |
|----------------|-----------|-------------|
| `sub` | `sub` | Unique identifier |
| `email` | `email` | Email address |
| `name` | `name` | Display name |
| `given_name` | `givenName` | First name |
| `family_name` | `surname` | Last name |
| `picture` | N/A | Profile picture URL |

### Custom Claims Mapping

For Custom Policies, you can define explicit claim mappings:

```xml
<ClaimsTransformation Id="CreateNameFromSocialClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="givenName" />
    <InputClaim ClaimTypeReferenceId="surname" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="displayName" />
  </OutputClaims>
</ClaimsTransformation>
```

## ⚠️ Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| **Redirect URI mismatch** | Wrong URL in provider | Use exact B2C redirect URL |
| **Invalid client secret** | Expired or wrong secret | Regenerate and update |
| **Missing email claim** | User privacy settings | Request optional claims |
| **App not in live mode** | Facebook dev mode | Switch to Live mode |
| **Apple domain verification** | Domain not verified | Add verification file |

## 🔒 Security Considerations

| Consideration | Best Practice |
|---------------|---------------|
| **Client Secret** | Store securely, rotate periodically |
| **Scopes** | Request minimum necessary |
| **Redirect URI** | Use HTTPS, exact match |
| **App Review** | Complete provider's review process |
| **Rate Limits** | Be aware of provider limits |

## 🎯 Exam Tips

> **Social Provider Knowledge:**
> - Each provider needs **app registration at the provider**
> - **Client ID** and **Client Secret** are required
> - **Redirect URI** must match exactly
> - Apple uses **OIDC**, Twitter uses **OAuth 1.0a**
> - Facebook needs **Live mode** for production
> - Claims returned vary by provider
> - Configure IdPs **before** adding to User Flows

## 💡 Key Takeaways

1. 🔗 Social IdPs use **OAuth 2.0** or **OIDC** protocols
2. 📝 Each provider requires **app registration** at their developer portal
3. 🔑 **Client ID** and **Client Secret** are the key credentials
4. 🔄 **Redirect URI** must be configured at both provider and B2C
5. 📋 **Claims** vary by provider - map to B2C standard claims
6. 🔒 Keep **Client Secrets** secure and rotate periodically
7. ⚠️ **Apple** has unique requirements (Services ID, key file)
8. 🧪 Test thoroughly in **development** before going live
