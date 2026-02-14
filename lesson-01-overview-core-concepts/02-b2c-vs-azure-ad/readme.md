# 🔄 Azure AD B2C vs Azure AD

## 🎯 Understanding the Difference

Azure AD B2C and Azure AD are **separate services** designed for different identity scenarios. Understanding when to use each is critical.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                    Identity Solution Comparison                          │
├─────────────────────────────┬───────────────────────────────────────────┤
│      🏢 Azure AD            │           👤 Azure AD B2C                 │
│      (Azure Entra ID)       │                                           │
├─────────────────────────────┼───────────────────────────────────────────┤
│  • Employees                │  • Customers                              │
│  • Partners                 │  • Consumers                              │
│  • Internal Apps            │  • External Apps                          │
│  • Microsoft 365            │  • Custom Applications                    │
│  • Enterprise SSO           │  • Social Logins                          │
└─────────────────────────────┴───────────────────────────────────────────┘
```

## 📊 Detailed Comparison

| Aspect | Azure AD (Entra ID) | Azure AD B2C |
|--------|---------------------|--------------|
| **Primary Users** | Employees, B2B guests | Consumers, customers |
| **Tenant Type** | Organizational directory | Dedicated B2C directory |
| **Account Types** | Work/School accounts | Local, Social, Enterprise |
| **Scale Focus** | Thousands of employees | Millions of consumers |
| **Sign-up** | Admin-provisioned | Self-service registration |
| **Social Login** | Limited | Built-in (Google, FB, Apple) |
| **UI Customization** | Company branding only | Full page customization |
| **Pricing Model** | Per-user licensing | Monthly Active Users (MAU) |
| **Microsoft 365** | ✅ Integrated | ❌ Not applicable |
| **Conditional Access** | Full feature set | Limited but improving |

## 🔑 When to Use Each

### Use Azure AD (Entra ID) When:

```
┌────────────────────────────────────────────┐
│  ✅ Azure AD is the Right Choice           │
├────────────────────────────────────────────┤
│  • Managing employee identities            │
│  • Microsoft 365 / Office 365 access       │
│  • Enterprise SSO for SaaS apps            │
│  • B2B collaboration with partners         │
│  • Device management (Intune)              │
│  • Internal line-of-business apps          │
└────────────────────────────────────────────┘
```

### Use Azure AD B2C When:

```
┌────────────────────────────────────────────┐
│  ✅ Azure AD B2C is the Right Choice       │
├────────────────────────────────────────────┤
│  • Consumer-facing applications            │
│  • Self-service sign-up required           │
│  • Social identity provider integration    │
│  • Custom branded login experiences        │
│  • Millions of external users              │
│  • E-commerce, gaming, media platforms     │
└────────────────────────────────────────────┘
```

## 🏗️ Architecture Comparison

```
          Azure AD (Employees)                    Azure AD B2C (Consumers)
         ─────────────────────                   ─────────────────────────

    ┌─────────────────────┐               ┌─────────────────────────────┐
    │    Organization     │               │     Your Application        │
    │    Azure AD Tenant  │               │                             │
    └──────────┬──────────┘               └──────────────┬──────────────┘
               │                                         │
    ┌──────────▼──────────┐               ┌──────────────▼──────────────┐
    │  👤 Employee        │               │    🏢 Azure AD B2C Tenant   │
    │  Work Account       │               │    (Separate Directory)     │
    │  user@company.com   │               └──────────────┬──────────────┘
    └─────────────────────┘                              │
                                          ┌──────────────┼──────────────┐
                                          ▼              ▼              ▼
                                    ┌─────────┐   ┌───────────┐   ┌─────────┐
                                    │📧 Local │   │🔗 Social  │   │🏢 SAML  │
                                    │ Account │   │  Google   │   │  IdP    │
                                    └─────────┘   └───────────┘   └─────────┘
```

## 💰 Pricing Model Differences

| Pricing Aspect | Azure AD | Azure AD B2C |
|----------------|----------|--------------|
| **Model** | Per-user per-month | Monthly Active Users (MAU) |
| **Free Tier** | Azure AD Free (basic) | First 50,000 MAU free |
| **Premium** | P1: ~$6/user/month | Beyond 50k MAU: ~$0.00325/MAU |
| **Billing Unit** | All provisioned users | Only active users |
| **Cost for Inactive** | Still billed | Not billed |

## ⚠️ Common Misconceptions

| Misconception | Reality |
|---------------|---------|
| "B2C is part of Azure AD" | ❌ It's a **separate service** with its own tenant |
| "I can use my Azure AD tenant for B2C" | ❌ You need to **create a new B2C tenant** |
| "B2C has all Azure AD features" | ❌ Some features differ (e.g., Groups, Licensing) |
| "B2C is only for social logins" | ❌ Also supports local accounts & enterprise IdPs |

## 🔄 Can They Work Together?

**Yes!** Common hybrid scenarios:

```
┌─────────────────────────────────────────────────────────────────┐
│                     Hybrid Scenario                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│    🏢 Corporate Azure AD           👤 Consumer Azure AD B2C     │
│    ┌───────────────────┐          ┌───────────────────────┐    │
│    │  Employee Portal  │          │  Customer Portal      │    │
│    │  (Internal Apps)  │          │  (Public Website)     │    │
│    └─────────┬─────────┘          └──────────┬────────────┘    │
│              │                               │                  │
│              │       ┌───────────────┐       │                  │
│              └──────►│  Backend API  │◄──────┘                  │
│                      │  (Validates   │                          │
│                      │   both tokens)│                          │
│                      └───────────────┘                          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

## 🎯 Exam Tips

> **Key Differentiators to Remember:**
> - Azure AD = **Employees & Partners** (B2B)
> - Azure AD B2C = **Consumers & Customers** (B2C)
> - B2C requires a **separate, dedicated tenant**
> - B2C pricing is **MAU-based**, Azure AD is **per-user licensed**
> - B2C supports **self-service sign-up** with social providers
> - Both can coexist and work together in hybrid scenarios

## 💡 Key Takeaways

1. 🏢 Azure AD is for **internal/employee** identities with work accounts
2. 👤 Azure AD B2C is for **external/consumer** identities with flexible account types
3. 🔄 They are **separate services** requiring separate tenants
4. 💰 Azure AD bills **per provisioned user**, B2C bills **per active user (MAU)**
5. 🎨 B2C offers **extensive UI customization**, Azure AD has limited branding
6. 🔗 Both can work together in **hybrid enterprise-consumer architectures**
