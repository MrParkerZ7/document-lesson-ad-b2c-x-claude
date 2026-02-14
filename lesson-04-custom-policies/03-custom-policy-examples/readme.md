# 📜 Custom Policy Examples

## 🔑 Overview

This section provides practical examples of custom policies for common Azure AD B2C scenarios. Each example demonstrates real-world implementation patterns.

```
┌─────────────────────────────────────────────────────────────────────┐
│                  📜 Common Custom Policy Scenarios                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   🔗 REST API        🔀 Conditional      🔄 Account        🎫 Custom │
│   Integration        Flows              Migration         Claims    │
│       │                  │                   │               │      │
│       ▼                  ▼                   ▼               ▼      │
│   ┌────────┐        ┌────────┐         ┌────────┐      ┌────────┐  │
│   │ Fraud  │        │ Region │         │ Legacy │      │ Role   │  │
│   │ Check  │        │ Based  │         │ System │      │Mapping │  │
│   └────────┘        └────────┘         └────────┘      └────────┘  │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📞 Example 1: REST API Integration

Call an external API to validate user data during sign-up.

### Technical Profile

```xml
<TechnicalProfile Id="REST-ValidateUserData">
  <DisplayName>Validate user data via API</DisplayName>
  <Protocol Name="Proprietary"
            Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine">
  </Protocol>
  <Metadata>
    <Item Key="ServiceUrl">https://api.example.com/validate</Item>
    <Item Key="AuthenticationType">Bearer</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="AllowInsecureAuthInProduction">false</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BearerAuthenticationToken" StorageReferenceId="B2C_1A_RestApiKey" />
  </CryptographicKeys>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" />
    <InputClaim ClaimTypeReferenceId="displayName" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isValidUser" />
    <OutputClaim ClaimTypeReferenceId="riskScore" />
  </OutputClaims>
</TechnicalProfile>
```

### REST API Authentication Methods

| Method | Metadata Key | Use Case |
|--------|--------------|----------|
| **None** | `None` | Internal/trusted APIs |
| **Basic** | `Basic` | Simple username/password |
| **Bearer** | `Bearer` | OAuth 2.0 tokens |
| **ClientCertificate** | `ClientCertificate` | Mutual TLS |
| **ApiKeyHeader** | `ApiKeyHeader` | API key in header |

## 🔀 Example 2: Conditional Flow Based on Email Domain

Route users to different flows based on their email domain.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔀 Conditional Flow Logic                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│                      👤 User Signs In                                │
│                            │                                         │
│                            ▼                                         │
│                   ┌─────────────────┐                               │
│                   │  Extract Email  │                               │
│                   │     Domain      │                               │
│                   └────────┬────────┘                               │
│                            │                                         │
│            ┌───────────────┼───────────────┐                        │
│            ▼               ▼               ▼                        │
│    @company.com     @partner.com      @gmail.com                    │
│         │                 │                │                        │
│         ▼                 ▼                ▼                        │
│   ┌──────────┐     ┌──────────┐     ┌──────────┐                   │
│   │ Employee │     │ Partner  │     │ Consumer │                   │
│   │   Flow   │     │   Flow   │     │   Flow   │                   │
│   └──────────┘     └──────────┘     └──────────┘                   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Claims Transformation

```xml
<ClaimsTransformation Id="GetDomainFromEmail"
                       TransformationMethod="ParseDomain">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="emailAddress" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="domain" />
  </OutputClaims>
</ClaimsTransformation>
```

### Precondition in User Journey

```xml
<OrchestrationStep Order="3" Type="ClaimsExchange">
  <Preconditions>
    <Precondition Type="ClaimEquals" ExecuteActionsIf="false">
      <Value>domainName</Value>
      <Value>company.com</Value>
      <Action>SkipThisOrchestrationStep</Action>
    </Precondition>
  </Preconditions>
  <ClaimsExchanges>
    <ClaimsExchange Id="EmployeeFlow" TechnicalProfileReferenceId="AAD-EmployeeProfile" />
  </ClaimsExchanges>
</OrchestrationStep>
```

## 🔄 Example 3: Just-in-Time Migration

Migrate users from a legacy system during their first sign-in.

### Migration Flow

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔄 JIT Migration Flow                             │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  Step 1              Step 2              Step 3              Step 4 │
│  ┌──────┐           ┌──────┐           ┌──────┐           ┌──────┐ │
│  │Login │           │Check │           │Call  │           │Create│ │
│  │Form  │──────────►│B2C   │──────────►│Legacy│──────────►│B2C   │ │
│  │      │           │User? │           │API   │           │User  │ │
│  └──────┘           └──────┘           └──────┘           └──────┘ │
│                          │                                    │     │
│                          │ User Exists                        │     │
│                          └────────────────────────────────────┘     │
│                                   Normal Login                       │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Technical Profile for Legacy Lookup

```xml
<TechnicalProfile Id="REST-LegacyUserLookup">
  <DisplayName>Legacy System User Lookup</DisplayName>
  <Protocol Name="Proprietary"
            Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine" />
  <Metadata>
    <Item Key="ServiceUrl">https://legacy.example.com/api/users/validate</Item>
    <Item Key="SendClaimsIn">Body</Item>
    <Item Key="AuthenticationType">Basic</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_LegacyApiUsername" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_LegacyApiPassword" />
  </CryptographicKeys>
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="signInName" PartnerClaimType="username" />
    <InputClaim ClaimTypeReferenceId="password" PartnerClaimType="password" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="tokenSuccess" />
    <OutputClaim ClaimTypeReferenceId="legacyUserId" />
    <OutputClaim ClaimTypeReferenceId="displayName" />
    <OutputClaim ClaimTypeReferenceId="email" />
  </OutputClaims>
</TechnicalProfile>
```

## 🎫 Example 4: Custom Claims Enrichment

Add custom claims to the token from external sources.

### Claims Transformation Examples

| Transformation | Purpose | Example |
|----------------|---------|---------|
| **CreateStringClaim** | Create static claim | Add app version |
| **FormatStringClaim** | Format claim value | Combine first + last name |
| **GetCurrentDateTime** | Add timestamp | Login time |
| **CompareClaims** | Compare values | Age verification |
| **AssertStringClaimsAreEqual** | Validate equality | Confirm email |

### Example: Combine Claims

```xml
<ClaimsTransformation Id="CreateFullName"
                       TransformationMethod="FormatStringMultipleClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="surname" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="{0} {1}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="displayName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

## 🔐 Example 5: Multi-Factor Authentication

Add custom MFA logic with conditional triggers.

```
┌─────────────────────────────────────────────────────────────────────┐
│                     🔐 Conditional MFA Flow                          │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   👤 User Login                                                      │
│        │                                                             │
│        ▼                                                             │
│   ┌─────────────────────────────────────────┐                       │
│   │          🛡️ Risk Assessment              │                       │
│   │  • New device?                           │                       │
│   │  • Unusual location?                     │                       │
│   │  • Sensitive operation?                  │                       │
│   └────────────────────┬────────────────────┘                       │
│                        │                                             │
│         ┌──────────────┴──────────────┐                             │
│         ▼                             ▼                              │
│    Low Risk                      High Risk                           │
│   ┌─────────┐                   ┌─────────┐                         │
│   │ ✅ Allow │                   │ 🔐 MFA  │                         │
│   │  Access │                   │Required │                         │
│   └─────────┘                   └─────────┘                         │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📁 Starter Pack Policy Files

| File | Purpose |
|------|---------|
| `TrustFrameworkBase.xml` | Core building blocks |
| `TrustFrameworkLocalization.xml` | Multi-language support |
| `TrustFrameworkExtensions.xml` | Your customizations |
| `SignUpOrSignIn.xml` | Sign-up/sign-in RP |
| `ProfileEdit.xml` | Profile editing RP |
| `PasswordReset.xml` | Password reset RP |

## 🎯 Exam Tips

> **Remember:**
> - REST API calls require a **Technical Profile** with RestfulProvider
> - **Preconditions** control which orchestration steps execute
> - **Claims Transformations** manipulate claim values
> - JIT migration uses REST to call legacy systems and creates B2C accounts
> - **Output Claims** in RelyingParty define what goes in the token
> - Custom policies can integrate with **any REST API** for enrichment

## 💡 Key Takeaways

1. 📞 REST API integration enables external validation and data enrichment
2. 🔀 Preconditions allow conditional execution of orchestration steps
3. 🔄 JIT migration seamlessly moves users from legacy systems
4. 🎫 Claims transformations can combine, format, and validate claims
5. 🔐 Custom MFA logic can be triggered based on risk assessment
6. 📁 Always start with the official starter pack from GitHub
