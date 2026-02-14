# ☁️ What is Azure AD B2C?

## 🔑 Overview

**Azure Active Directory B2C (Business-to-Consumer)** is a customer identity and access management (CIAM) solution that enables you to build secure, scalable authentication experiences for your consumer-facing applications.

```
┌─────────────────────────────────────────────────────────────────────┐
│                        🏢 Azure AD B2C                              │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │                    CIAM Solution                             │   │
│  │                                                              │   │
│  │   👤 Millions of Consumers    🔐 Secure Authentication      │   │
│  │   🎨 Custom Branding          🔗 Social Logins              │   │
│  │   🛡️ Enterprise Security      📈 Scalable                   │   │
│  │                                                              │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                              │                                      │
│              ┌───────────────┼───────────────┐                      │
│              ▼               ▼               ▼                      │
│        ┌─────────┐     ┌─────────┐     ┌─────────┐                 │
│        │ 🖥️ Web  │     │ 📱 Mobile│     │ 🔌 API  │                 │
│        │  Apps   │     │  Apps   │     │Services │                 │
│        └─────────┘     └─────────┘     └─────────┘                 │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Definition

| Term | Description |
|------|-------------|
| **Azure AD B2C** | A separate service from Azure AD designed specifically for consumer identity management |
| **CIAM** | Customer Identity and Access Management - managing external user identities |
| **Tenant** | A dedicated Azure AD B2C directory that represents your organization |
| **User Flow** | Pre-built, configurable authentication experiences (sign-up, sign-in, etc.) |
| **Custom Policy** | Advanced XML-based policies for complex identity scenarios |

## 🔗 Key Features

| Feature | Description | Use Case |
|---------|-------------|----------|
| 🔑 **Social Identity** | Sign in with Google, Facebook, Apple, etc. | Consumer convenience |
| 🏢 **Enterprise Identity** | Connect SAML/OIDC enterprise IdPs | B2B2C scenarios |
| 📧 **Local Accounts** | Email/password or phone number sign-up | Direct registration |
| 🎨 **Custom UI** | Full control over authentication pages | Brand consistency |
| 🛡️ **MFA** | Multi-factor authentication | Enhanced security |
| 📊 **Analytics** | Sign-in logs and user insights | Compliance & monitoring |

## 👥 Who Uses Azure AD B2C?

```
┌──────────────────────────────────────────────────────────────┐
│                       Use Cases                               │
├──────────────────────────────────────────────────────────────┤
│                                                               │
│  🛒 E-Commerce        → Customer accounts & checkout          │
│  🏥 Healthcare        → Patient portals                       │
│  🏦 Banking           → Consumer banking apps                 │
│  🎮 Gaming            → Player accounts & profiles            │
│  📺 Media             → Streaming subscriptions               │
│  🏛️ Government        → Citizen services                      │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

## 📈 Scale & Performance

| Metric | Capability |
|--------|------------|
| **Users** | Millions of consumer accounts |
| **Authentications** | Billions of authentications per day |
| **Availability** | 99.9% SLA |
| **Global Reach** | Available in all Azure regions |

## 🎯 Exam Tips

> **Remember:**
> - Azure AD B2C is a **separate service** from Azure AD (not the same tenant)
> - Designed for **external/consumer** identities, not employees
> - Supports **social logins** out of the box
> - Uses **User Flows** for simple scenarios, **Custom Policies** for complex ones
> - Pricing is based on **Monthly Active Users (MAU)**

## 💡 Key Takeaways

1. 🔑 Azure AD B2C is a CIAM solution for consumer-facing applications
2. 🏢 It's a separate tenant/service from your organizational Azure AD
3. 👤 Supports local accounts, social identities, and enterprise federation
4. 📈 Scales to millions of users with enterprise-grade security
5. 🎨 Provides full customization of authentication experiences
6. 💰 Billing is based on Monthly Active Users (MAU), not total users
