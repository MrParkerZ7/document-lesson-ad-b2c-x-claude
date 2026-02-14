# 🔑 Key Components of Azure AD B2C

## 🏗️ Architecture Overview

Azure AD B2C consists of several interconnected components that work together to provide a complete identity management solution.

```
┌─────────────────────────────────────────────────────────────────────────┐
│                      🏢 Azure AD B2C Architecture                        │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  ┌─────────────────────────────────────────────────────────────────┐    │
│  │                        B2C Tenant                                │    │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │    │
│  │  │ 👤 User     │  │ 🖥️ App      │  │ 🔑 Identity Providers   │  │    │
│  │  │ Directory   │  │ Registrations│  │ (Social/Enterprise)    │  │    │
│  │  └─────────────┘  └─────────────┘  └─────────────────────────┘  │    │
│  │                                                                  │    │
│  │  ┌─────────────────────────────────────────────────────────────┐│    │
│  │  │                   🔄 User Flows / Custom Policies            ││    │
│  │  └─────────────────────────────────────────────────────────────┘│    │
│  │                                                                  │    │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────────────────┐  │    │
│  │  │ 🛡️ Security │  │ 🎨 UI       │  │ 🎫 Token Configuration  │  │    │
│  │  │ Features    │  │ Customization│  │ (Claims, Lifetime)     │  │    │
│  │  └─────────────┘  └─────────────┘  └─────────────────────────┘  │    │
│  └─────────────────────────────────────────────────────────────────┘    │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

## 📋 Component Details

### 1️⃣ B2C Tenant (Directory)

| Aspect | Description |
|--------|-------------|
| **What** | A dedicated Azure AD B2C directory |
| **Purpose** | Container for all B2C configurations and user data |
| **Naming** | `yourtenant.onmicrosoft.com` |
| **Isolation** | Separate from organizational Azure AD |
| **Region** | Selected during creation, affects data residency |

```
┌─────────────────────────────────────┐
│       🏢 B2C Tenant Structure       │
├─────────────────────────────────────┤
│  Tenant Name: contoso.onmicrosoft.com
│  ├── 👤 Users (millions)            │
│  ├── 🖥️ Applications                │
│  ├── 🔑 Identity Providers          │
│  ├── 🔄 User Flows                  │
│  ├── 📜 Custom Policies             │
│  └── ⚙️ Settings                    │
└─────────────────────────────────────┘
```

### 2️⃣ User Directory

| Feature | Description |
|---------|-------------|
| **Storage** | User profiles and credentials |
| **Account Types** | Local, Social, Federated |
| **Attributes** | Built-in + Custom attributes |
| **Scale** | Millions of user accounts |
| **Management** | Azure Portal, Graph API, PowerShell |

**User Account Types:**

| Type | Description | Example |
|------|-------------|---------|
| **Local Account** | Email/username + password | user@email.com |
| **Social Account** | Linked to social IdP | Google, Facebook user |
| **Federated Account** | Enterprise IdP | Corporate SAML user |

### 3️⃣ Application Registrations

```
┌───────────────────────────────────────────────────────┐
│               🖥️ App Registration                     │
├───────────────────────────────────────────────────────┤
│  Application (client) ID:  xxxxxxxx-xxxx-xxxx-xxxx   │
│  Directory (tenant) ID:    xxxxxxxx-xxxx-xxxx-xxxx   │
│                                                       │
│  ┌─────────────────────────────────────────────────┐ │
│  │ Redirect URIs:                                   │ │
│  │  • https://myapp.com/callback                   │ │
│  │  • https://localhost:3000/auth                  │ │
│  └─────────────────────────────────────────────────┘ │
│                                                       │
│  ┌─────────────────────────────────────────────────┐ │
│  │ Supported Account Types:                         │ │
│  │  ☑️ Accounts in this organizational directory   │ │
│  └─────────────────────────────────────────────────┘ │
└───────────────────────────────────────────────────────┘
```

| Property | Description |
|----------|-------------|
| **Application ID** | Unique identifier (Client ID) |
| **Client Secret** | Confidential credential (web apps) |
| **Redirect URIs** | Where tokens are sent after auth |
| **Supported Flows** | Authorization Code, Implicit, etc. |

### 4️⃣ Identity Providers

| Category | Providers | Configuration |
|----------|-----------|---------------|
| **Social** | Google, Facebook, Apple, Microsoft, Twitter | OAuth 2.0 / OIDC |
| **Enterprise** | Azure AD, ADFS, Okta, Ping | SAML 2.0 / OIDC |
| **Local** | Email + Password, Phone + OTP | Built-in |
| **Custom** | Any OIDC/SAML provider | Manual config |

### 5️⃣ User Flows & Custom Policies

```
┌──────────────────────────────────────────────────────────────┐
│                   Policy Types Comparison                     │
├────────────────────────────┬─────────────────────────────────┤
│    🔄 User Flows           │      📜 Custom Policies         │
│    (Recommended)           │      (Advanced)                 │
├────────────────────────────┼─────────────────────────────────┤
│  • Portal-based config     │  • XML-based configuration     │
│  • Pre-built templates     │  • Full flexibility            │
│  • Quick setup             │  • Complex scenarios           │
│  • Limited customization   │  • Custom logic & API calls    │
│  • Best for most apps      │  • Requires IEF knowledge      │
└────────────────────────────┴─────────────────────────────────┘
```

**Available User Flow Types:**

| User Flow | Purpose |
|-----------|---------|
| **Sign up and sign in** | Combined registration & login |
| **Sign in** | Login only |
| **Sign up** | Registration only |
| **Profile editing** | Update user attributes |
| **Password reset** | Self-service password reset |

### 6️⃣ Token Configuration

| Token Type | Purpose | Content |
|------------|---------|---------|
| **ID Token** | User identity | User claims (name, email, etc.) |
| **Access Token** | API authorization | Scopes, permissions |
| **Refresh Token** | Token renewal | Long-lived, secure |

```
┌─────────────────────────────────────────────────────────────┐
│                    🎫 Token Lifecycle                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│    User Login ──► Authentication ──► Token Issued           │
│                                           │                  │
│                                           ▼                  │
│                               ┌─────────────────────┐       │
│                               │  🎫 JWT Token       │       │
│                               │  • ID Token (1hr)   │       │
│                               │  • Access Token     │       │
│                               │  • Refresh Token    │       │
│                               └─────────────────────┘       │
│                                           │                  │
│                                           ▼                  │
│                                    App Validates             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 7️⃣ Security Features

| Feature | Description |
|---------|-------------|
| **MFA** | Multi-factor authentication (SMS, Email, Authenticator) |
| **Conditional Access** | Risk-based policies (P2 feature) |
| **Identity Protection** | Anomaly detection |
| **Password Policies** | Complexity requirements |
| **Account Lockout** | Brute force protection |

## 🔗 Component Relationships

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Component Interaction Flow                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   👤 User                                                           │
│      │                                                               │
│      ▼                                                               │
│   🖥️ Application ──────────────► 🔄 User Flow/Policy                │
│      │                                    │                          │
│      │                                    ▼                          │
│      │                           🔑 Identity Provider                │
│      │                                    │                          │
│      │                                    ▼                          │
│      │                           👤 User Directory                   │
│      │                                    │                          │
│      │                                    ▼                          │
│      ◄────────────────────────── 🎫 Token Issued                    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🎯 Exam Tips

> **Component Knowledge:**
> - **Tenant**: The container for everything - users, apps, policies
> - **User Flows**: Use for standard scenarios (sign-up, sign-in, password reset)
> - **Custom Policies**: Use for complex requirements (custom logic, API calls)
> - **App Registration**: Every app needs one - provides Client ID
> - **Identity Providers**: Configure once, use across multiple flows

> **Common Confusion Points:**
> - User Flows are **portal-configured**, Custom Policies are **XML-based**
> - **Local accounts** are stored in B2C, **social accounts** are linked references
> - **ID tokens** identify users, **Access tokens** authorize API calls

## 💡 Key Takeaways

1. 🏢 **B2C Tenant** is a dedicated directory separate from Azure AD
2. 👤 **User Directory** stores local accounts and links to external identities
3. 🖥️ **App Registrations** connect your applications to B2C
4. 🔑 **Identity Providers** enable social and enterprise logins
5. 🔄 **User Flows** are pre-built policies for common scenarios
6. 📜 **Custom Policies** (IEF) provide full control for complex needs
7. 🎫 **Tokens** (JWT) are issued after successful authentication
8. 🛡️ **Security Features** protect user accounts and detect threats
