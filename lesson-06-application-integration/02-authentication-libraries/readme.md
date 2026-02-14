# 📚 Authentication Libraries (MSAL)

## 🔑 Overview

Microsoft Authentication Library (MSAL) is the recommended library for integrating applications with Azure AD B2C. It handles the complexity of OAuth 2.0 and OpenID Connect protocols.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📚 MSAL Library Family                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │                     MSAL (Recommended)                       │   │
│   │                                                              │   │
│   │  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐           │   │
│   │  │ MSAL.js │ │ MSAL    │ │ MSAL    │ │ MSAL    │           │   │
│   │  │ (React, │ │ .NET    │ │ Python  │ │ Java    │           │   │
│   │  │ Angular)│ │         │ │         │ │         │           │   │
│   │  └─────────┘ └─────────┘ └─────────┘ └─────────┘           │   │
│   │                                                              │   │
│   │  ┌─────────┐ ┌─────────┐ ┌─────────┐                        │   │
│   │  │ MSAL    │ │ MSAL    │ │ MSAL    │                        │   │
│   │  │ iOS     │ │ Android │ │ Go      │                        │   │
│   │  └─────────┘ └─────────┘ └─────────┘                        │   │
│   │                                                              │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   ⚠️ ADAL (Active Directory Authentication Library) is DEPRECATED  │
│      Migrate to MSAL for all new development                        │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 MSAL by Platform

| Platform | Library | Package |
|----------|---------|---------|
| **JavaScript/SPA** | MSAL.js | `@azure/msal-browser` |
| **React** | MSAL React | `@azure/msal-react` |
| **Angular** | MSAL Angular | `@azure/msal-angular` |
| **.NET** | MSAL.NET | `Microsoft.Identity.Client` |
| **ASP.NET Core** | Microsoft.Identity.Web | `Microsoft.Identity.Web` |
| **Python** | MSAL Python | `msal` |
| **Java** | MSAL Java | `msal4j` |
| **iOS** | MSAL iOS | `MSAL` (CocoaPods) |
| **Android** | MSAL Android | `com.microsoft.identity.client` |

## 🔄 Authentication Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔄 MSAL Authentication Flow                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   👤 User          🖥️ Your App (MSAL)         ☁️ Azure AD B2C       │
│      │                    │                          │               │
│      │  1. Click Login    │                          │               │
│      │───────────────────►│                          │               │
│      │                    │                          │               │
│      │                    │  2. Redirect to B2C      │               │
│      │◄───────────────────│─────────────────────────►│               │
│      │                    │                          │               │
│      │  3. Enter Credentials                         │               │
│      │──────────────────────────────────────────────►│               │
│      │                    │                          │               │
│      │                    │  4. Auth Code + Redirect │               │
│      │◄─────────────────────────────────────────────│               │
│      │                    │                          │               │
│      │                    │  5. Exchange Code        │               │
│      │                    │─────────────────────────►│               │
│      │                    │                          │               │
│      │                    │  6. Tokens (ID, Access)  │               │
│      │                    │◄─────────────────────────│               │
│      │                    │                          │               │
│      │  7. User Logged In │                          │               │
│      │◄───────────────────│                          │               │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 💻 MSAL.js Configuration (SPA)

```javascript
// Configuration
const msalConfig = {
  auth: {
    clientId: "your-client-id",
    authority: "https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_SignUpSignIn",
    knownAuthorities: ["contoso.b2clogin.com"],
    redirectUri: "https://yourapp.com/",
  },
  cache: {
    cacheLocation: "sessionStorage", // or "localStorage"
    storeAuthStateInCookie: false,
  },
};

// Initialize MSAL
const msalInstance = new msal.PublicClientApplication(msalConfig);

// Login
async function login() {
  try {
    const response = await msalInstance.loginPopup({
      scopes: ["openid", "profile", "https://contoso.com/api/read"],
    });
    console.log("Logged in:", response.account);
  } catch (error) {
    console.error("Login failed:", error);
  }
}
```

## 🖥️ MSAL.NET Configuration (Web App)

```csharp
// In Program.cs or Startup.cs
builder.Services.AddAuthentication(OpenIdConnectDefaults.AuthenticationScheme)
    .AddMicrosoftIdentityWebApp(options =>
    {
        options.Instance = "https://contoso.b2clogin.com/";
        options.Domain = "contoso.onmicrosoft.com";
        options.ClientId = "your-client-id";
        options.ClientSecret = "your-client-secret";
        options.SignUpSignInPolicyId = "B2C_1_SignUpSignIn";
        options.ResetPasswordPolicyId = "B2C_1_PasswordReset";
        options.EditProfilePolicyId = "B2C_1_ProfileEdit";
    });
```

## 🔐 Token Acquisition Methods

| Method | Use Case | Platform |
|--------|----------|----------|
| `loginPopup()` | Interactive login (popup) | SPA |
| `loginRedirect()` | Interactive login (redirect) | SPA |
| `acquireTokenSilent()` | Get token silently (cached) | All |
| `acquireTokenPopup()` | Get token interactively | SPA |
| `acquireTokenByCode()` | Exchange auth code | Server |
| `acquireTokenForClient()` | Client credentials (daemon) | Server |

## 🔄 Silent Token Acquisition

```
┌─────────────────────────────────────────────────────────────────────┐
│                 🔄 Silent Token Acquisition                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   acquireTokenSilent()                                               │
│          │                                                           │
│          ▼                                                           │
│   ┌──────────────────────────────────────────────┐                  │
│   │  Check token cache                            │                  │
│   └──────────────────────────────────────────────┘                  │
│          │                                                           │
│          ├─── Token valid ──────► ✅ Return cached token            │
│          │                                                           │
│          ├─── Token expired ────► Use refresh token                 │
│          │                        ├── Success ──► ✅ New tokens     │
│          │                        └── Fail ─────► ❌ Need login     │
│          │                                                           │
│          └─── No token ─────────► ❌ InteractionRequired            │
│                                                                      │
│   Best Practice: Always try silent first, fall back to interactive  │
│                                                                      │
│   try {                                                              │
│     token = await msalInstance.acquireTokenSilent(request);         │
│   } catch (error) {                                                  │
│     if (error instanceof InteractionRequiredAuthError) {            │
│       token = await msalInstance.acquireTokenPopup(request);        │
│     }                                                                │
│   }                                                                  │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📱 React Integration Example

```jsx
import { MsalProvider, useMsal, useIsAuthenticated } from "@azure/msal-react";
import { PublicClientApplication } from "@azure/msal-browser";

const msalInstance = new PublicClientApplication(msalConfig);

function App() {
  return (
    <MsalProvider instance={msalInstance}>
      <MainContent />
    </MsalProvider>
  );
}

function MainContent() {
  const { instance, accounts } = useMsal();
  const isAuthenticated = useIsAuthenticated();

  const handleLogin = () => {
    instance.loginPopup({
      scopes: ["openid", "profile"],
    });
  };

  return (
    <div>
      {isAuthenticated ? (
        <p>Welcome, {accounts[0]?.name}</p>
      ) : (
        <button onClick={handleLogin}>Sign In</button>
      )}
    </div>
  );
}
```

## 🔀 B2C-Specific Considerations

| Feature | Standard Azure AD | Azure AD B2C |
|---------|-------------------|--------------|
| **Authority URL** | `login.microsoftonline.com` | `{tenant}.b2clogin.com` |
| **Policy in authority** | N/A | Required in URL |
| **Known authorities** | Not needed | Must specify |
| **Multiple policies** | N/A | Different flows |

### Authority URL Format

```
https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}

Example:
https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_SignUpSignIn
```

## ⚠️ Common Pitfalls

| Issue | Cause | Solution |
|-------|-------|----------|
| **CORS errors** | Wrong redirect URI | Add exact URI to registration |
| **Invalid authority** | Missing policy | Include policy in authority |
| **Token not found** | Cache location | Check cacheLocation setting |
| **Popup blocked** | Browser settings | Use redirect instead |
| **Silent fails** | No session | Handle InteractionRequired |

## 🎯 Exam Tips

> **Remember:**
> - **MSAL** is the recommended library (ADAL is deprecated)
> - B2C authority URL includes the **policy name**
> - Always try **acquireTokenSilent** first
> - **knownAuthorities** must include your B2C domain
> - SPAs use `PublicClientApplication`, servers use `ConfidentialClientApplication`
> - React/Angular have dedicated MSAL wrapper packages

## 💡 Key Takeaways

1. 📚 Use MSAL for all new Azure AD B2C integrations
2. 🔗 Authority URL must include your B2C tenant and policy
3. 🔄 Always try silent token acquisition first
4. ⚠️ Handle InteractionRequired errors gracefully
5. 💾 Configure appropriate cache location (sessionStorage vs localStorage)
6. 📱 Use platform-specific MSAL libraries for best experience
