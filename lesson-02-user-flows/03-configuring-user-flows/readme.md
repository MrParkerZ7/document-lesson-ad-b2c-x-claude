# ⚙️ Configuring User Flows

## 📋 Configuration Overview

User Flows are configured through the Azure Portal with a step-by-step wizard. Each configuration aspect can be customized to match your application requirements.

```
┌─────────────────────────────────────────────────────────────────────┐
│                 ⚙️ User Flow Configuration Steps                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  1️⃣ Create    2️⃣ Identity   3️⃣ User       4️⃣ Token    5️⃣ Page   │
│     Flow         Providers     Attributes     Claims      Layout   │
│                                                                      │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  │
│  │ Select  │  │ Enable  │  │Configure│  │ Choose  │  │Customize│  │
│  │ Type &  │─►│ Social/ │─►│ Collect │─►│ Return  │─►│ UI/     │  │
│  │ Name    │  │ Local   │  │ & Edit  │  │ Claims  │  │ Branding│  │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘  └─────────┘  │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 1️⃣ Creating a User Flow

### Step 1: Navigate to User Flows

```
Azure Portal → Azure AD B2C → User flows → + New user flow
```

### Step 2: Select Flow Type

| Type | Version | Description |
|------|---------|-------------|
| **Sign up and sign in** | Recommended | Combined registration & login |
| **Sign in** | Recommended | Login only |
| **Sign up** | Recommended | Registration only |
| **Password reset** | Recommended | Self-service recovery |
| **Profile editing** | Recommended | Update user info |

⚠️ **Always use "Recommended" version** for new implementations.

### Step 3: Name Your Flow

```
Policy Name: B2C_1_<YourPolicyName>

Examples:
├── B2C_1_SignUpSignIn
├── B2C_1_SignUpSignIn_v2
├── B2C_1_PasswordReset
└── B2C_1_ProfileEdit
```

| Naming Best Practice | Example |
|---------------------|---------|
| Descriptive | `B2C_1_SignUpSignIn_Production` |
| Versioned | `B2C_1_SignUpSignIn_v2` |
| Environment-specific | `B2C_1_SignUpSignIn_Dev` |

## 2️⃣ Configuring Identity Providers

```
┌─────────────────────────────────────────────────────────────────────┐
│                   🔑 Identity Provider Selection                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  ☑️ Local Account (Email signup)                                    │
│                                                                      │
│  Social Providers:                                                  │
│  ☑️ Google                  ☐ Amazon                               │
│  ☑️ Facebook                ☐ LinkedIn                             │
│  ☑️ Apple                   ☐ Twitter                              │
│  ☑️ Microsoft Account       ☐ GitHub                               │
│                                                                      │
│  Enterprise:                                                        │
│  ☐ Azure AD (work accounts)                                        │
│  ☐ SAML Identity Provider                                          │
│  ☐ OpenID Connect Provider                                         │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Prerequisites for Social Providers

| Provider | Requires |
|----------|----------|
| **Google** | Google Cloud Console app |
| **Facebook** | Meta for Developers app |
| **Apple** | Apple Developer account |
| **Microsoft** | Azure AD app registration |

## 3️⃣ User Attributes Configuration

### Attribute Types

| Type | Description | Examples |
|------|-------------|----------|
| **Built-in** | Standard attributes | Email, Display Name, Given Name |
| **Custom** | User-defined | Loyalty Number, Preferences |
| **Extension** | Graph API attributes | `extension_<appId>_<attrName>` |

### Attribute Settings

```
┌────────────────────────────────────────────────────────────────┐
│                  📋 Attribute Configuration                     │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Attribute           Collect    Return    Type                 │
│  ─────────────────   ────────   ──────    ─────                │
│  ☑️ Email Address      ✓          ✓       String              │
│  ☑️ Display Name       ✓          ✓       String              │
│  ☑️ Given Name         ✓          ✓       String              │
│  ☑️ Surname            ✓          ✓       String              │
│  ☐ City                ✓          ✓       String              │
│  ☐ Country/Region      ✓          ✓       String              │
│  ☐ Postal Code         ✓          ✓       String              │
│  ☐ Job Title           ✓          ✓       String              │
│                                                                 │
│  Custom Attributes:                                            │
│  ☑️ loyaltyNumber      ✓          ✓       String              │
│  ☐ membershipTier      ✓          ✓       String              │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

| Column | Purpose |
|--------|---------|
| **Collect** | Asked during sign-up |
| **Return** | Included in token claims |

## 4️⃣ Token Claims Configuration

### Claim Types

| Claim | Description | Example Value |
|-------|-------------|---------------|
| `sub` | Subject identifier | User's object ID |
| `email` | User's email | user@example.com |
| `name` | Display name | John Doe |
| `given_name` | First name | John |
| `family_name` | Last name | Doe |
| `idp` | Identity provider | google.com |

### Token Lifetime Settings

```
┌────────────────────────────────────────────────────────────────┐
│                   🎫 Token Configuration                        │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Token Lifetimes:                                              │
│  ┌────────────────────────────────────────────────┐            │
│  │ Access Token Lifetime:     [60 minutes    ▼]  │            │
│  │ Refresh Token Lifetime:    [14 days       ▼]  │            │
│  │ ID Token Lifetime:         [60 minutes    ▼]  │            │
│  └────────────────────────────────────────────────┘            │
│                                                                 │
│  Compatibility Settings:                                       │
│  ☑️ Issue ID token                                             │
│  ☑️ Issue refresh token                                        │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

## 5️⃣ Page Layout Configuration

### Built-in Templates

| Template | Description |
|----------|-------------|
| **Ocean Blue** | Blue theme with modern layout |
| **Slate Gray** | Professional gray theme |
| **Classic** | Traditional white background |

### Custom Branding

```
┌────────────────────────────────────────────────────────────────┐
│                   🎨 Custom Branding Options                    │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  Company Branding:                                             │
│  ├── Banner Logo              [Upload PNG/JPG]                 │
│  ├── Background Image         [Upload PNG/JPG]                 │
│  ├── Background Color         [#FFFFFF_______]                 │
│  └── Sign-in Page Text        [_____________]                  │
│                                                                 │
│  Custom Page Content (HTML):                                   │
│  ├── Enable custom content    ☑️                               │
│  └── Content URL              [https://cdn.example.com/login]  │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

### Self-Hosted Custom Pages

| Requirement | Description |
|-------------|-------------|
| **CORS** | Enable for B2C domain |
| **HTTPS** | Required for all content |
| **CDN** | Recommended for performance |
| **Elements** | Include B2C required elements |

## 6️⃣ MFA Configuration

```
┌────────────────────────────────────────────────────────────────┐
│                   🛡️ MFA Settings                              │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  MFA Enforcement:                                              │
│  ○ Off           - No MFA required                             │
│  ○ Always On     - MFA for every sign-in                       │
│  ● Conditional   - Based on risk or conditions                 │
│                                                                 │
│  Available Methods:                                            │
│  ☑️ Email OTP                                                   │
│  ☑️ SMS/Phone                                                   │
│  ☑️ Authenticator App (TOTP)                                   │
│                                                                 │
│  ⚠️ Conditional Access requires Premium P2                     │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

## 7️⃣ API Connectors (Extensibility)

API Connectors allow you to call external APIs during user flow execution.

```
┌────────────────────────────────────────────────────────────────┐
│                   🔗 API Connector Flow                         │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Sign-up Flow                                                 │
│        │                                                        │
│        ▼                                                        │
│   ┌─────────────────────────┐                                  │
│   │ Before creating user    │◄─── API Call 1 (Validation)     │
│   └───────────┬─────────────┘                                  │
│               │                                                 │
│               ▼                                                 │
│   ┌─────────────────────────┐                                  │
│   │ Before including claims │◄─── API Call 2 (Enrichment)     │
│   └───────────┬─────────────┘                                  │
│               │                                                 │
│               ▼                                                 │
│        Token Issued                                            │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

| Connector Point | Use Case |
|-----------------|----------|
| **Before creating user** | Validate, block, enrich |
| **Before including claims** | Add custom claims from API |

### API Connector Setup

| Setting | Description |
|---------|-------------|
| **Endpoint URL** | Your API endpoint (HTTPS) |
| **Authentication** | Basic or Certificate |
| **Username** | For basic auth |
| **Password** | For basic auth |

## ⚙️ Configuration Checklist

```
┌────────────────────────────────────────────────────────────────┐
│               ✅ User Flow Configuration Checklist              │
├────────────────────────────────────────────────────────────────┤
│                                                                 │
│  □ Flow type selected (use Recommended version)                │
│  □ Policy name defined (follows naming convention)             │
│  □ Identity providers configured                               │
│  □ User attributes selected (collect + return)                 │
│  □ Token claims configured                                     │
│  □ Token lifetimes set appropriately                          │
│  □ Page layouts customized (branding applied)                  │
│  □ MFA settings configured (if required)                       │
│  □ API connectors added (if needed)                           │
│  □ Flow tested in "Run user flow" mode                        │
│                                                                 │
└────────────────────────────────────────────────────────────────┘
```

## 🎯 Exam Tips

> **Configuration Points:**
> - Always use **Recommended** version for new flows
> - **Collect** = sign-up form, **Return** = token claims
> - MFA can be **Off**, **Always On**, or **Conditional** (P2 feature)
> - **API Connectors** extend flows with custom logic
> - Test flows using **"Run user flow"** button in portal
> - **Custom page content** requires HTTPS and CORS configuration

## 💡 Key Takeaways

1. ⚙️ User Flows are configured via **Azure Portal UI** step-by-step
2. 🔑 Enable **Identity Providers** before using them in flows
3. 📋 **Attributes**: Collect during sign-up, Return in tokens
4. 🎫 Configure **token lifetimes** based on security requirements
5. 🎨 **Customize branding** for consistent user experience
6. 🛡️ **MFA** adds security layer with multiple verification methods
7. 🔗 **API Connectors** enable custom validation and enrichment
8. 🧪 Always **test flows** before production deployment
