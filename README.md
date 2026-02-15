# ☁️ Azure AD B2C Study Guide

A comprehensive study guide for Azure Active Directory Business-to-Consumer (Azure AD B2C), designed for exam preparation and practical learning.

## 🎯 What is Azure AD B2C?

Azure AD B2C is a customer identity access management (CIAM) solution that enables:
- **Consumer-facing applications** to authenticate users
- **Social & enterprise logins** (Google, Facebook, Microsoft, etc.)
- **Custom branding** for sign-up/sign-in experiences
- **Scalable identity management** for millions of users

## 📚 Lesson Structure

### 📘 Lesson 01 - Overview & Core Concepts
| # | Topic | Description |
|---|-------|-------------|
| 01 | [What is Azure AD B2C](./lesson-01-overview-core-concepts/01-what-is-azure-ad-b2c/readme.md) | Introduction to Azure AD B2C and CIAM |
| 02 | [B2C vs Azure AD](./lesson-01-overview-core-concepts/02-b2c-vs-azure-ad/readme.md) | Key differences between Azure AD and B2C |
| 03 | [Key Components](./lesson-01-overview-core-concepts/03-key-components/readme.md) | Core components and architecture |

### 🔄 Lesson 02 - User Flows
| # | Topic | Description |
|---|-------|-------------|
| 01 | [Understanding User Flows](./lesson-02-user-flows/01-understanding-user-flows/readme.md) | What are user flows and how they work |
| 02 | [User Flow Types](./lesson-02-user-flows/02-user-flow-types/readme.md) | Sign-up, sign-in, profile edit, password reset |
| 03 | [Configuring User Flows](./lesson-02-user-flows/03-configuring-user-flows/readme.md) | Step-by-step configuration guide |

### 🔑 Lesson 03 - Identity Providers
| # | Topic | Description |
|---|-------|-------------|
| 01 | [Identity Provider Types](./lesson-03-identity-providers/01-identity-provider-types/readme.md) | Overview of supported identity providers |
| 02 | [Social Identity Providers](./lesson-03-identity-providers/02-social-identity-providers/readme.md) | Google, Facebook, Microsoft, Apple |
| 03 | [Enterprise Identity Providers](./lesson-03-identity-providers/03-enterprise-identity-providers/readme.md) | SAML, OpenID Connect, WS-Federation |

### 📜 Lesson 04 - Custom Policies
| # | Topic | Description |
|---|-------|-------------|
| 01 | [Identity Experience Framework](./lesson-04-custom-policies/01-identity-experience-framework/readme.md) | IEF overview and capabilities |
| 02 | [Policy Structure](./lesson-04-custom-policies/02-policy-structure/readme.md) | XML policy structure and elements |
| 03 | [Custom Policy Examples](./lesson-04-custom-policies/03-custom-policy-examples/readme.md) | Common custom policy scenarios |

### 🎫 Lesson 05 - Tokens & Claims
| # | Topic | Description |
|---|-------|-------------|
| 01 | [Token Types](./lesson-05-tokens-claims/01-token-types/readme.md) | ID tokens, access tokens, refresh tokens |
| 02 | [Claims Customization](./lesson-05-tokens-claims/02-claims-customization/readme.md) | Adding and transforming claims |
| 03 | [Token Lifetimes](./lesson-05-tokens-claims/03-token-lifetimes/readme.md) | Configuring token expiration |

### 🖥️ Lesson 06 - Application Integration
| # | Topic | Description |
|---|-------|-------------|
| 01 | [App Registration](./lesson-06-application-integration/01-app-registration/readme.md) | Registering applications in B2C |
| 02 | [Authentication Libraries](./lesson-06-application-integration/02-authentication-libraries/readme.md) | MSAL and other libraries |
| 03 | [API Protection](./lesson-06-application-integration/03-api-protection/readme.md) | Securing APIs with B2C |

### 🛡️ Lesson 07 - Security Features
| # | Topic | Description |
|---|-------|-------------|
| 01 | [MFA Configuration](./lesson-07-security-features/01-mfa-configuration/readme.md) | Multi-factor authentication setup |
| 02 | [Conditional Access](./lesson-07-security-features/02-conditional-access/readme.md) | Risk-based access policies |
| 03 | [Identity Protection](./lesson-07-security-features/03-identity-protection/readme.md) | Fraud detection and prevention |

### 🎨 Lesson 08 - Customization & Branding
| # | Topic | Description |
|---|-------|-------------|
| 01 | [UI Customization](./lesson-08-customization-branding/01-ui-customization/readme.md) | Customizing the user interface |
| 02 | [Page Layouts](./lesson-08-customization-branding/02-page-layouts/readme.md) | Built-in and custom page layouts |
| 03 | [Custom Domains](./lesson-08-customization-branding/03-custom-domains/readme.md) | Using your own domain name |

### 📊 Lesson 09 - Monitoring & Troubleshooting
| # | Topic | Description |
|---|-------|-------------|
| 01 | [Sign-in Logs](./lesson-09-monitoring-troubleshooting/01-sign-in-logs/readme.md) | Analyzing sign-in activities |
| 02 | [Audit Logs](./lesson-09-monitoring-troubleshooting/02-audit-logs/readme.md) | Tracking administrative changes |
| 03 | [Common Issues](./lesson-09-monitoring-troubleshooting/03-common-issues/readme.md) | Troubleshooting common problems |

### 💰 Lesson 10 - Pricing & Best Practices
| # | Topic | Description |
|---|-------|-------------|
| 01 | [MAU Pricing](./lesson-10-pricing-best-practices/01-mau-pricing/readme.md) | Monthly Active Users pricing model |
| 02 | [Production Guidelines](./lesson-10-pricing-best-practices/02-production-guidelines/readme.md) | Best practices for production |
| 03 | [Migration Strategies](./lesson-10-pricing-best-practices/03-migration-strategies/readme.md) | Migrating to Azure AD B2C |

## 📁 Repository Structure

### 📦 Root Files
- 📄 [README.md](./README.md)
- 📄 [CLAUDE.md](./CLAUDE.md)

### 📁 lesson-01-overview-core-concepts
- **01-what-is-azure-ad-b2c/** — [📖 readme.md](./lesson-01-overview-core-concepts/01-what-is-azure-ad-b2c/readme.md) · [🎨 diagram.drawio](./lesson-01-overview-core-concepts/01-what-is-azure-ad-b2c/diagram.drawio) · [🖼️ diagram.png](./lesson-01-overview-core-concepts/01-what-is-azure-ad-b2c/diagram.png)
- **02-b2c-vs-azure-ad/** — [📖 readme.md](./lesson-01-overview-core-concepts/02-b2c-vs-azure-ad/readme.md) · [🎨 diagram.drawio](./lesson-01-overview-core-concepts/02-b2c-vs-azure-ad/diagram.drawio) · [🖼️ diagram.png](./lesson-01-overview-core-concepts/02-b2c-vs-azure-ad/diagram.png)
- **03-key-components/** — [📖 readme.md](./lesson-01-overview-core-concepts/03-key-components/readme.md) · [🎨 diagram.drawio](./lesson-01-overview-core-concepts/03-key-components/diagram.drawio) · [🖼️ diagram.png](./lesson-01-overview-core-concepts/03-key-components/diagram.png)

### 📁 lesson-02-user-flows
- **01-understanding-user-flows/** — [📖 readme.md](./lesson-02-user-flows/01-understanding-user-flows/readme.md) · [🎨 diagram.drawio](./lesson-02-user-flows/01-understanding-user-flows/diagram.drawio) · [🖼️ diagram.png](./lesson-02-user-flows/01-understanding-user-flows/diagram.png)
- **02-user-flow-types/** — [📖 readme.md](./lesson-02-user-flows/02-user-flow-types/readme.md) · [🎨 diagram.drawio](./lesson-02-user-flows/02-user-flow-types/diagram.drawio) · [🖼️ diagram.png](./lesson-02-user-flows/02-user-flow-types/diagram.png)
- **03-configuring-user-flows/** — [📖 readme.md](./lesson-02-user-flows/03-configuring-user-flows/readme.md) · [🎨 diagram.drawio](./lesson-02-user-flows/03-configuring-user-flows/diagram.drawio) · [🖼️ diagram.png](./lesson-02-user-flows/03-configuring-user-flows/diagram.png)

### 📁 lesson-03-identity-providers
- **01-identity-provider-types/** — [📖 readme.md](./lesson-03-identity-providers/01-identity-provider-types/readme.md) · [🎨 diagram.drawio](./lesson-03-identity-providers/01-identity-provider-types/diagram.drawio) · [🖼️ diagram.png](./lesson-03-identity-providers/01-identity-provider-types/diagram.png)
- **02-social-identity-providers/** — [📖 readme.md](./lesson-03-identity-providers/02-social-identity-providers/readme.md) · [🎨 diagram.drawio](./lesson-03-identity-providers/02-social-identity-providers/diagram.drawio) · [🖼️ diagram.png](./lesson-03-identity-providers/02-social-identity-providers/diagram.png)
- **03-enterprise-identity-providers/** — [📖 readme.md](./lesson-03-identity-providers/03-enterprise-identity-providers/readme.md) · [🎨 diagram.drawio](./lesson-03-identity-providers/03-enterprise-identity-providers/diagram.drawio) · [🖼️ diagram.png](./lesson-03-identity-providers/03-enterprise-identity-providers/diagram.png)

### 📁 lesson-04-custom-policies
- **01-identity-experience-framework/** — [📖 readme.md](./lesson-04-custom-policies/01-identity-experience-framework/readme.md) · [🎨 diagram.drawio](./lesson-04-custom-policies/01-identity-experience-framework/diagram.drawio) · [🖼️ diagram.png](./lesson-04-custom-policies/01-identity-experience-framework/diagram.png)
- **02-policy-structure/** — [📖 readme.md](./lesson-04-custom-policies/02-policy-structure/readme.md) · [🎨 diagram.drawio](./lesson-04-custom-policies/02-policy-structure/diagram.drawio) · [🖼️ diagram.png](./lesson-04-custom-policies/02-policy-structure/diagram.png)
- **03-custom-policy-examples/** — [📖 readme.md](./lesson-04-custom-policies/03-custom-policy-examples/readme.md) · [🎨 diagram.drawio](./lesson-04-custom-policies/03-custom-policy-examples/diagram.drawio) · [🖼️ diagram.png](./lesson-04-custom-policies/03-custom-policy-examples/diagram.png)

### 📁 lesson-05-tokens-claims
- **01-token-types/** — [📖 readme.md](./lesson-05-tokens-claims/01-token-types/readme.md) · [🎨 diagram.drawio](./lesson-05-tokens-claims/01-token-types/diagram.drawio) · [🖼️ diagram.png](./lesson-05-tokens-claims/01-token-types/diagram.png)
- **02-claims-customization/** — [📖 readme.md](./lesson-05-tokens-claims/02-claims-customization/readme.md) · [🎨 diagram.drawio](./lesson-05-tokens-claims/02-claims-customization/diagram.drawio) · [🖼️ diagram.png](./lesson-05-tokens-claims/02-claims-customization/diagram.png)
- **03-token-lifetimes/** — [📖 readme.md](./lesson-05-tokens-claims/03-token-lifetimes/readme.md) · [🎨 diagram.drawio](./lesson-05-tokens-claims/03-token-lifetimes/diagram.drawio) · [🖼️ diagram.png](./lesson-05-tokens-claims/03-token-lifetimes/diagram.png)

### 📁 lesson-06-application-integration
- **01-app-registration/** — [📖 readme.md](./lesson-06-application-integration/01-app-registration/readme.md) · [🎨 diagram.drawio](./lesson-06-application-integration/01-app-registration/diagram.drawio) · [🖼️ diagram.png](./lesson-06-application-integration/01-app-registration/diagram.png)
- **02-authentication-libraries/** — [📖 readme.md](./lesson-06-application-integration/02-authentication-libraries/readme.md) · [🎨 diagram.drawio](./lesson-06-application-integration/02-authentication-libraries/diagram.drawio) · [🖼️ diagram.png](./lesson-06-application-integration/02-authentication-libraries/diagram.png)
- **03-api-protection/** — [📖 readme.md](./lesson-06-application-integration/03-api-protection/readme.md) · [🎨 diagram.drawio](./lesson-06-application-integration/03-api-protection/diagram.drawio) · [🖼️ diagram.png](./lesson-06-application-integration/03-api-protection/diagram.png)

### 📁 lesson-07-security-features
- **01-mfa-configuration/** — [📖 readme.md](./lesson-07-security-features/01-mfa-configuration/readme.md) · [🎨 diagram.drawio](./lesson-07-security-features/01-mfa-configuration/diagram.drawio) · [🖼️ diagram.png](./lesson-07-security-features/01-mfa-configuration/diagram.png)
- **02-conditional-access/** — [📖 readme.md](./lesson-07-security-features/02-conditional-access/readme.md) · [🎨 diagram.drawio](./lesson-07-security-features/02-conditional-access/diagram.drawio) · [🖼️ diagram.png](./lesson-07-security-features/02-conditional-access/diagram.png)
- **03-identity-protection/** — [📖 readme.md](./lesson-07-security-features/03-identity-protection/readme.md) · [🎨 diagram.drawio](./lesson-07-security-features/03-identity-protection/diagram.drawio) · [🖼️ diagram.png](./lesson-07-security-features/03-identity-protection/diagram.png)

### 📁 lesson-08-customization-branding
- **01-ui-customization/** — [📖 readme.md](./lesson-08-customization-branding/01-ui-customization/readme.md) · [🎨 diagram.drawio](./lesson-08-customization-branding/01-ui-customization/diagram.drawio) · [🖼️ diagram.png](./lesson-08-customization-branding/01-ui-customization/diagram.png)
- **02-page-layouts/** — [📖 readme.md](./lesson-08-customization-branding/02-page-layouts/readme.md) · [🎨 diagram.drawio](./lesson-08-customization-branding/02-page-layouts/diagram.drawio) · [🖼️ diagram.png](./lesson-08-customization-branding/02-page-layouts/diagram.png)
- **03-custom-domains/** — [📖 readme.md](./lesson-08-customization-branding/03-custom-domains/readme.md) · [🎨 diagram.drawio](./lesson-08-customization-branding/03-custom-domains/diagram.drawio) · [🖼️ diagram.png](./lesson-08-customization-branding/03-custom-domains/diagram.png)

### 📁 lesson-09-monitoring-troubleshooting
- **01-sign-in-logs/** — [📖 readme.md](./lesson-09-monitoring-troubleshooting/01-sign-in-logs/readme.md) · [🎨 diagram.drawio](./lesson-09-monitoring-troubleshooting/01-sign-in-logs/diagram.drawio) · [🖼️ diagram.png](./lesson-09-monitoring-troubleshooting/01-sign-in-logs/diagram.png)
- **02-audit-logs/** — [📖 readme.md](./lesson-09-monitoring-troubleshooting/02-audit-logs/readme.md) · [🎨 diagram.drawio](./lesson-09-monitoring-troubleshooting/02-audit-logs/diagram.drawio) · [🖼️ diagram.png](./lesson-09-monitoring-troubleshooting/02-audit-logs/diagram.png)
- **03-common-issues/** — [📖 readme.md](./lesson-09-monitoring-troubleshooting/03-common-issues/readme.md) · [🎨 diagram.drawio](./lesson-09-monitoring-troubleshooting/03-common-issues/diagram.drawio) · [🖼️ diagram.png](./lesson-09-monitoring-troubleshooting/03-common-issues/diagram.png)

### 📁 lesson-10-pricing-best-practices
- **01-mau-pricing/** — [📖 readme.md](./lesson-10-pricing-best-practices/01-mau-pricing/readme.md) · [🎨 diagram.drawio](./lesson-10-pricing-best-practices/01-mau-pricing/diagram.drawio) · [🖼️ diagram.png](./lesson-10-pricing-best-practices/01-mau-pricing/diagram.png)
- **02-production-guidelines/** — [📖 readme.md](./lesson-10-pricing-best-practices/02-production-guidelines/readme.md) · [🎨 diagram.drawio](./lesson-10-pricing-best-practices/02-production-guidelines/diagram.drawio) · [🖼️ diagram.png](./lesson-10-pricing-best-practices/02-production-guidelines/diagram.png)
- **03-migration-strategies/** — [📖 readme.md](./lesson-10-pricing-best-practices/03-migration-strategies/readme.md) · [🎨 diagram.drawio](./lesson-10-pricing-best-practices/03-migration-strategies/diagram.drawio) · [🖼️ diagram.png](./lesson-10-pricing-best-practices/03-migration-strategies/diagram.png)

## 🔐 Key Concepts Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                        AZURE AD B2C                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   👤 Consumer Users          🖥️ Your Applications              │
│        │                            │                          │
│        ▼                            ▼                          │
│   ┌─────────┐               ┌──────────────┐                   │
│   │ Sign-up │               │ App Registration│                │
│   │ Sign-in │◄─────────────►│ (Client ID)    │                 │
│   │ Profile │               └──────────────┘                   │
│   └─────────┘                                                  │
│        │                                                       │
│        ▼                                                       │
│   ┌─────────────────────────────────────────┐                  │
│   │           User Flows / Policies          │                 │
│   │  ┌──────────┐ ┌──────────┐ ┌──────────┐ │                  │
│   │  │ Sign-up  │ │ Sign-in  │ │ Password │ │                  │
│   │  │   Flow   │ │   Flow   │ │  Reset   │ │                  │
│   │  └──────────┘ └──────────┘ └──────────┘ │                  │
│   └─────────────────────────────────────────┘                  │
│        │                                                       │
│        ▼                                                       │
│   ┌─────────────────────────────────────────┐                  │
│   │         Identity Providers               │                 │
│   │  🔗 Google  🔗 Facebook  🔗 Microsoft   │                  │
│   │  🔗 Apple   🔗 SAML      🔗 OpenID      │                  │
│   └─────────────────────────────────────────┘                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## ⚡ Quick Comparison: Azure AD vs Azure AD B2C

| Feature | Azure AD | Azure AD B2C |
|---------|----------|--------------|
| **Target Users** | Employees, partners | Consumers, customers |
| **User Management** | IT-managed | Self-service |
| **Identity Providers** | Enterprise (SAML, WS-Fed) | Social + Enterprise |
| **Customization** | Limited | Fully customizable UI |
| **Pricing** | Per user/license | Per Monthly Active User (MAU) |
| **Use Case** | Internal apps | Customer-facing apps |

## 🎯 Exam Focus Areas

- ✅ Understand when to use Azure AD B2C vs Azure AD
- ✅ Know the difference between User Flows and Custom Policies
- ✅ Configure identity providers (social and enterprise)
- ✅ Understand token types and claims
- ✅ Implement MFA and Conditional Access
- ✅ Application registration and integration

## 🛠️ Prerequisites

- Basic understanding of OAuth 2.0 and OpenID Connect
- Familiarity with Azure Portal
- Knowledge of web application authentication concepts

## 📖 How to Use This Guide

1. **Start with Lesson 01** to understand core concepts
2. **Follow lessons in order** - each builds on previous knowledge
3. **Review diagrams** in Draw.io for visual understanding
4. **Focus on 🎯 Exam Tips** sections for test preparation
5. **Practice** in Azure Portal with a free B2C tenant

## 🔗 Useful Resources

- [Azure AD B2C Documentation](https://docs.microsoft.com/azure/active-directory-b2c/)
- [Azure AD B2C Samples](https://github.com/azure-ad-b2c/samples)
- [Identity Experience Framework](https://docs.microsoft.com/azure/active-directory-b2c/custom-policy-overview)

---

💡 **Tip:** First 50,000 MAU per month are FREE in Azure AD B2C!

