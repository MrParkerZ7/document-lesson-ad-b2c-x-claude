# 🏢 Enterprise Identity Providers

## 📋 Overview

**Enterprise Identity Providers** enable B2B2C scenarios where your customers' employees can sign in using their corporate credentials. This is essential for SaaS applications serving enterprise customers.

```
┌─────────────────────────────────────────────────────────────────────┐
│                 🏢 Enterprise Federation Architecture               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │                     Your SaaS Application                    │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                │                                     │
│                                ▼                                     │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │                      Azure AD B2C                            │   │
│   │                    (Federation Hub)                          │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                │                                     │
│           ┌────────────────────┼────────────────────┐               │
│           ▼                    ▼                    ▼               │
│   ┌───────────────┐   ┌───────────────┐   ┌───────────────┐        │
│   │   Customer A   │   │   Customer B   │   │   Customer C   │        │
│   │   Azure AD     │   │   Okta         │   │   ADFS         │        │
│   │   (OIDC)       │   │   (SAML)       │   │   (SAML)       │        │
│   └───────────────┘   └───────────────┘   └───────────────┘        │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔐 Supported Enterprise Protocols

| Protocol | Description | Use Case |
|----------|-------------|----------|
| **OpenID Connect** | Modern, OAuth 2.0 based | Azure AD, Auth0, Okta |
| **SAML 2.0** | XML-based, enterprise standard | ADFS, Ping, legacy IdPs |
| **WS-Federation** | Microsoft legacy protocol | Older ADFS deployments |

## 🔄 OpenID Connect (OIDC) Federation

### OIDC Overview

```
┌────────────────────────────────────────────────────────────────┐
│                 OIDC Federation Flow                            │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│   👤 User clicks "Sign in with Corporate Account"              │
│                        │                                        │
│                        ▼                                        │
│   ┌────────────────────────────────────────────────┐           │
│   │  Azure AD B2C redirects to OIDC Provider       │           │
│   │  (Authorization Endpoint)                       │           │
│   └────────────────────────────────────────────────┘           │
│                        │                                        │
│                        ▼                                        │
│   ┌────────────────────────────────────────────────┐           │
│   │  User authenticates at their corporate IdP     │           │
│   │  (Azure AD, Okta, etc.)                         │           │
│   └────────────────────────────────────────────────┘           │
│                        │                                        │
│                        ▼                                        │
│   ┌────────────────────────────────────────────────┐           │
│   │  IdP returns ID token with user claims         │           │
│   └────────────────────────────────────────────────┘           │
│                        │                                        │
│                        ▼                                        │
│   ┌────────────────────────────────────────────────┐           │
│   │  B2C validates token, creates session          │           │
│   │  Issues B2C token to application               │           │
│   └────────────────────────────────────────────────┘           │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

### OIDC Configuration

| Setting | Description | Example |
|---------|-------------|---------|
| **Metadata URL** | OIDC discovery endpoint | `https://login.microsoftonline.com/{tenant}/v2.0/.well-known/openid-configuration` |
| **Client ID** | Application ID at IdP | `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` |
| **Client Secret** | Application secret | `*****************` |
| **Scope** | Requested permissions | `openid email profile` |
| **Response Type** | OAuth response | `code` or `id_token` |

### Azure AD OIDC Setup

```
┌────────────────────────────────────────────────────────────────┐
│           Azure AD as OIDC Provider - Setup                    │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  At Customer's Azure AD:                                       │
│  ┌───────────────────────────────────────────────────────┐    │
│  │ 1. Register app for B2C federation                    │    │
│  │ 2. Configure redirect URI to B2C                      │    │
│  │ 3. Create client secret                                │    │
│  │ 4. Grant permissions (openid, email, profile)         │    │
│  │ 5. Share Metadata URL, Client ID, Secret with you     │    │
│  └───────────────────────────────────────────────────────┘    │
│                                                                 │
│  At Your Azure AD B2C:                                        │
│  ┌───────────────────────────────────────────────────────┐    │
│  │ 1. Add new Identity Provider (OpenID Connect)         │    │
│  │ 2. Enter Metadata URL                                  │    │
│  │ 3. Enter Client ID and Secret                          │    │
│  │ 4. Configure claim mappings                            │    │
│  │ 5. Add to User Flow                                    │    │
│  └───────────────────────────────────────────────────────┘    │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

## 📜 SAML 2.0 Federation

### SAML Overview

```
┌────────────────────────────────────────────────────────────────┐
│                    SAML 2.0 Concepts                            │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  B2C acts as Service Provider (SP)                             │
│  Customer IdP acts as Identity Provider (IdP)                  │
│                                                                 │
│  ┌─────────────────┐         ┌─────────────────┐              │
│  │  Azure AD B2C   │  ◄────► │  Customer IdP   │              │
│  │  (SP)           │  SAML   │  (IdP)          │              │
│  │                 │ Assertion│                 │              │
│  └─────────────────┘         └─────────────────┘              │
│                                                                 │
│  Key Components:                                               │
│  • SAML Assertion: XML document with user identity            │
│  • Metadata: XML describing SP and IdP configuration          │
│  • Signing Certificate: Used to verify assertions             │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

### SAML Configuration

| Setting | Description | Example |
|---------|-------------|---------|
| **IdP Metadata URL** | SAML metadata endpoint | `https://idp.example.com/metadata.xml` |
| **Entity ID** | IdP unique identifier | `https://idp.example.com` |
| **SSO URL** | Single Sign-On endpoint | `https://idp.example.com/sso` |
| **Signing Certificate** | X.509 certificate | PEM or CER file |
| **Name ID Format** | User identifier format | `emailAddress` or `persistent` |

### SAML Setup Steps

```
┌────────────────────────────────────────────────────────────────┐
│                SAML Federation Setup                            │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Step 1: Exchange Metadata                                     │
│  ┌────────────────────────────────────────────────────┐       │
│  │  Customer provides:                                  │       │
│  │  • IdP Metadata URL or XML file                     │       │
│  │  • Signing certificate                               │       │
│  │                                                      │       │
│  │  You provide (B2C SP metadata):                     │       │
│  │  • Entity ID: https://<tenant>.b2clogin.com/<tenant>│       │
│  │  • ACS URL (Assertion Consumer Service)             │       │
│  └────────────────────────────────────────────────────┘       │
│                                                                 │
│  Step 2: Configure B2C                                         │
│  ┌────────────────────────────────────────────────────┐       │
│  │  • Add SAML Identity Provider                       │       │
│  │  • Upload/enter IdP metadata                        │       │
│  │  • Configure attribute mappings                     │       │
│  │  • Set response signature requirements              │       │
│  └────────────────────────────────────────────────────┘       │
│                                                                 │
│  Step 3: Configure Customer IdP                                │
│  ┌────────────────────────────────────────────────────┐       │
│  │  • Add B2C as trusted SP                            │       │
│  │  • Configure attribute release policy               │       │
│  │  • Test authentication flow                         │       │
│  └────────────────────────────────────────────────────┘       │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

## 📊 OIDC vs SAML Comparison

| Aspect | OIDC | SAML 2.0 |
|--------|------|----------|
| **Protocol** | JSON-based (JWT) | XML-based |
| **Complexity** | Simpler | More complex |
| **Token Format** | JWT | SAML Assertion |
| **Modern Support** | Excellent | Legacy but widespread |
| **Mobile Friendly** | Yes | Less so |
| **Setup** | Easier | More configuration |
| **Adoption** | Growing | Mature, established |

## 🏢 Common Enterprise IdPs

| IdP | Protocol Support | Notes |
|-----|-----------------|-------|
| **Azure AD** | OIDC, SAML | Native integration |
| **Okta** | OIDC, SAML | Popular cloud IdP |
| **Ping Identity** | OIDC, SAML | Enterprise focused |
| **OneLogin** | OIDC, SAML | Cloud-based |
| **ADFS** | SAML, WS-Fed | Microsoft on-premises |
| **Auth0** | OIDC, SAML | Developer-friendly |
| **Google Workspace** | OIDC, SAML | Business accounts |

## 🔑 Claims Mapping

### Standard SAML Claims

| SAML Attribute | B2C Claim | Description |
|----------------|-----------|-------------|
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` | email | User email |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname` | givenName | First name |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname` | surname | Last name |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` | name | Display name |

### Custom Claims (Custom Policies)

```xml
<ClaimsTransformation Id="ExtractEmployeeId">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="idp_claims" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="employeeId" />
  </OutputClaims>
</ClaimsTransformation>
```

## 🔒 Security Best Practices

| Practice | Description |
|----------|-------------|
| **Verify signatures** | Always validate SAML assertion signatures |
| **Use HTTPS** | All endpoints must use TLS |
| **Validate issuer** | Ensure assertions come from trusted IdP |
| **Check timestamps** | Verify assertion validity period |
| **Rotate secrets** | Periodically rotate client secrets |
| **Certificate management** | Monitor certificate expiration |

## ⚠️ Common Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| **Signature validation failed** | Wrong certificate | Use correct signing cert |
| **Time skew** | Server clocks out of sync | Use NTP, allow clock skew |
| **Missing claims** | IdP not releasing attributes | Configure attribute release |
| **Redirect loop** | Misconfigured URLs | Verify redirect URIs |
| **Certificate expired** | IdP cert expired | Get new certificate |

## 🎯 Exam Tips

> **Enterprise Federation Knowledge:**
> - **OIDC** uses **JWT tokens**, **SAML** uses **XML assertions**
> - **Metadata exchange** is required for both protocols
> - **B2C is the Service Provider (SP)** in SAML
> - **Client ID + Secret** for OIDC, **Certificates** for SAML
> - **Claims mapping** translates IdP claims to B2C claims
> - Enterprise IdPs require **Custom Policies** for advanced scenarios

## 💡 Key Takeaways

1. 🏢 Enterprise IdPs enable **B2B2C scenarios** with corporate credentials
2. 🔄 **OIDC** is modern and simpler, **SAML 2.0** is enterprise standard
3. 📋 **Metadata exchange** is required between B2C and customer IdP
4. 🔑 **OIDC** uses Client ID/Secret, **SAML** uses certificates
5. 📊 **Claims mapping** translates IdP attributes to B2C claims
6. 🔒 Always **validate signatures** and use **HTTPS**
7. ⚠️ **Certificate management** is critical for SAML
8. 📜 **Custom Policies** needed for complex enterprise scenarios
