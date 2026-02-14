# 🕐 Token Lifetimes Configuration

## 🔑 Overview

Token lifetimes control how long tokens remain valid. Proper configuration balances security (shorter lifetimes) with user experience (longer lifetimes to reduce login frequency).

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🕐 Token Lifetime Overview                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Timeline (hours/days)                                              │
│   ─────────────────────────────────────────────────────────────────►│
│   0        1 hr       24 hrs              14 days        90 days    │
│   │         │           │                    │              │        │
│   │         │           │                    │              │        │
│   │    ┌────┴────┐      │                    │              │        │
│   │    │ Access  │      │                    │              │        │
│   │    │ Token   │      │                    │              │        │
│   │    │ (1 hr)  │      │                    │              │        │
│   │    └─────────┘      │                    │              │        │
│   │                     │                    │              │        │
│   │    ┌────────────────┴──┐                 │              │        │
│   │    │     ID Token      │                 │              │        │
│   │    │  (1 hr default)   │                 │              │        │
│   │    └───────────────────┘                 │              │        │
│   │                                          │              │        │
│   │    ┌─────────────────────────────────────┴─────┐        │        │
│   │    │         Refresh Token (14 days)           │        │        │
│   │    └───────────────────────────────────────────┘        │        │
│   │                                                         │        │
│   │    ┌────────────────────────────────────────────────────┴──┐    │
│   │    │      Sliding Window Maximum (90 days)                  │    │
│   │    └───────────────────────────────────────────────────────┘    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Default Token Lifetimes

| Token Type | Default | Minimum | Maximum |
|------------|---------|---------|---------|
| **ID Token** | 60 minutes | 5 minutes | 1 day |
| **Access Token** | 60 minutes | 5 minutes | 1 day |
| **Refresh Token** | 14 days | 24 hours | 90 days |
| **Refresh Token Sliding Window** | 90 days | 1 day | 365 days |
| **Authorization Code** | 10 minutes | N/A | N/A |

## ⚙️ Configuring Lifetimes in User Flows

```
┌─────────────────────────────────────────────────────────────────────┐
│                📝 User Flow Token Configuration                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Azure Portal → Azure AD B2C → User Flows → Select Flow            │
│                    ↓                                                 │
│              Properties                                              │
│                    ↓                                                 │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Token lifetime (minutes)                                    │   │
│   │                                                              │   │
│   │  Access & ID token lifetimes (minutes):  [  60  ]           │   │
│   │                                    Min: 5, Max: 1440         │   │
│   │                                                              │   │
│   │  Refresh token lifetime (days):         [  14  ]            │   │
│   │                                    Min: 1, Max: 90           │   │
│   │                                                              │   │
│   │  Refresh token sliding window:          [  90  ]            │   │
│   │                                    Min: 1, Max: 365          │   │
│   │                                                              │   │
│   │  ☐ Bounded (finite window)                                  │   │
│   │  ☑️ No Expiry (always active with usage)                     │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔄 Sliding Window Behavior

```
┌─────────────────────────────────────────────────────────────────────┐
│                  🔄 Sliding Window Explained                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Scenario: Refresh Token = 14 days, Sliding Window = 90 days       │
│                                                                      │
│   Day 0: User logs in                                               │
│   │       └── Refresh token valid until Day 14                      │
│   │                                                                  │
│   Day 10: User refreshes token                                      │
│   │       └── NEW refresh token valid until Day 24 (10+14)          │
│   │                                                                  │
│   Day 20: User refreshes token                                      │
│   │       └── NEW refresh token valid until Day 34 (20+14)          │
│   │                                                                  │
│   Day 80: User refreshes token                                      │
│   │       └── NEW refresh token valid until Day 90 (window limit!)  │
│   │                                                                  │
│   Day 90: Sliding window expires                                    │
│           └── User MUST re-authenticate                             │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  With "No Expiry": Sliding window never expires              │   │
│   │  as long as user keeps refreshing within refresh period      │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📜 Custom Policy Configuration

```xml
<!-- In your RelyingParty policy -->
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />

  <!-- Token lifetime configuration -->
  <UserJourneyBehaviors>
    <SingleSignOn Scope="Tenant" />
    <SessionExpiryType>Rolling</SessionExpiryType>
    <SessionExpiryInSeconds>86400</SessionExpiryInSeconds>
  </UserJourneyBehaviors>

  <TechnicalProfile Id="PolicyProfile">
    <Metadata>
      <!-- Access/ID token lifetime in seconds -->
      <Item Key="token_lifetime_secs">3600</Item>
      <!-- Refresh token lifetime in days -->
      <Item Key="refresh_token_lifetime_secs">1209600</Item>
      <!-- Sliding window in days -->
      <Item Key="rolling_refresh_token_lifetime_secs">7776000</Item>
    </Metadata>
  </TechnicalProfile>
</RelyingParty>
```

## 🔐 Security Considerations

| Setting | More Secure | Better UX | Recommendation |
|---------|-------------|-----------|----------------|
| **Short access token** | ✅ | ❌ | 1 hour for most apps |
| **Short refresh token** | ✅ | ❌ | 14 days for web apps |
| **Bounded sliding window** | ✅ | ❌ | 90 days for compliance |
| **Long refresh + sliding** | ❌ | ✅ | Mobile apps with biometrics |

## 📱 Platform Recommendations

| Platform | Access Token | Refresh Token | Sliding Window |
|----------|--------------|---------------|----------------|
| **Web App (SPA)** | 1 hour | 14 days | 90 days bounded |
| **Web App (Server)** | 1 hour | 14 days | 90 days bounded |
| **Mobile App** | 1 hour | 90 days | No expiry |
| **High Security** | 15 min | 1 day | 7 days bounded |
| **Kiosk/Shared** | 5 min | Disabled | N/A |

## 🔄 Token Refresh Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔄 Token Refresh Logic                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   App checks: Is access token expired or expiring soon?             │
│                          │                                           │
│            ┌─────────────┴─────────────┐                            │
│            │                           │                             │
│            ▼                           ▼                             │
│      ┌──────────┐              ┌──────────────┐                     │
│      │    No    │              │     Yes      │                     │
│      │Use Token │              │Refresh Token │                     │
│      └──────────┘              └──────┬───────┘                     │
│                                       │                              │
│                         Is refresh token valid?                      │
│                              │                                       │
│               ┌──────────────┴──────────────┐                       │
│               │                             │                        │
│               ▼                             ▼                        │
│        ┌──────────┐                 ┌──────────────┐                │
│        │   Yes    │                 │      No      │                │
│        │ Get new  │                 │ Re-login     │                │
│        │ tokens   │                 │ required     │                │
│        └──────────┘                 └──────────────┘                │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🚫 Revoking Tokens

| Method | Scope | Effect |
|--------|-------|--------|
| **Revoke via Graph API** | Single user | Invalidates refresh tokens |
| **Password change** | Single user | Invalidates all sessions |
| **User disabled** | Single user | Immediate access denial |
| **New policy version** | All users | Forces re-authentication |

### Graph API Revocation

```bash
# Revoke all refresh tokens for a user
POST /users/{id}/revokeSignInSessions
```

## 🎯 Exam Tips

> **Remember:**
> - Access/ID tokens: **5 minutes to 1 day** (default 1 hour)
> - Refresh tokens: **1 day to 90 days** (default 14 days)
> - Sliding window: **1 day to 365 days** (default 90 days)
> - **"No Expiry"** means unlimited sliding window with activity
> - Token lifetimes are configured per **user flow or policy**
> - Refresh tokens use **rotation** (new token each refresh)
> - **Graph API** can revoke user tokens

## 💡 Key Takeaways

1. 🕐 Token lifetimes balance security (short) vs UX (long)
2. ⚙️ Configure in User Flow properties or Custom Policy metadata
3. 🔄 Sliding window extends session as long as user is active
4. 📱 Different platforms need different lifetime strategies
5. 🚫 Tokens can be revoked via Graph API or password change
6. 🔐 Shorter lifetimes = more secure but more frequent logins
