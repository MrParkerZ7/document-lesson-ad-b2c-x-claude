# 📜 Custom Policy Structure

## 🔑 Overview

Custom policies in Azure AD B2C are defined using XML files that follow a specific schema. Understanding the structure is essential for building and troubleshooting identity journeys.

```
┌─────────────────────────────────────────────────────────────────────┐
│                   📜 Custom Policy XML Structure                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   <TrustFrameworkPolicy>                                            │
│   │                                                                  │
│   ├── 📦 BuildingBlocks                                             │
│   │   ├── ClaimsSchema                                              │
│   │   ├── Predicates                                                │
│   │   ├── ContentDefinitions                                        │
│   │   └── Localization                                              │
│   │                                                                  │
│   ├── 🔗 ClaimsProviders                                            │
│   │   ├── Technical Profiles                                        │
│   │   └── Claims Transformations                                    │
│   │                                                                  │
│   ├── 🔄 UserJourneys                                               │
│   │   ├── Orchestration Steps                                       │
│   │   └── Preconditions                                             │
│   │                                                                  │
│   └── 🖥️ RelyingParty                                               │
│       ├── DefaultUserJourney                                        │
│       └── TechnicalProfile                                          │
│                                                                      │
│   </TrustFrameworkPolicy>                                           │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 XML Elements Overview

| Element | Location | Purpose |
|---------|----------|---------|
| **TrustFrameworkPolicy** | Root | Container for entire policy |
| **BuildingBlocks** | Top-level | Reusable components (claims, predicates) |
| **ClaimsProviders** | Top-level | Technical profiles & transformations |
| **UserJourneys** | Top-level | Step-by-step authentication flows |
| **RelyingParty** | Top-level | Application entry point |

## 📦 BuildingBlocks Section

```xml
<BuildingBlocks>
  <!-- Define claim types -->
  <ClaimsSchema>
    <ClaimType Id="displayName">
      <DisplayName>Display Name</DisplayName>
      <DataType>string</DataType>
    </ClaimType>
  </ClaimsSchema>

  <!-- Password validation rules -->
  <Predicates>
    <Predicate Id="IsLengthRange" Method="IsLengthRange">
      <Parameters>
        <Parameter Id="Minimum">8</Parameter>
        <Parameter Id="Maximum">64</Parameter>
      </Parameters>
    </Predicate>
  </Predicates>

  <!-- UI page definitions -->
  <ContentDefinitions>
    <ContentDefinition Id="api.signuporsignin">
      <LoadUri>~/tenant/templates/AzureBlue/unified.cshtml</LoadUri>
    </ContentDefinition>
  </ContentDefinitions>
</BuildingBlocks>
```

### ClaimsSchema Elements

| Attribute | Description | Example |
|-----------|-------------|---------|
| **Id** | Unique claim identifier | `displayName` |
| **DisplayName** | Human-readable name | `Display Name` |
| **DataType** | Data type | `string`, `boolean`, `int`, `date` |
| **UserInputType** | UI control type | `TextBox`, `Password`, `RadioSingleSelect` |
| **Mask** | Input masking | `Simple`, `Regex` |

## 🔗 ClaimsProviders Section

```
┌─────────────────────────────────────────────────────────────────────┐
│                     🔗 Claims Providers                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌───────────────┐    ┌───────────────┐    ┌───────────────┐       │
│   │ 📞 Technical  │    │ 🔄 Claims     │    │ 🔐 Protocol   │       │
│   │   Profiles    │    │ Transformations│   │   Handlers    │       │
│   └───────────────┘    └───────────────┘    └───────────────┘       │
│          │                     │                    │                │
│          ▼                     ▼                    ▼                │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  • AAD-UserReadUsingObjectId      • CreateDisplayName       │   │
│   │  • REST-ValidateProfile           • GetCurrentDateTime      │   │
│   │  • login-NonInteractive           • AssertEmailVerified     │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Technical Profile Types

| Type | Protocol | Use Case |
|------|----------|----------|
| **Azure AD** | Proprietary | Read/write user attributes |
| **OAuth2** | OAuth 2.0 | Social identity providers |
| **SAML2** | SAML 2.0 | Enterprise federation |
| **REST** | REST API | External service calls |
| **Self-Asserted** | None | User input pages |
| **Claims Transformation** | None | Claims manipulation |

## 🔄 UserJourneys Section

```
┌─────────────────────────────────────────────────────────────────────┐
│                      🔄 User Journey Flow                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Step 1                Step 2                Step 3                 │
│   ┌─────────┐          ┌─────────┐          ┌─────────┐             │
│   │ 👤 IdP  │          │ 🔐 Read │          │ 🎫 Issue│             │
│   │Selection│────────►│ Profile │────────►│  Token  │             │
│   └─────────┘          └─────────┘          └─────────┘             │
│       │                     │                    │                   │
│       ▼                     ▼                    ▼                   │
│   Preconditions        Input Claims         Output Claims            │
│   ClaimsExchange       ClaimsExchange       SendClaims              │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Orchestration Step Types

| Type | Description | Example |
|------|-------------|---------|
| **ClaimsProviderSelection** | Display IdP selection buttons | Sign-in options |
| **ClaimsExchange** | Execute technical profile | Read user data |
| **SendClaims** | Issue token | Return to application |

## 🖥️ RelyingParty Section

```xml
<RelyingParty>
  <DefaultUserJourney ReferenceId="SignUpOrSignIn" />
  <TechnicalProfile Id="PolicyProfile">
    <DisplayName>PolicyProfile</DisplayName>
    <Protocol Name="OpenIdConnect" />
    <OutputClaims>
      <OutputClaim ClaimTypeReferenceId="displayName" />
      <OutputClaim ClaimTypeReferenceId="email" />
      <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub" />
    </OutputClaims>
  </TechnicalProfile>
</RelyingParty>
```

### RelyingParty Elements

| Element | Purpose |
|---------|---------|
| **DefaultUserJourney** | Which journey to execute |
| **Protocol** | Output protocol (OpenIdConnect, SAML2) |
| **OutputClaims** | Claims to include in token |
| **SubjectNamingInfo** | Token subject claim |

## 🔀 Policy Inheritance

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📂 Policy Hierarchy                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌───────────────────────────────────────────────────────────────┐ │
│   │  🏗️ TrustFrameworkBase.xml                                    │ │
│   │     └── Core definitions, common technical profiles           │ │
│   └───────────────────────────────────────────────────────────────┘ │
│                              │                                       │
│                              ▼                                       │
│   ┌───────────────────────────────────────────────────────────────┐ │
│   │  🧩 TrustFrameworkLocalization.xml                            │ │
│   │     └── Language-specific content                              │ │
│   └───────────────────────────────────────────────────────────────┘ │
│                              │                                       │
│                              ▼                                       │
│   ┌───────────────────────────────────────────────────────────────┐ │
│   │  🔧 TrustFrameworkExtensions.xml                              │ │
│   │     └── Tenant-specific customizations                         │ │
│   └───────────────────────────────────────────────────────────────┘ │
│                              │                                       │
│                              ▼                                       │
│   ┌───────────────────────────────────────────────────────────────┐ │
│   │  🖥️ SignUpOrSignIn.xml (Relying Party)                        │ │
│   │     └── Application entry point                                │ │
│   └───────────────────────────────────────────────────────────────┘ │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🎯 Exam Tips

> **Remember:**
> - Policy XML has **4 main sections**: BuildingBlocks, ClaimsProviders, UserJourneys, RelyingParty
> - **ClaimsSchema** defines all claim types used in the policy
> - **Technical Profiles** are the workhorses - they do the actual work
> - Policies use **inheritance** (BasePolicy attribute)
> - **Upload order matters**: Base → Extensions → Relying Party
> - **RelyingParty** defines what claims appear in the token

## 💡 Key Takeaways

1. 📜 Custom policies follow a strict XML schema with defined sections
2. 📦 BuildingBlocks contain reusable definitions (claims, predicates, content)
3. 🔗 ClaimsProviders house technical profiles that perform operations
4. 🔄 UserJourneys define the step-by-step authentication flow
5. 🖥️ RelyingParty is the entry point and defines token output
6. 📂 Policies inherit from parent policies using the BasePolicy attribute
