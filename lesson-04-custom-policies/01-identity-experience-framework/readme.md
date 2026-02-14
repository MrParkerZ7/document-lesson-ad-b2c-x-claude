# 📜 Identity Experience Framework (IEF)

## 🔑 Overview

The **Identity Experience Framework (IEF)** is the underlying engine that powers Azure AD B2C custom policies. It provides a highly configurable platform for building complex identity journeys that go beyond standard user flows.

```
┌─────────────────────────────────────────────────────────────────────┐
│                 🏗️ Identity Experience Framework                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌──────────────────────────────────────────────────────────────┐  │
│   │                    📜 Custom Policies                         │  │
│   │                     (XML Configuration)                       │  │
│   └──────────────────────────────────────────────────────────────┘  │
│                              │                                       │
│              ┌───────────────┼───────────────┐                       │
│              ▼               ▼               ▼                       │
│   ┌──────────────┐  ┌──────────────┐  ┌──────────────┐              │
│   │ 🔗 Claims    │  │ 🔄 User      │  │ 📞 Technical │              │
│   │   Providers  │  │   Journeys   │  │   Profiles   │              │
│   └──────────────┘  └──────────────┘  └──────────────┘              │
│              │               │               │                       │
│              └───────────────┼───────────────┘                       │
│                              ▼                                       │
│   ┌──────────────────────────────────────────────────────────────┐  │
│   │              ⚙️ Trust Framework Engine                        │  │
│   │         (Orchestrates Identity Operations)                    │  │
│   └──────────────────────────────────────────────────────────────┘  │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Key Concepts

| Component | Description | Purpose |
|-----------|-------------|---------|
| **Trust Framework** | The policy engine that processes custom policies | Orchestration engine |
| **Base Policy** | Foundation policy with common definitions | Reusable building blocks |
| **Extension Policy** | Extends base with custom logic | Organization-specific config |
| **Relying Party** | Entry point policy for applications | Application interface |
| **Claims Schema** | Defines data attributes | Identity data structure |

## 🔄 User Flows vs Custom Policies

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Comparison Overview                               │
├────────────────────────────┬────────────────────────────────────────┤
│      🔄 User Flows         │       📜 Custom Policies               │
├────────────────────────────┼────────────────────────────────────────┤
│  ✅ Simple configuration   │  ✅ Full customization                 │
│  ✅ Azure Portal UI        │  ✅ XML-based configuration            │
│  ✅ Quick setup            │  ✅ Complex identity scenarios         │
│  ❌ Limited flexibility    │  ❌ Steeper learning curve             │
│  ❌ No external API calls  │  ✅ REST API integrations              │
│  ❌ Fixed journey flow     │  ✅ Branching logic                    │
└────────────────────────────┴────────────────────────────────────────┘
```

## 🛠️ When to Use Custom Policies

| Scenario | Use Case | Example |
|----------|----------|---------|
| 🔗 **External API Integration** | Call REST APIs during authentication | Fraud detection, CRM lookup |
| 🔀 **Complex Branching** | Conditional flows based on user data | Different flows by region |
| 🔄 **Account Migration** | Just-in-time user migration | Legacy system integration |
| 🎫 **Custom Claims** | Transform or enrich claims | Role mapping, group claims |
| 🔐 **Multi-IdP Linking** | Link multiple identities | Social + local account |
| 📧 **Custom Verification** | Email/phone verification logic | Custom OTP providers |

## 📦 IEF Components

```
┌─────────────────────────────────────────────────────────────────────┐
│                    IEF Architecture                                  │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │                   📜 Policy Files                            │    │
│  │  ┌───────────┐   ┌───────────┐   ┌───────────────────────┐  │    │
│  │  │TrustFrame-│   │TrustFrame-│   │  Relying Party        │  │    │
│  │  │workBase   │──►│workExtens-│──►│  SignUpSignIn         │  │    │
│  │  │.xml       │   │ions.xml   │   │  .xml                 │  │    │
│  │  └───────────┘   └───────────┘   └───────────────────────┘  │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                              │                                       │
│                              ▼                                       │
│  ┌─────────────────────────────────────────────────────────────┐    │
│  │                 🔑 Policy Keys                               │    │
│  │    ┌──────────┐  ┌──────────┐  ┌──────────┐                 │    │
│  │    │ Signing  │  │ Encrypt- │  │ IdP      │                 │    │
│  │    │   Key    │  │ ion Key  │  │ Secrets  │                 │    │
│  │    └──────────┘  └──────────┘  └──────────┘                 │    │
│  └─────────────────────────────────────────────────────────────┘    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔑 Required Policy Keys

| Key Name | Type | Purpose |
|----------|------|---------|
| **TokenSigningKeyContainer** | RSA | Signs issued tokens |
| **TokenEncryptionKeyContainer** | RSA | Encrypts tokens |
| **B2C_1A_FacebookSecret** | Secret | Facebook app secret |
| **B2C_1A_GoogleSecret** | Secret | Google client secret |

## 🚀 Getting Started Steps

1. **Download Starter Pack** → Get policy templates from GitHub
2. **Configure Policy Keys** → Create signing/encryption keys
3. **Update Tenant Settings** → Replace tenant name in XML files
4. **Upload Base Policies** → Upload in correct order
5. **Test with Run Now** → Validate policy execution

## 🎯 Exam Tips

> **Remember:**
> - IEF is the **engine** behind custom policies
> - Custom policies use **XML configuration** files
> - Policies are uploaded in a **specific order** (base → extension → relying party)
> - **Policy keys** must be created before uploading policies
> - IEF is required for **REST API integrations** in the auth flow

## 💡 Key Takeaways

1. 📜 Identity Experience Framework is the powerful engine behind custom policies
2. 🔄 Use custom policies when user flows don't meet your requirements
3. 📦 Policies follow an inheritance model (Base → Extensions → Relying Party)
4. 🔑 Policy keys for signing and encryption must be set up first
5. 🔗 Custom policies enable REST API calls, complex branching, and account migration
6. ⚠️ Custom policies have a steeper learning curve but offer maximum flexibility
