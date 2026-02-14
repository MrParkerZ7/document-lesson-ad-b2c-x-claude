# 🔄 Understanding User Flows

## 📋 What Are User Flows?

**User Flows** are pre-built, configurable policies in Azure AD B2C that define the authentication experiences for your applications. They provide a quick way to set up common identity scenarios without writing code.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔄 User Flow Concept                             │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   👤 User Action          🔄 User Flow          📱 App Response     │
│                                                                      │
│   ┌─────────────┐       ┌─────────────┐       ┌─────────────┐      │
│   │ Click       │       │ B2C handles │       │ Receives    │      │
│   │ "Sign In"   │──────►│ auth flow   │──────►│ JWT Token   │      │
│   │ button      │       │ (UI + Logic)│       │             │      │
│   └─────────────┘       └─────────────┘       └─────────────┘      │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🎯 Purpose of User Flows

| Purpose | Description |
|---------|-------------|
| **Simplify Setup** | Portal-based configuration, no code needed |
| **Standard Scenarios** | Pre-built for common use cases |
| **Quick Deployment** | Minutes to configure vs hours of development |
| **Secure by Default** | Microsoft-managed security best practices |
| **Customizable** | Adjust UI, claims, and behavior |

## 🔄 How User Flows Work

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        User Flow Execution                               │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  1️⃣ App redirects to B2C         2️⃣ B2C shows login page               │
│     ┌─────────┐                      ┌─────────────────────┐            │
│     │  Your   │ ──── REDIRECT ──────►│  🔄 User Flow Page  │            │
│     │   App   │                      │  (Sign-in/Sign-up)  │            │
│     └─────────┘                      └──────────┬──────────┘            │
│                                                  │                       │
│  4️⃣ App receives token            3️⃣ User authenticates               │
│     ┌─────────┐                      ┌─────────────────────┐            │
│     │  Your   │ ◄── TOKEN + CODE ─── │  🔑 Verify Identity │            │
│     │   App   │                      │  (IdP / Local)      │            │
│     └─────────┘                      └─────────────────────┘            │
│                                                                          │
└─────────────────────────────────────────────────────────────────────────┘
```

### Step-by-Step Flow:

| Step | Action | Details |
|------|--------|---------|
| 1 | **Redirect** | App sends user to B2C with policy name |
| 2 | **Display UI** | B2C shows appropriate authentication page |
| 3 | **Authenticate** | User enters credentials or selects IdP |
| 4 | **Verify** | B2C validates identity (local or federated) |
| 5 | **Collect Claims** | Gather user attributes (if configured) |
| 6 | **Issue Token** | B2C creates JWT tokens |
| 7 | **Redirect Back** | User returns to app with authorization code |
| 8 | **Token Exchange** | App exchanges code for tokens |

## 📊 User Flow vs Custom Policy

| Aspect | User Flow | Custom Policy |
|--------|-----------|---------------|
| **Configuration** | Azure Portal UI | XML files |
| **Complexity** | Low | High |
| **Setup Time** | Minutes | Hours/Days |
| **Flexibility** | Limited | Full control |
| **Learning Curve** | Easy | Steep |
| **Use Cases** | Standard scenarios | Complex requirements |
| **API Calls** | Limited (API Connectors) | Full integration |
| **Recommended For** | Most applications | Advanced scenarios |

```
┌────────────────────────────────────────────────────────────────┐
│              When to Use Each                                   │
├────────────────────┬───────────────────────────────────────────┤
│   🔄 User Flows    │           📜 Custom Policies              │
├────────────────────┼───────────────────────────────────────────┤
│ ✅ Simple sign-up  │ ✅ Multi-step registration                │
│ ✅ Social logins   │ ✅ Complex validation logic               │
│ ✅ Password reset  │ ✅ External API integration               │
│ ✅ Profile editing │ ✅ Custom MFA providers                   │
│ ✅ Standard MFA    │ ✅ Multiple IdP orchestration             │
│                    │ ✅ Claims transformation                  │
│                    │ ✅ Conditional flows                      │
└────────────────────┴───────────────────────────────────────────┘
```

## 🔗 User Flow Components

```
┌─────────────────────────────────────────────────────────────────────┐
│                    User Flow Building Blocks                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐ │
│  │ 🔑 Identity     │    │ 📋 User         │    │ 🎨 Page         │ │
│  │    Providers    │    │    Attributes   │    │    Layouts      │ │
│  │                 │    │                 │    │                 │ │
│  │ • Local         │    │ • Display Name  │    │ • Sign-in page  │ │
│  │ • Google        │    │ • Email         │    │ • Sign-up page  │ │
│  │ • Facebook      │    │ • Phone         │    │ • MFA page      │ │
│  │ • Enterprise    │    │ • Custom attrs  │    │ • Error page    │ │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘ │
│                                                                      │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐ │
│  │ 🎫 Token        │    │ 🛡️ Security     │    │ 🔗 API          │ │
│  │    Claims       │    │    Settings     │    │    Connectors   │ │
│  │                 │    │                 │    │                 │ │
│  │ • Standard      │    │ • MFA           │    │ • Pre-auth      │ │
│  │ • Custom        │    │ • Lockout       │    │ • Post-auth     │ │
│  │ • From IdP      │    │ • Password reqs │    │ • Custom logic  │ │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘ │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📝 Naming Convention

User Flows follow a standard naming pattern:

```
B2C_1_<PolicyName>

Examples:
├── B2C_1_SignUpSignIn
├── B2C_1_PasswordReset
├── B2C_1_ProfileEdit
└── B2C_1_SignIn_Social
```

| Prefix | Meaning |
|--------|---------|
| `B2C_1_` | Standard prefix for user flows |
| `B2C_1A_` | Prefix for custom policies |

## ⚙️ User Flow Versions

| Version | Description | Status |
|---------|-------------|--------|
| **Recommended** | Latest features, actively updated | ✅ Use this |
| **Standard (Legacy)** | Older version, limited features | ⚠️ Migrate away |
| **Preview** | New features being tested | 🧪 Test only |

## 🎯 Exam Tips

> **Key Points:**
> - User Flows = **Portal-configured**, Custom Policies = **XML-based**
> - User Flows start with `B2C_1_`, Custom Policies with `B2C_1A_`
> - Choose User Flows for **standard scenarios**, Custom Policies for **complex needs**
> - **Recommended** version should be used for new implementations
> - User Flows can be customized with **API Connectors** for external calls

## 💡 Key Takeaways

1. 🔄 **User Flows** are pre-built authentication policies configured via portal
2. ⚡ They provide **quick setup** for standard identity scenarios
3. 📋 Configure **identity providers**, **attributes**, and **UI** in one place
4. 🔗 Use **API Connectors** to extend functionality with external APIs
5. 📝 Follow naming convention: `B2C_1_<PolicyName>`
6. ✅ Use **Recommended** version for new implementations
7. 📜 Graduate to **Custom Policies** only when User Flows aren't sufficient
