# 🛡️ Multi-Factor Authentication (MFA)

## 🔑 Overview

Multi-Factor Authentication adds an extra layer of security by requiring users to verify their identity using multiple methods. Azure AD B2C supports various MFA methods out of the box.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🛡️ MFA Authentication Factors                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐    │
│   │ 🔑 Something    │  │ 📱 Something    │  │ 👆 Something    │    │
│   │    You Know     │  │    You Have     │  │    You Are      │    │
│   └─────────────────┘  └─────────────────┘  └─────────────────┘    │
│          │                    │                    │                │
│          ▼                    ▼                    ▼                │
│   • Password            • Phone (SMS/Call)   • Fingerprint         │
│   • PIN                 • Authenticator App  • Face ID             │
│   • Security Questions  • Hardware Key       • Voice               │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │     Azure AD B2C MFA = Factor 1 (Password) + Factor 2      │   │
│   │                    (Phone or Email OTP)                      │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Supported MFA Methods

| Method | Description | User Experience |
|--------|-------------|-----------------|
| **Phone (SMS)** | Code sent via text message | Enter 6-digit code |
| **Phone (Call)** | Automated voice call | Press # to verify |
| **Email OTP** | Code sent to email | Enter verification code |
| **Authenticator App** | TOTP code generation | Enter app code |
| **FIDO2 Keys** | Hardware security keys | Touch/insert key |

## ⚙️ Enabling MFA in User Flows

```
┌─────────────────────────────────────────────────────────────────────┐
│                  📝 User Flow MFA Configuration                      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Azure Portal → Azure AD B2C → User Flows → Select Flow           │
│                    ↓                                                 │
│              Properties                                              │
│                    ↓                                                 │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Multifactor authentication                                  │   │
│   │                                                              │   │
│   │  Type of method:                                             │   │
│   │  ○ Off                                                       │   │
│   │  ○ Email                                                     │   │
│   │  ● Phone (SMS/Call)   ← Most common                         │   │
│   │  ○ Phone or Email                                            │   │
│   │                                                              │   │
│   │  MFA enforcement:                                            │   │
│   │  ○ Always on          ← Require for every login             │   │
│   │  ● Conditional        ← Based on Conditional Access          │   │
│   │  ○ Off                                                       │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📱 Phone Verification Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📱 SMS MFA Flow                                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   1. User enters email/password                                      │
│      │                                                               │
│      ▼                                                               │
│   2. B2C validates credentials                                       │
│      │                                                               │
│      ▼                                                               │
│   3. MFA Required - Show phone entry/selection                       │
│      ┌─────────────────────────────────────────────────────────┐    │
│      │  Verify your identity                                    │    │
│      │                                                          │    │
│      │  Phone number: [+1 555-123-4567]                        │    │
│      │                                                          │    │
│      │  ○ Send me a text message                                │    │
│      │  ○ Call me                                               │    │
│      │                                                          │    │
│      │  [Send Code]                                             │    │
│      └─────────────────────────────────────────────────────────┘    │
│      │                                                               │
│      ▼                                                               │
│   4. User receives SMS: "Your code is: 123456"                      │
│      │                                                               │
│      ▼                                                               │
│   5. User enters code                                                │
│      ┌─────────────────────────────────────────────────────────┐    │
│      │  Enter verification code                                 │    │
│      │                                                          │    │
│      │  Code: [123456]                                         │    │
│      │                                                          │    │
│      │  [Verify]                                                │    │
│      └─────────────────────────────────────────────────────────┘    │
│      │                                                               │
│      ▼                                                               │
│   6. ✅ Authentication complete                                      │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📜 MFA in Custom Policies

```xml
<!-- Phone Factor Technical Profile -->
<TechnicalProfile Id="PhoneFactor-InputOrVerify">
  <DisplayName>Phone Factor</DisplayName>
  <Protocol Name="Proprietary"
            Handler="Web.TPEngine.Providers.PhoneFactorProtocolProvider" />
  <Metadata>
    <Item Key="ContentDefinitionReferenceId">api.phonefactor</Item>
    <Item Key="ManualPhoneNumberEntryAllowed">true</Item>
    <Item Key="setting.authenticationMode">sms</Item>
    <Item Key="setting.autodial">true</Item>
  </Metadata>
  <InputClaimsTransformations>
    <InputClaimsTransformation ReferenceId="CreateUserIdForMFA" />
  </InputClaimsTransformations>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="userIdForMFA" PartnerClaimType="UserId" />
    <InputClaim ClaimTypeReferenceId="strongAuthenticationPhoneNumber" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="Verified.strongAuthenticationPhoneNumber"
                 PartnerClaimType="Verified.OfficePhone" />
    <OutputClaim ClaimTypeReferenceId="newPhoneNumberEntered" />
  </OutputClaims>
</TechnicalProfile>

<!-- Add to User Journey -->
<OrchestrationStep Order="4" Type="ClaimsExchange">
  <ClaimsExchanges>
    <ClaimsExchange Id="PhoneFactorExchange"
                    TechnicalProfileReferenceId="PhoneFactor-InputOrVerify" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## 🔐 MFA Enforcement Options

| Option | Description | Use Case |
|--------|-------------|----------|
| **Always** | MFA for every sign-in | High security apps |
| **Conditional** | Based on Conditional Access | Risk-based MFA |
| **Sign-up only** | MFA during registration | Verify phone ownership |
| **Remember device** | Skip MFA on trusted devices | Balance security/UX |

## 📊 MFA Methods Comparison

| Method | Security | User Experience | Cost |
|--------|----------|-----------------|------|
| **SMS** | Medium | Good | Per message |
| **Voice Call** | Medium | Good | Per call |
| **Email OTP** | Low-Medium | Good | Free |
| **Authenticator App** | High | Best | Free |
| **FIDO2** | Highest | Good | Hardware cost |

## ⚠️ SMS Security Considerations

```
┌─────────────────────────────────────────────────────────────────────┐
│                    ⚠️ SMS MFA Vulnerabilities                        │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Known Risks:                                                       │
│   • SIM swapping attacks                                            │
│   • SS7 network vulnerabilities                                     │
│   • Phone number porting fraud                                      │
│   • Social engineering at carriers                                  │
│                                                                      │
│   Mitigations:                                                       │
│   ✅ Use authenticator apps when possible                           │
│   ✅ Implement additional verification for phone changes            │
│   ✅ Combine with Conditional Access for risk assessment            │
│   ✅ Monitor for suspicious MFA registrations                       │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  SMS is better than no MFA, but authenticator apps          │   │
│   │  or FIDO2 keys provide stronger protection                  │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔄 MFA Registration Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│                  🔄 First-Time MFA Setup                             │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   During Sign-Up or First MFA Challenge:                            │
│                                                                      │
│   1. User prompted to register MFA method                           │
│      ┌─────────────────────────────────────────────────────────┐    │
│      │  Set up additional security                              │    │
│      │                                                          │    │
│      │  Enter your phone number:                                │    │
│      │  Country: [+1 United States ▼]                          │    │
│      │  Number:  [555-123-4567    ]                            │    │
│      │                                                          │    │
│      │  [Send verification code]                                │    │
│      └─────────────────────────────────────────────────────────┘    │
│                                                                      │
│   2. Phone number stored in user profile                            │
│      (strongAuthenticationPhoneNumber attribute)                    │
│                                                                      │
│   3. Future logins use saved phone number                           │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 💰 MFA Pricing

| Method | Azure AD B2C Pricing |
|--------|---------------------|
| **Phone (SMS)** | $0.03 per SMS |
| **Phone (Voice)** | $0.03 per call |
| **Email** | Included |
| **Authenticator App** | Included |
| **Custom MFA Provider** | Your provider's cost |

## 🎯 Exam Tips

> **Remember:**
> - MFA adds **second factor** beyond password
> - **Phone and Email** are the built-in MFA methods
> - MFA can be **Always on** or **Conditional**
> - In custom policies, use **PhoneFactorProtocolProvider**
> - Phone numbers stored in `strongAuthenticationPhoneNumber`
> - **SMS/Voice have per-use costs** ($0.03 each)
> - Authenticator apps provide **better security** than SMS

## 💡 Key Takeaways

1. 🛡️ MFA significantly improves security by requiring multiple factors
2. 📱 Azure AD B2C supports Phone (SMS/Call) and Email OTP
3. ⚙️ Configure MFA enforcement level per user flow
4. 📜 Custom policies enable advanced MFA scenarios
5. 💰 SMS and Voice have per-use costs
6. 🔐 Authenticator apps are more secure than SMS
