# 🔧 Common Issues and Troubleshooting

## 🔑 Overview

This guide covers the most common Azure AD B2C issues and their solutions. Understanding these problems helps quickly resolve authentication and configuration errors.

```
┌─────────────────────────────────────────────────────────────────────┐
│                 🔧 Troubleshooting Workflow                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ❌ Error Occurs                                                    │
│        │                                                             │
│        ▼                                                             │
│   ┌─────────────────┐                                               │
│   │ 1. Check Error  │ ← Error message, code, correlation ID         │
│   │    Message      │                                               │
│   └────────┬────────┘                                               │
│            │                                                         │
│            ▼                                                         │
│   ┌─────────────────┐                                               │
│   │ 2. Review Logs  │ ← Sign-in logs, Application Insights          │
│   └────────┬────────┘                                               │
│            │                                                         │
│            ▼                                                         │
│   ┌─────────────────┐                                               │
│   │ 3. Check Config │ ← App registration, redirect URIs, policies   │
│   └────────┬────────┘                                               │
│            │                                                         │
│            ▼                                                         │
│   ┌─────────────────┐                                               │
│   │ 4. Test Flow    │ ← "Run now" in Azure Portal                   │
│   └────────┬────────┘                                               │
│            │                                                         │
│            ▼                                                         │
│   ✅ Issue Resolved                                                  │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🚫 Authentication Errors

### AADB2C90006: Invalid Redirect URI

```
┌─────────────────────────────────────────────────────────────────────┐
│  Error: AADB2C90006                                                  │
│  "The redirect URI 'X' provided in the request is not registered"   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Cause:                                                              │
│  • Redirect URI in request doesn't match registered URIs            │
│  • Trailing slash mismatch (http://app.com vs http://app.com/)      │
│  • HTTP vs HTTPS mismatch                                           │
│  • Port number difference                                           │
│                                                                      │
│  Solution:                                                           │
│  1. Go to App Registrations → Your App → Authentication             │
│  2. Check registered redirect URIs                                  │
│  3. Add the exact URI your app is using                             │
│  4. Ensure case-sensitive match                                     │
│                                                                      │
│  Example Fix:                                                        │
│  App sends:     https://myapp.com/auth/callback                     │
│  Registered:    https://myapp.com/auth/callback  ← Must match!      │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### AADB2C90014: Missing Required Parameter

```
┌─────────────────────────────────────────────────────────────────────┐
│  Error: AADB2C90014                                                  │
│  "A required parameter is missing from the request"                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Common Missing Parameters:                                          │
│  • client_id                                                         │
│  • response_type                                                     │
│  • redirect_uri                                                      │
│  • scope (must include 'openid')                                    │
│  • nonce (for implicit flow)                                        │
│                                                                      │
│  Solution:                                                           │
│  Check your MSAL configuration includes all required parameters:    │
│                                                                      │
│  const msalConfig = {                                               │
│    auth: {                                                          │
│      clientId: "required",        ← Must be set                     │
│      authority: "required",       ← Must be set                     │
│      redirectUri: "required",     ← Must be set                     │
│    },                                                                │
│  };                                                                  │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### AADB2C90117: Invalid Authority

```
┌─────────────────────────────────────────────────────────────────────┐
│  Error: AADB2C90117                                                  │
│  "The specified authority URL is invalid"                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Cause:                                                              │
│  • Malformed authority URL                                          │
│  • Missing policy name                                              │
│  • Wrong tenant name                                                │
│                                                                      │
│  Correct Authority Format:                                          │
│  https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}    │
│                                                                      │
│  Example:                                                            │
│  ❌ https://contoso.b2clogin.com/                                   │
│  ❌ https://contoso.b2clogin.com/contoso.onmicrosoft.com            │
│  ✅ https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_susi │
│                                                                      │
│  Also set knownAuthorities:                                         │
│  knownAuthorities: ["contoso.b2clogin.com"]                        │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔐 Token and Claims Issues

### Missing Claims in Token

```
┌─────────────────────────────────────────────────────────────────────┐
│  Issue: Expected claims not appearing in token                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Checklist:                                                          │
│                                                                      │
│  1. Check User Flow Configuration                                   │
│     Azure AD B2C → User Flows → [Your Flow] → Application claims    │
│     ☑️ Ensure claim is checked                                       │
│                                                                      │
│  2. Check User Attributes                                           │
│     If claim is empty, user may not have the attribute set          │
│     Azure AD B2C → Users → [User] → Profile                         │
│                                                                      │
│  3. Check Scope Requested                                           │
│     Ensure you're requesting the right scopes in your app           │
│     scopes: ["openid", "profile", "your-api-scope"]                 │
│                                                                      │
│  4. For Custom Policies                                              │
│     Check RelyingParty OutputClaims includes the claim              │
│     <OutputClaim ClaimTypeReferenceId="displayName" />              │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Token Validation Failures

```
┌─────────────────────────────────────────────────────────────────────┐
│  Issue: API rejecting tokens with "Invalid token"                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Common Causes:                                                      │
│                                                                      │
│  1. Wrong Audience                                                  │
│     Token aud: "client-id-A"                                        │
│     API expects: "client-id-B"                                      │
│     Fix: Check API's expected audience matches token                │
│                                                                      │
│  2. Wrong Issuer                                                    │
│     Different policy = different issuer                             │
│     Fix: Validate against correct policy's issuer                   │
│                                                                      │
│  3. Expired Token                                                   │
│     Check exp claim vs current time                                 │
│     Fix: Refresh token or re-authenticate                           │
│                                                                      │
│  4. Clock Skew                                                       │
│     Server time different from B2C time                             │
│     Fix: Add 5 minute tolerance for exp validation                  │
│                                                                      │
│  Debug: Decode token at jwt.ms or jwt.io                            │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🎨 UI and Customization Issues

### Custom HTML Not Loading

```
┌─────────────────────────────────────────────────────────────────────┐
│  Issue: Custom page content not appearing                           │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Checklist:                                                          │
│                                                                      │
│  1. CORS Configuration                                              │
│     Storage account must allow your B2C tenant:                     │
│     Allowed origins: https://{tenant}.b2clogin.com                  │
│                                                                      │
│  2. HTTPS Required                                                   │
│     All resources must use HTTPS                                    │
│     ❌ http://storage.com/page.html                                 │
│     ✅ https://storage.com/page.html                                │
│                                                                      │
│  3. Public Access                                                    │
│     Files must be publicly accessible                               │
│     Check blob container access level                               │
│                                                                      │
│  4. Required Element                                                │
│     HTML must contain: <div id="api"></div>                         │
│     B2C injects content here                                        │
│                                                                      │
│  5. Content-Type Header                                              │
│     HTML files: text/html                                           │
│     CSS files: text/css                                             │
│     JS files: application/javascript                                │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📜 Custom Policy Issues

### Policy Upload Failure

```
┌─────────────────────────────────────────────────────────────────────┐
│  Issue: Custom policy upload fails with XML error                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Common Causes:                                                      │
│                                                                      │
│  1. Wrong Upload Order                                              │
│     Must upload in order: Base → Extensions → Relying Party         │
│                                                                      │
│  2. Missing Base Policy Reference                                   │
│     <BasePolicy>                                                    │
│       <TenantId>yourtenant.onmicrosoft.com</TenantId>              │
│       <PolicyId>B2C_1A_TrustFrameworkBase</PolicyId>               │
│     </BasePolicy>                                                   │
│                                                                      │
│  3. Duplicate Element IDs                                           │
│     Each ClaimType, TechnicalProfile must have unique Id            │
│                                                                      │
│  4. Missing Referenced Elements                                     │
│     ClaimType referenced but not defined                            │
│     TechnicalProfile referenced but not found                       │
│                                                                      │
│  5. Tenant Name Mismatch                                            │
│     Replace ALL yourtenant.onmicrosoft.com with your actual tenant │
│                                                                      │
│  Debug: Check the detailed error message for line number            │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔍 Using Application Insights

```
┌─────────────────────────────────────────────────────────────────────┐
│                 📊 Application Insights Setup                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   For Custom Policies - Add to TrustFrameworkExtensions.xml:        │
│                                                                      │
│   <UserJourneyBehaviors>                                            │
│     <JourneyInsights                                                │
│       TelemetryEngine="ApplicationInsights"                        │
│       InstrumentationKey="your-app-insights-key"                   │
│       DeveloperMode="true"                                         │
│       ClientEnabled="true"                                         │
│       ServerEnabled="true"                                         │
│       TelemetryVersion="1.0.0" />                                  │
│   </UserJourneyBehaviors>                                          │
│                                                                      │
│   What You Get:                                                      │
│   • User journey step timing                                        │
│   • Technical profile execution                                     │
│   • Claims transformation results                                   │
│   • Detailed error information                                      │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Error Code Reference

| Error Code | Description | Solution |
|------------|-------------|----------|
| **90006** | Invalid redirect URI | Match exact URI in app registration |
| **90014** | Missing parameter | Add required params (client_id, etc.) |
| **90117** | Invalid authority | Check tenant/policy in authority URL |
| **90205** | No email claim | User didn't provide email or IdP didn't |
| **90238** | MFA phone unverified | User needs to verify phone |
| **90256** | User cancelled | User closed the flow |
| **99002** | Policy not found | Upload policy or check policy name |

## 🎯 Exam Tips

> **Remember:**
> - **AADB2C** error codes are B2C-specific
> - **Correlation ID** is essential for support tickets
> - **Application Insights** provides detailed custom policy debugging
> - **jwt.ms** helps decode and analyze tokens
> - Test policies using **"Run now"** button in Azure Portal
> - Most issues are **configuration mismatches** (URIs, tenant names)

## 💡 Key Takeaways

1. 🔍 Always check error codes and correlation IDs
2. 📋 Most errors are configuration mismatches
3. 🔐 Token issues usually involve audience or issuer
4. 🎨 Custom UI needs CORS, HTTPS, and `<div id="api">`
5. 📜 Custom policies must be uploaded in correct order
6. 📊 Use Application Insights for detailed debugging
