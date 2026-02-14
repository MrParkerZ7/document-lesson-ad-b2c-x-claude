# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an Azure AD B2C (Azure Active Directory Business-to-Consumer) study guide repository containing lessons organized by core concepts. Each lesson topic has a `readme.md` (study content) and `diagram.drawio` (visual diagram).

## Repository Structure

```
lesson-XX-topic-name/
  ├── 01-subtopic/
  │   ├── readme.md      # Study content with tables, ASCII diagrams, exam tips
  │   └── diagram.drawio # Visual diagram in Draw.io XML format
  └── 02-subtopic/
      └── ...
```

## Content Standards

### readme.md Files
- Include exam-focused content with key concepts, definitions, and comparisons
- Use markdown tables for structured information
- Include ASCII art diagrams for visual representation
- Add "🎯 Exam Tips" sections highlighting what to remember for the test
- End with "💡 Key Takeaways" as numbered bullet points
- Use icon symbols in titles and section headers for visual recognition

**Icon Symbols Guide:**
| Icon | Category | Usage |
|------|----------|-------|
| ☁️ | Cloud | General cloud concepts, Azure overview |
| 🔐 | Security | Authentication, encryption, access control |
| 🛡️ | Protection | MFA, Conditional Access, compliance |
| 💰 | Cost | Pricing, billing, MAU (Monthly Active Users) |
| 🌍 | Global | Regions, availability, geo-redundancy |
| ⚡ | Performance | Token issuance, speed optimization |
| 📊 | Analytics | Sign-in logs, audit logs, monitoring |
| 🔑 | Identity | User accounts, identity providers |
| 🖥️ | Application | App registrations, client apps |
| 🔗 | Integration | APIs, connectors, social logins |
| 🔄 | Flows | User flows, custom policies |
| 🎨 | Customization | UI branding, page layouts |
| 🤖 | Automation | Graph API, PowerShell |
| ✅ | Best Practice | Recommendations, guidelines |
| ⚠️ | Warning | Important notes, cautions |
| 🎯 | Exam Tips | Test focus areas |
| 💡 | Takeaways | Key insights, summary |
| 👤 | User | Consumer accounts, profiles |
| 🏢 | Tenant | B2C directory, organization |
| 📜 | Policy | Custom policies, trust framework |
| 🎫 | Token | JWT, ID tokens, access tokens |

### diagram.drawio Styling Requirements

All diagrams must follow this Azure-themed styling:

**Color Scheme:**
- `#0078D4` - Azure Blue (primary accent, arrows, highlights) 🔵
- `#232F3E` - Dark (containers, service blocks) ⬛
- `#50E6FF` - Azure Cyan (secondary accent, connections) 🔷
- `#1E8900` - Green (positive/benefits, success states) 🟢
- `#DD3522` - Red (security, warnings, error states) 🔴
- `#FF8C00` - Orange (identity providers, social logins) 🟠
- `#7B2D8E` - Purple (custom policies, advanced features) 🟣

**Icon Symbols in Diagrams:**
Use text labels with icons for visual clarity in diagram elements:
- `☁️` Azure services and cloud icons
- `🔐` Security/authentication components
- `🔑` Identity providers
- `👤` User/consumer elements
- `🎫` Token elements
- `🔄` Flow/process indicators
- `🖥️` Application components
- `🔗` Integration connections
- `✅` Success/benefits
- `❌` Drawbacks/limitations

**Required Style Attributes:**
- Container/group boxes: `rounded=0;shadow=1;`
- Animated arrows: `flowAnimation=1;shadow=1;`
- All shapes include: `shadow=1;`
- Service blocks: `rounded=0;whiteSpace=wrap;html=1;fillColor=#0078D4;strokeColor=none;fontColor=#FFFFFF;`

**Example container style:**
```xml
style="whiteSpace=wrap;html=1;fillColor=#E6F2FF;strokeColor=#0078D4;strokeWidth=2;rounded=0;shadow=1;"
```

**Example arrow style:**
```xml
style="endArrow=classic;html=1;strokeWidth=2;strokeColor=#0078D4;flowAnimation=1;shadow=1;"
```

## Lesson Structure

| Lesson | Topic | Description |
|--------|-------|-------------|
| 01 | Overview & Core Concepts | Introduction to Azure AD B2C, use cases, vs Azure AD |
| 02 | Tenant & Directory | B2C tenant setup, directory structure |
| 03 | User Flows | Built-in user flows (sign-up, sign-in, etc.) |
| 04 | Custom Policies | Identity Experience Framework, XML policies |
| 05 | Identity Providers | Social & enterprise identity providers |
| 06 | Tokens & Claims | JWT tokens, claims, token customization |
| 07 | Application Integration | App registration, authentication libraries |
| 08 | Security Features | MFA, Conditional Access, fraud protection |
| 09 | Customization & Branding | UI customization, page layouts |
| 10 | Monitoring & Troubleshooting | Logs, diagnostics, common issues |
