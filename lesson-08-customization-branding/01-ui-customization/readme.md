# 🎨 UI Customization

## 🔑 Overview

Azure AD B2C allows extensive customization of the authentication UI to match your brand identity. You can modify colors, logos, layouts, and even create fully custom HTML templates.

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🎨 UI Customization Options                       │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐          │
│   │  🎨 Basic   │     │ 📄 Page     │     │ 🖥️ Custom  │          │
│   │  Branding   │     │  Templates  │     │   HTML      │          │
│   └─────────────┘     └─────────────┘     └─────────────┘          │
│         │                   │                   │                   │
│         ▼                   ▼                   ▼                   │
│   • Logo             • Ocean Blue        • Full control            │
│   • Background       • Slate Gray        • Your hosting            │
│   • Colors           • Classic           • JavaScript OK           │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │     Flexibility ────────────────────────────────►           │   │
│   │     Low                  Medium                  High       │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 📋 Customization Levels

| Level | What You Can Change | Complexity |
|-------|---------------------|------------|
| **Company Branding** | Logo, background, colors | Easy |
| **Page Templates** | Pre-built themes | Easy |
| **Page Layouts** | Layout selection | Medium |
| **Custom HTML** | Full page control | Advanced |
| **Custom Policies + HTML** | Complete control | Expert |

## 🏢 Company Branding

```
┌─────────────────────────────────────────────────────────────────────┐
│                    🏢 Company Branding Setup                         │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   Azure Portal → Azure AD B2C → Company branding                    │
│                                                                      │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  Sign-in page background image                               │   │
│   │  ┌─────────────────────────────────────────────────────┐    │   │
│   │  │  📷 Upload: background.jpg                           │    │   │
│   │  │  Recommended: 1920x1080, < 300KB                    │    │   │
│   │  └─────────────────────────────────────────────────────┘    │   │
│   │                                                              │   │
│   │  Banner logo                                                 │   │
│   │  ┌─────────────────────────────────────────────────────┐    │   │
│   │  │  📷 Upload: logo.png                                 │    │   │
│   │  │  Recommended: 280x60, PNG with transparency         │    │   │
│   │  └─────────────────────────────────────────────────────┘    │   │
│   │                                                              │   │
│   │  Square logo (dark theme)                                   │   │
│   │  ┌─────────────────────────────────────────────────────┐    │   │
│   │  │  📷 Upload: logo-square.png                          │    │   │
│   │  │  Recommended: 240x240, PNG                          │    │   │
│   │  └─────────────────────────────────────────────────────┘    │   │
│   │                                                              │   │
│   │  Username hint text: [ Enter your email ]                   │   │
│   │  Sign-in page text: [ Welcome to Contoso ]                  │   │
│   │                                                              │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Branding Elements

| Element | Size | Format | Notes |
|---------|------|--------|-------|
| **Banner Logo** | 280x60 px | PNG, JPG, GIF | Appears at top of sign-in |
| **Square Logo** | 240x240 px | PNG, JPG, GIF | Used in dark theme |
| **Background Image** | 1920x1080 px | PNG, JPG | < 300 KB |
| **Background Color** | N/A | Hex color | Fallback if no image |
| **Sign-in Text** | N/A | Text | Custom welcome message |

## 📄 Built-in Page Templates

```
┌─────────────────────────────────────────────────────────────────────┐
│                    📄 Available Templates                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐    │
│   │   🌊 Ocean      │  │   🪨 Slate      │  │   📜 Classic    │    │
│   │     Blue        │  │     Gray        │  │                 │    │
│   │ ┌───────────┐   │  │ ┌───────────┐   │  │ ┌───────────┐   │    │
│   │ │  Modern   │   │  │ │Professional│  │  │ │Traditional│   │    │
│   │ │  clean    │   │  │ │  neutral  │   │  │ │  simple   │   │    │
│   │ │  blue     │   │  │ │  gray     │   │  │ │  white    │   │    │
│   │ └───────────┘   │  │ └───────────┘   │  │ └───────────┘   │    │
│   └─────────────────┘  └─────────────────┘  └─────────────────┘    │
│                                                                      │
│   Configuration in User Flow → Properties → Page layouts            │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

## 🖥️ Custom HTML/CSS

### Enabling Custom Page Content

```
┌─────────────────────────────────────────────────────────────────────┐
│                 🖥️ Custom HTML Setup                                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   1. Host your HTML file (must be HTTPS, CORS enabled)              │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  https://yourstorage.blob.core.windows.net/templates/       │   │
│   │  ├── signin.html                                             │   │
│   │  ├── signup.html                                             │   │
│   │  ├── mfa.html                                                │   │
│   │  └── error.html                                              │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
│   2. Configure User Flow                                            │
│   ┌─────────────────────────────────────────────────────────────┐   │
│   │  User Flow → Page layouts → Unified sign up or sign in      │   │
│   │                                                              │   │
│   │  Use custom page content: ● Yes ○ No                        │   │
│   │  Custom page URI: https://storage.../templates/signin.html  │   │
│   └─────────────────────────────────────────────────────────────┘   │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### Custom HTML Template Structure

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Sign In - Contoso</title>
  <link href="https://yourdomain.com/styles.css" rel="stylesheet">
</head>
<body>
  <!-- Your custom header -->
  <header>
    <img src="https://yourdomain.com/logo.png" alt="Contoso">
  </header>

  <!-- B2C injects content here - DO NOT REMOVE -->
  <div id="api">
    <!-- Azure AD B2C will inject the sign-in form here -->
  </div>

  <!-- Your custom footer -->
  <footer>
    <p>© 2024 Contoso. All rights reserved.</p>
  </footer>

  <script src="https://yourdomain.com/custom.js"></script>
</body>
</html>
```

### Required Hosting Configuration

| Requirement | Description |
|-------------|-------------|
| **HTTPS** | All resources must use HTTPS |
| **CORS** | Enable cross-origin requests from your B2C tenant |
| **Public Access** | Files must be publicly accessible |
| **Content-Type** | Correct MIME types for all files |

## 🎨 CSS Customization

```css
/* Example: Custom styling for B2C elements */

/* Main container */
#api {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

/* Form labels */
.entry-item label {
  font-weight: 600;
  color: #333;
}

/* Input fields */
.entry-item input {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

/* Primary button (Sign In) */
#next {
  background-color: #0078D4;
  color: white;
  padding: 12px 24px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

#next:hover {
  background-color: #005a9e;
}

/* Error messages */
.error {
  color: #d32f2f;
  font-size: 14px;
}

/* Social login buttons */
.accountButton {
  display: flex;
  align-items: center;
  padding: 10px;
  margin: 5px 0;
  border: 1px solid #ddd;
  border-radius: 4px;
}
```

## 📜 Custom Policy Content Definitions

```xml
<!-- Define content definitions in custom policy -->
<ContentDefinitions>
  <ContentDefinition Id="api.signuporsignin">
    <LoadUri>https://yourstorage.blob.core.windows.net/templates/unified.html</LoadUri>
    <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:2.1.7</DataUri>
  </ContentDefinition>

  <ContentDefinition Id="api.selfasserted">
    <LoadUri>https://yourstorage.blob.core.windows.net/templates/selfAsserted.html</LoadUri>
    <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:selfasserted:2.1.7</DataUri>
  </ContentDefinition>

  <ContentDefinition Id="api.phonefactor">
    <LoadUri>https://yourstorage.blob.core.windows.net/templates/mfa.html</LoadUri>
    <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
    <DataUri>urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.2.4</DataUri>
  </ContentDefinition>
</ContentDefinitions>
```

## ⚠️ Common Customization Issues

| Issue | Cause | Solution |
|-------|-------|----------|
| **Page not loading** | CORS not configured | Enable CORS for your B2C tenant |
| **Styles not applying** | CSS blocked | Use HTTPS, check MIME type |
| **Form missing** | No `<div id="api">` | Add the required container |
| **JavaScript errors** | Inline scripts blocked | Use external JS files |
| **Mixed content** | HTTP resources on HTTPS | Use HTTPS for all resources |

## 🎯 Exam Tips

> **Remember:**
> - Custom HTML must include `<div id="api">` for B2C to inject content
> - All resources must use **HTTPS**
> - **CORS** must be enabled on your hosting
> - Company branding is the **easiest** customization option
> - **Page contracts** (DataUri) specify B2C features available
> - Custom policies offer **most flexibility** for UI customization

## 💡 Key Takeaways

1. 🎨 Multiple customization levels: branding → templates → custom HTML
2. 🏢 Company branding is quick setup via Azure Portal
3. 🖥️ Custom HTML requires HTTPS hosting with CORS
4. 📋 Always include `<div id="api">` for B2C content injection
5. 📜 Custom policies enable content definitions for each page type
6. ⚠️ Test all customizations thoroughly across browsers
