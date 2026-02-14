# ✅ Production Guidelines

## 🔑 Overview

Moving Azure AD B2C to production requires careful planning. This guide covers best practices for reliability, security, and maintainability in production environments.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    ✅ Production Readiness Checklist                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   🔒 Security          📈 Monitoring         🔄 Reliability         │
│   ┌────────────┐      ┌────────────┐       ┌────────────┐          │
│   │ ☑️ MFA      │      │ ☑️ Logs     │       │ ☑️ Backup   │          │
│   │ ☑️ HTTPS    │      │ ☑️ Alerts   │       │ ☑️ DR Plan  │          │
│   │ ☑️ Tokens   │      │ ☑️ Metrics  │       │ ☑️ Testing  │          │
│   │ ☑️ Secrets  │      │ ☑️ Insights │       │ ☑️ Rollback │          │
│   └────────────┘      └────────────┘       └────────────┘          │
│                                                                      │
│   🏗️ Architecture     📚 Documentation     🧪 Testing              │
│   ┌────────────┐      ┌────────────┐       ┌────────────┐          │
│   │ ☑️ Scaling  │      │ ☑️ Runbooks │       │ ☑️ E2E Tests│          │
│   │ ☑️ HA       │      │ ☑️ Contacts │       │ ☑️ Load Test│          │
│   │ ☑️ Regions  │      │ ☑️ Policies │       │ ☑️ Security │          │
│   └────────────┘      └────────────┘       └────────────┘          │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔒 Security Best Practices

### Application Security

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔒 Application Security                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ✅ Do:                                                             │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  • Use HTTPS everywhere (never HTTP)                         │   │
│   │  • Use certificates instead of client secrets                │   │
│   │  • Rotate secrets before expiration                          │   │
│   │  • Store secrets in Key Vault                                │   │
│   │  • Validate tokens properly (signature, issuer, audience)   │   │
│   │  • Use PKCE for all public clients                          │   │
│   │  • Implement proper logout (clear tokens, revoke)           │   │
│   │  • Set appropriate token lifetimes                           │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   ❌ Don't:                                                          │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  • Store tokens in localStorage (use sessionStorage/memory)  │   │
│   │  • Log full tokens or secrets                                │   │
│   │  • Use implicit grant flow (deprecated)                      │   │
│   │  • Skip token validation                                     │   │
│   │  • Use long-lived secrets without rotation                   │   │
│   │  • Trust client-side claims without validation               │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Secret Management

| Practice | Implementation |
|----------|----------------|
| **Store in Key Vault** | Never in code or config files |
| **Rotate regularly** | Before 25% of expiry period |
| **Use managed identities** | Avoid credentials where possible |
| **Audit access** | Enable Key Vault logging |
| **Separate per environment** | Different secrets for dev/prod |

### MFA Configuration

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔐 MFA Recommendations                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Require MFA for:                                                   │
│   ✅ Sensitive operations (payments, account changes)               │
│   ✅ Admin access                                                    │
│   ✅ High-risk sign-ins (Conditional Access)                        │
│   ✅ New device/location                                             │
│                                                                      │
│   MFA Method Priority:                                               │
│   1. 🥇 Authenticator App (TOTP) - Most secure, free                │
│   2. 🥈 FIDO2 security keys - Highest security, hardware cost      │
│   3. 🥉 SMS/Voice - Acceptable, but has cost and vulnerabilities   │
│                                                                      │
│   Remember device settings:                                          │
│   • Consider allowing "remember MFA" for 14-30 days                 │
│   • Balance security vs user experience                             │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📈 Monitoring Setup

### Essential Monitoring

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📈 Monitoring Configuration                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   1. Enable Diagnostic Settings                                     │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Azure AD B2C → Diagnostic settings → Add                   │   │
│   │                                                              │   │
│   │  Logs to capture:                                            │   │
│   │  ☑️ AuditLogs                                                 │   │
│   │  ☑️ SignInLogs                                                │   │
│   │                                                              │   │
│   │  Send to:                                                    │   │
│   │  ☑️ Log Analytics workspace (recommended)                    │   │
│   │  ☑️ Storage account (long-term archive)                      │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   2. Configure Alerts                                               │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Critical alerts to set up:                                  │   │
│   │  • High failure rate (>10% sign-ins failing)                │   │
│   │  • Policy errors                                             │   │
│   │  • Unusual sign-in patterns                                  │   │
│   │  • Admin operations                                          │   │
│   │  • Certificate expiration warnings                           │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   3. Application Insights (Custom Policies)                         │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  DeveloperMode="false"  (for production)                    │   │
│   │  ClientEnabled="true"                                        │   │
│   │  ServerEnabled="true"                                        │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Key Metrics to Monitor

| Metric | Warning Threshold | Critical Threshold |
|--------|-------------------|-------------------|
| **Sign-in failure rate** | > 5% | > 15% |
| **Average latency** | > 2 seconds | > 5 seconds |
| **MFA failure rate** | > 10% | > 25% |
| **Token issuance errors** | Any | Any |
| **Policy execution errors** | Any | Any |

## 🏗️ Architecture Recommendations

### Environment Separation

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🏗️ Environment Strategy                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Option 1: Separate Tenants (Recommended for isolation)            │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  contoso-dev.onmicrosoft.com    → Development               │   │
│   │  contoso-staging.onmicrosoft.com → Staging/QA               │   │
│   │  contoso.onmicrosoft.com         → Production               │   │
│   │                                                              │   │
│   │  ✅ Complete isolation                                       │   │
│   │  ✅ Separate billing                                         │   │
│   │  ❌ More management overhead                                 │   │
│   │  ❌ Policy sync needed                                       │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   Option 2: Single Tenant with Policy Prefixes                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  B2C_1_dev_SignUpSignIn    → Development                    │   │
│   │  B2C_1_staging_SignUpSignIn → Staging                        │   │
│   │  B2C_1_SignUpSignIn         → Production                     │   │
│   │                                                              │   │
│   │  ✅ Simpler management                                       │   │
│   │  ✅ Single tenant to monitor                                 │   │
│   │  ❌ Shared user directory                                    │   │
│   │  ❌ Risk of production impact                                │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### High Availability

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔄 High Availability                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Azure AD B2C is a global service with built-in HA:               │
│                                                                      │
│   ✅ Multi-region deployment (Azure manages)                        │
│   ✅ Automatic failover                                              │
│   ✅ 99.9% SLA                                                       │
│                                                                      │
│   Your responsibilities:                                             │
│                                                                      │
│   1. Custom HTML hosting                                            │
│      • Use CDN (Azure CDN, CloudFront)                              │
│      • Multiple region storage accounts                             │
│      • Failover configuration                                        │
│                                                                      │
│   2. REST API endpoints                                              │
│      • Deploy to multiple regions                                    │
│      • Use load balancer/Traffic Manager                            │
│      • Health probes and auto-failover                              │
│                                                                      │
│   3. Custom domains                                                  │
│      • Azure Front Door provides global edge                        │
│      • Built-in DDoS protection                                     │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📚 Documentation Requirements

| Document | Content | Owner |
|----------|---------|-------|
| **Runbook** | Step-by-step procedures | Operations |
| **Architecture** | System design, dependencies | Architecture |
| **DR Plan** | Recovery procedures | Operations |
| **Contact List** | Escalation paths | Management |
| **Policy Inventory** | All policies and purposes | Development |
| **Secret Inventory** | All secrets and expiry dates | Security |

## 🧪 Testing Requirements

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🧪 Testing Checklist                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Functional Testing:                                                │
│   ☐ Sign-up flow (all identity providers)                          │
│   ☐ Sign-in flow (local and social accounts)                       │
│   ☐ Password reset                                                  │
│   ☐ Profile editing                                                 │
│   ☐ MFA enrollment and verification                                │
│   ☐ Token claims validation                                        │
│                                                                      │
│   Security Testing:                                                  │
│   ☐ Token validation in APIs                                       │
│   ☐ CORS configuration                                              │
│   ☐ Redirect URI validation                                        │
│   ☐ Session timeout behavior                                       │
│   ☐ Brute force protection                                         │
│                                                                      │
│   Performance Testing:                                               │
│   ☐ Load test authentication flow                                  │
│   ☐ Concurrent user simulation                                     │
│   ☐ API response times under load                                  │
│                                                                      │
│   Disaster Recovery:                                                 │
│   ☐ Custom HTML failover                                            │
│   ☐ REST API failover                                               │
│   ☐ Certificate rotation procedure                                 │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Go-Live Checklist

| Category | Item | Status |
|----------|------|--------|
| **Security** | MFA enabled | ☐ |
| **Security** | Secrets in Key Vault | ☐ |
| **Security** | HTTPS only | ☐ |
| **Security** | Token validation | ☐ |
| **Monitoring** | Diagnostic settings | ☐ |
| **Monitoring** | Alerts configured | ☐ |
| **Reliability** | Custom HTML backed up | ☐ |
| **Reliability** | DR plan documented | ☐ |
| **Testing** | E2E tests passing | ☐ |
| **Testing** | Load test completed | ☐ |
| **Documentation** | Runbooks ready | ☐ |
| **Documentation** | Contact list updated | ☐ |

## 🎯 Exam Tips

> **Remember:**
> - Production B2C needs **diagnostic settings** enabled
> - Store secrets in **Azure Key Vault**
> - Use **certificates** instead of secrets when possible
> - **Separate environments** (tenants or policy prefixes)
> - Enable **MFA** for sensitive operations
> - **Monitor** sign-in failures and policy errors
> - Have a **DR plan** for custom components

## 💡 Key Takeaways

1. ✅ Security first: HTTPS, MFA, Key Vault
2. 📈 Enable comprehensive monitoring and alerting
3. 🏗️ Separate dev/staging/production environments
4. 📚 Document everything: runbooks, contacts, DR plan
5. 🧪 Test thoroughly before go-live
6. 🔄 Plan for high availability of custom components
