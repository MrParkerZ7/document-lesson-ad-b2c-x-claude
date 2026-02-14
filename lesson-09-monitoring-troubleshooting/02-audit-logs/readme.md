# 📋 Audit Logs

## 🔑 Overview

Audit logs in Azure AD B2C record administrative and system activities. They track changes to policies, user management, and configuration modifications - essential for compliance and security monitoring.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📋 Audit Logs vs Sign-in Logs                     │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────────────────────┐    ┌─────────────────────────┐        │
│   │     📊 Sign-in Logs     │    │     📋 Audit Logs       │        │
│   ├─────────────────────────┤    ├─────────────────────────┤        │
│   │                         │    │                         │        │
│   │ • Authentication events │    │ • Administrative actions│        │
│   │ • User logins           │    │ • Policy changes        │        │
│   │ • MFA challenges        │    │ • User management       │        │
│   │ • Token issuance        │    │ • App registrations     │        │
│   │                         │    │ • Configuration changes │        │
│   │                         │    │                         │        │
│   │ WHO logged in?          │    │ WHO changed WHAT?       │        │
│   │                         │    │                         │        │
│   └─────────────────────────┘    └─────────────────────────┘        │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Audit Log Categories

| Category | Description | Examples |
|----------|-------------|----------|
| **Core Directory** | User and group management | User created, deleted, updated |
| **Application Management** | App registrations | App created, permissions changed |
| **Policy** | User flows and custom policies | Policy uploaded, modified |
| **B2C** | B2C-specific operations | Claims updated, IdP configured |
| **Device** | Device registration | Device registered, deleted |
| **Role Management** | Role assignments | Admin role assigned |

## 🔍 Accessing Audit Logs

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📍 Audit Logs Location                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Azure Portal → Azure AD B2C → Monitoring → Audit logs             │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │                      Filter Options                          │   │
│   │                                                              │   │
│   │  Date range:     [Last 24 hours          ▼]                 │   │
│   │  Service:        [All Services           ▼]                 │   │
│   │  Category:       [All Categories         ▼]                 │   │
│   │  Activity:       [All Activities         ▼]                 │   │
│   │  Status:         [All  ▼]  Success / Failure                │   │
│   │  Target:         [_________________________]                 │   │
│   │  Initiated by:   [_________________________]                 │   │
│   │                                                              │   │
│   │  [ Apply ]  [ Reset ]  [ Download ]                         │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📊 Common Audit Activities

### User Management

| Activity | Description | Category |
|----------|-------------|----------|
| **Add user** | New user created | Core Directory |
| **Delete user** | User removed | Core Directory |
| **Update user** | User properties changed | Core Directory |
| **Reset user password** | Admin password reset | Core Directory |
| **Disable account** | User disabled | Core Directory |

### Policy Operations

| Activity | Description | Category |
|----------|-------------|----------|
| **Set B2C user journey policy** | Custom policy uploaded | Policy |
| **Update B2C user journey policy** | Custom policy modified | Policy |
| **Delete B2C user journey policy** | Custom policy removed | Policy |
| **Create user flow** | User flow created | B2C |
| **Update user flow** | User flow modified | B2C |

### Application Management

| Activity | Description | Category |
|----------|-------------|----------|
| **Add application** | App registration created | Application Management |
| **Update application** | App configuration changed | Application Management |
| **Add service principal** | Service principal created | Application Management |
| **Add app role assignment** | Permissions granted | Application Management |

## 📝 Audit Log Entry Structure

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📝 Audit Log Entry Detail                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Activity Information                                               │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Activity:          Update user                              │   │
│   │  Activity Date:     2024-02-14 10:30:45 UTC                 │   │
│   │  Status:            ✅ Success                               │   │
│   │  Status Reason:     None                                     │   │
│   │  Category:          Core Directory                           │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   Initiated By (Actor)                                              │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  User:              admin@contoso.onmicrosoft.com           │   │
│   │  User ID:           abcd1234-...                            │   │
│   │  IP Address:        203.0.113.10                            │   │
│   │  User Agent:        Mozilla/5.0...                          │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   Target(s)                                                          │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Target:            john@email.com                          │   │
│   │  Target ID:         user-object-id                          │   │
│   │  Target Type:       User                                     │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   Modified Properties                                                │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Property:          DisplayName                              │   │
│   │  Old Value:         John Doe                                 │   │
│   │  New Value:         John Smith                               │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📈 Log Analytics Queries

```kusto
// All policy changes in the last 7 days
AuditLogs
| where TimeGenerated > ago(7d)
| where Category == "Policy"
| project TimeGenerated, OperationName, InitiatedBy.user.userPrincipalName,
          TargetResources[0].displayName, Result

// User management activities
AuditLogs
| where TimeGenerated > ago(24h)
| where Category == "UserManagement"
| project TimeGenerated, OperationName, InitiatedBy.user.userPrincipalName,
          TargetResources[0].userPrincipalName

// Failed administrative operations
AuditLogs
| where TimeGenerated > ago(24h)
| where Result == "failure"
| project TimeGenerated, OperationName, ResultReason,
          InitiatedBy.user.userPrincipalName

// Application changes
AuditLogs
| where TimeGenerated > ago(7d)
| where Category == "ApplicationManagement"
| project TimeGenerated, OperationName,
          TargetResources[0].displayName, Result

// Password reset operations
AuditLogs
| where TimeGenerated > ago(30d)
| where OperationName has "password"
| project TimeGenerated, OperationName, InitiatedBy.user.userPrincipalName,
          TargetResources[0].userPrincipalName
```

## 🔔 Setting Up Alerts

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🔔 Alert Configuration                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Azure Monitor → Alerts → Create alert rule                        │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Scope: Your Log Analytics workspace                         │   │
│   │                                                              │   │
│   │  Condition: Custom log search                                │   │
│   │  ┌───────────────────────────────────────────────────────┐  │   │
│   │  │  AuditLogs                                             │  │   │
│   │  │  | where OperationName == "Delete user"               │  │   │
│   │  │  | where Result == "success"                          │  │   │
│   │  └───────────────────────────────────────────────────────┘  │   │
│   │                                                              │   │
│   │  Threshold: Greater than 0                                   │   │
│   │  Evaluation period: Every 15 minutes                        │   │
│   │                                                              │   │
│   │  Actions:                                                    │   │
│   │  • Send email to security@contoso.com                       │   │
│   │  • Send SMS to on-call admin                                │   │
│   │  • Create ServiceNow ticket                                 │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔒 Compliance Use Cases

| Use Case | What to Monitor | Alert Condition |
|----------|-----------------|-----------------|
| **Privileged Access** | Admin role assignments | Role change detected |
| **Data Deletion** | User/data deletions | Delete operation success |
| **Policy Changes** | Custom policy uploads | Policy modification |
| **App Permissions** | Permission grants | New permissions added |
| **Suspicious Activity** | Failed operations | Multiple failures from IP |

## 📤 Export and Integration

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📤 Integration Options                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │                 Diagnostic Settings                          │   │
│   │                                                              │   │
│   │   AuditLogs ────► Log Analytics     ─► KQL Queries          │   │
│   │              │                        ─► Dashboards          │   │
│   │              │                        ─► Alerts              │   │
│   │              │                                               │   │
│   │              ├─► Storage Account    ─► Long-term archive     │   │
│   │              │                        ─► Compliance          │   │
│   │              │                                               │   │
│   │              └─► Event Hub          ─► SIEM integration      │   │
│   │                                       ─► Splunk, Sentinel    │   │
│   │                                       ─► Custom processing   │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## ⏰ Retention Periods

| Destination | Retention |
|-------------|-----------|
| **Azure Portal** | 7-30 days (license dependent) |
| **Log Analytics** | Up to 2 years (configurable) |
| **Storage Account** | Unlimited (your configuration) |
| **Event Hub** | Real-time streaming |

## 🎯 Exam Tips

> **Remember:**
> - Audit logs track **administrative activities**, not sign-ins
> - Categories include: Core Directory, Policy, Application Management, B2C
> - Each entry shows **who** (actor) changed **what** (target)
> - **Modified properties** show old and new values
> - Use **Diagnostic settings** for continuous export
> - **Log Analytics** enables KQL queries and alerts

## 💡 Key Takeaways

1. 📋 Audit logs record administrative and configuration changes
2. 🔍 Filter by category, activity type, actor, and target
3. 📝 Entries show who made what change with before/after values
4. 📈 Use Log Analytics for advanced querying and alerting
5. 🔒 Essential for compliance, security, and troubleshooting
6. 📤 Export to SIEM tools for security monitoring
