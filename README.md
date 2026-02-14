# ☁️ Azure AD B2C Study Guide

A comprehensive study guide for Azure Active Directory Business-to-Consumer (Azure AD B2C), designed for exam preparation and practical learning.

## 🎯 What is Azure AD B2C?

Azure AD B2C is a customer identity access management (CIAM) solution that enables:
- **Consumer-facing applications** to authenticate users
- **Social & enterprise logins** (Google, Facebook, Microsoft, etc.)
- **Custom branding** for sign-up/sign-in experiences
- **Scalable identity management** for millions of users

## 📚 Lesson Structure

| Lesson | Topic | Description |
|--------|-------|-------------|
| [01](./lesson-01-overview-core-concepts/) | 🏢 Overview & Core Concepts | Introduction to Azure AD B2C, use cases, vs Azure AD |
| [02](./lesson-02-user-flows/) | 🔄 User Flows | Built-in user flows (sign-up, sign-in, profile edit, password reset) |
| [03](./lesson-03-identity-providers/) | 🔑 Identity Providers | Social & enterprise identity providers configuration |
| 04 | 📜 Custom Policies | Identity Experience Framework, XML policies |
| 05 | 🎫 Tokens & Claims | JWT tokens, claims customization, token lifetimes |
| 06 | 🖥️ Application Integration | App registration, MSAL, authentication libraries |
| 07 | 🛡️ Security Features | MFA, Conditional Access, fraud protection |
| 08 | 🎨 Customization & Branding | UI customization, page layouts, custom domains |
| 09 | 📊 Monitoring & Troubleshooting | Sign-in logs, audit logs, diagnostics |
| 10 | 💰 Pricing & Best Practices | MAU pricing, production guidelines |

## 📁 Repository Structure

```
lesson-XX-topic-name/
  ├── 01-subtopic/
  │   ├── readme.md      # 📖 Study content with tables, ASCII diagrams, exam tips
  │   └── diagram.drawio # 🎨 Visual diagram in Draw.io XML format
  └── 02-subtopic/
      └── ...
```

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

