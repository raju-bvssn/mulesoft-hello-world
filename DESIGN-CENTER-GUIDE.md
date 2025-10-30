# Anypoint Design Center Guide

Complete guide for creating, editing, and publishing the Hello World API in Anypoint Design Center.

**Date**: October 30, 2025  
**Status**: Ready for Design Center Import

---

## Overview

This guide walks you through importing your compliant RAML specification into Anypoint Design Center, making any necessary edits, and publishing directly to Anypoint Exchange.

---

## Prerequisites

✅ Anypoint Platform account  
✅ Access to Design Center  
✅ Compliant RAML file: `hello-world-api-compliant.raml`  
✅ Organization ID: `d7699a02-16e5-43d5-ae1c-875f3a9196e0`

---

## Method 1: Import RAML to Design Center (Recommended)

### Step 1: Access Design Center

1. Navigate to [Anypoint Platform](https://anypoint.mulesoft.com)
2. Log in with your credentials
3. Click **Design Center** in the left navigation
4. Click **Create** → **Create API Specification**

### Step 2: Configure New API Project

1. **Name**: `Hello World API - Compliant`
2. **Project Type**: API Specification
3. **RAML Version**: RAML 1.0
4. Click **Create**

### Step 3: Import the RAML Content

#### Option A: Copy-Paste (Quick)

1. Open the Design Center editor
2. Delete the default RAML content
3. Copy the entire content from:
   ```
   /Users/vbolisetti/mulesoft-hello-world/src/main/resources/api/hello-world-api-compliant.raml
   ```
4. Paste into Design Center editor
5. Click **Save**

#### Option B: Upload File

1. In Design Center, click **Import** → **File or URL**
2. Select **File**
3. Browse to: `/Users/vbolisetti/mulesoft-hello-world/src/main/resources/api/hello-world-api-compliant.raml`
4. Click **Import**
5. Click **Save**

### Step 4: Validate the RAML

Design Center will automatically validate your RAML:

✅ **Expected: 0 Errors**  
✅ **Expected: 0 Warnings**

If you see any issues:
- Click on the error/warning to see details
- Fix inline in Design Center
- Save after each fix

### Step 5: Test the API in Mocking Service

1. Click **Mocking Service** toggle (top-right)
2. Copy the mocking service URL
3. Test endpoints:

```bash
# Test GET /hello
curl {MOCKING_URL}/hello

# Test GET /hello/{name}
curl {MOCKING_URL}/hello/John

# Test POST /greet
curl -X POST {MOCKING_URL}/greet \
  -H "Content-Type: application/json" \
  -d '{"name":"Jane","language":"es"}'

# Test GET /health
curl {MOCKING_URL}/health
```

### Step 6: Publish to Exchange from Design Center

1. Click **Publish** button (top-right)
2. Configure publication:
   - **API Version**: `v1`
   - **Asset Version**: `1.0.1` (increment from current 1.0.0)
   - **API Asset Type**: REST API
   - **Main file**: `hello-world-api-compliant.raml` (auto-selected)
3. Click **Publish to Exchange**

---

## Method 2: Link Existing Exchange Asset

If you want to update the existing Exchange asset instead of creating a new one:

### Step 1: Create from Exchange Asset

1. Go to Design Center
2. Click **Create** → **Create API Specification**
3. Select **Import**
4. Choose **From Exchange**
5. Search for: **Hello World API**
6. Select version **1.0.0**
7. Click **Create from Asset**

### Step 2: Replace Content

1. Select all content in editor (Cmd+A / Ctrl+A)
2. Delete
3. Paste content from `hello-world-api-compliant.raml`
4. Click **Save**

### Step 3: Publish Update

1. Click **Publish**
2. **Asset Version**: `1.0.1` (new version)
3. **API Version**: Keep as `v1`
4. Add **Release Notes**:
   ```
   ## Version 1.0.1 Release Notes
   
   ### Security Enhancements
   - Added Client ID Enforcement security scheme
   - All endpoints (except /health) now require authentication
   
   ### API Design Improvements
   - Added operationId to all operations
   - Defined 6 named types (reusable schemas)
   - Added standard HTTP status codes (400, 401, 403, 404, 415, 500)
   - Added response headers (Content-Type, X-Correlation-ID)
   
   ### Documentation Updates
   - Updated baseUri to production CloudHub URL
   - Added comprehensive endpoint descriptions
   - Added authentication documentation
   - Completed all property descriptions
   
   ### Compliance
   - ✅ Fully compliant with Anypoint Best Practices 1.6.x
   - ✅ Fully compliant with Authentication Security Best Practices 1.1.x
   - ✅ 0 violations, 0 warnings
   ```
5. Click **Publish to Exchange**

---

## Design Center Features to Utilize

### 1. Visual API Designer

Use the visual designer for easier editing:
- Click **Visual** tab (next to Code tab)
- Edit endpoints, types, and responses visually
- Switch back to Code tab to see RAML

### 2. API Console (Built-in Testing)

Test your API right in Design Center:
1. Click **API Console** tab
2. Expand endpoints
3. Click **Try It**
4. Enter parameters
5. Click **Send**
6. View response

### 3. Documentation Preview

Preview how documentation will appear in Exchange:
1. Click **Documentation** tab
2. Review all sections
3. Make sure examples render correctly

### 4. Validate Compliance

Design Center automatically checks for:
- RAML syntax errors
- Security scheme issues
- Type mismatches
- Required fields

Look for the **green checkmark** (✅) = No issues!

---

## Common Design Center Tasks

### Update baseUri for Different Environments

```raml
# Development
baseUri: https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/{version}

# Or use environment variables
baseUri: https://{environment}.cloudhub.io/api/{version}
baseUriParameters:
  environment:
    type: string
    enum: [dev, qa, prod]
    default: dev
```

### Add More HTTP Status Codes

If you need additional error codes:

```raml
responses:
  429:
    description: Too Many Requests - Rate limit exceeded
    body:
      application/json:
        type: ErrorResponse
        example: |
          {
            "error": "Too Many Requests",
            "message": "Rate limit exceeded. Try again in 60 seconds",
            "timestamp": "2025-10-30T15:30:00Z"
          }
```

### Add Query Parameters

```raml
/hello:
  get:
    queryParameters:
      lang:
        type: string
        description: Language code
        required: false
        enum: [en, es, fr]
        default: en
```

### Add New Endpoints

```raml
/goodbye:
  get:
    displayName: Get Goodbye Message
    operationId: getGoodbye
    description: Returns a goodbye message
    responses:
      200:
        description: Successfully returned goodbye message
        body:
          application/json:
            type: HelloResponse
```

---

## Publishing Options

### Option 1: Minor Version Update (Recommended)

Use this when making compatible changes:
- Asset Version: `1.0.1`, `1.0.2`, etc.
- API Version: Keep as `v1`
- Consumers can use without changes

### Option 2: Major Version Update

Use this when making breaking changes:
- Asset Version: `2.0.0`
- API Version: `v2`
- Consumers must update their code

### Option 3: Snapshot (Development)

Use for testing:
- Asset Version: `1.0.1-SNAPSHOT`
- Not recommended for production

---

## Validation Checklist

Before publishing, verify:

- [ ] **RAML validates** without errors
- [ ] **All types are defined** (no inline types)
- [ ] **All operations have operationId**
- [ ] **Security scheme is defined**
- [ ] **All responses have descriptions**
- [ ] **baseUri points to production**
- [ ] **Examples are correct and working**
- [ ] **Documentation is comprehensive**
- [ ] **API Console works** (Try It feature)
- [ ] **Mocking service returns correct responses**

---

## Post-Publication Steps

### 1. Verify in Exchange

1. Go to **Exchange**
2. Find **Hello World API** (version 1.0.1)
3. Verify:
   - Documentation displays correctly
   - API Console works
   - Download RAML works
   - Examples are visible

### 2. Update Implementation

After publishing from Design Center, update your Mule app:

```bash
cd /Users/vbolisetti/mulesoft-hello-world

# Update pom.xml with new version
# Change: <version>1.0.0</version>
# To: <version>1.0.1</version>

# Rebuild and redeploy
mvn clean package mule:deploy -DmuleDeploy
```

### 3. Apply Security Policy (If Not Already Applied)

1. Go to **API Manager**
2. Select **Hello World API**
3. Click **Policies**
4. Apply **Client ID Enforcement**
5. Configure:
   - Client ID expression: `#[attributes.headers.client_id]`
   - Client Secret expression: `#[attributes.headers.client_secret]`

### 4. Test with Security

```bash
# This should fail (401)
curl https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/v1/hello

# This should succeed (with valid credentials)
curl https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/v1/hello \
  -H "client_id: YOUR_CLIENT_ID" \
  -H "client_secret: YOUR_CLIENT_SECRET"
```

### 5. Update API Documentation in Exchange

Update the Exchange documentation pages with security information:
- Add authentication examples
- Update getting started guide
- Add security policy information

---

## Troubleshooting

### Issue: RAML Validation Errors

**Symptom**: Red X icons, errors listed  
**Solution**:
1. Click on error to see details
2. Fix the specific line
3. Common issues:
   - Missing required fields
   - Type mismatches
   - Invalid RAML syntax

### Issue: "securitySchemes not supported"

**Symptom**: Error about x-client-id-enforcement  
**Solution**: This is normal - custom security schemes may show warnings in Design Center but work fine in APIkit.

### Issue: Can't Publish to Exchange

**Symptom**: Publish button disabled or fails  
**Solution**:
1. Ensure RAML has no errors
2. Check you have publish permissions
3. Verify asset name doesn't conflict
4. Try incrementing version number

### Issue: Mocking Service Doesn't Return Expected Data

**Symptom**: Mock responses don't match examples  
**Solution**:
1. Verify examples are under `example:` or `examples:` key
2. Check YAML indentation (must be exact)
3. Restart mocking service (toggle off/on)

---

## Advanced: Design Center API

You can also use Design Center API for automation:

```bash
# Get Design Center projects
curl https://anypoint.mulesoft.com/designcenter/api-designer/projects \
  -H "Authorization: Bearer YOUR_TOKEN"

# Create new project via API
curl -X POST https://anypoint.mulesoft.com/designcenter/api-designer/projects \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "hello-world-api-compliant",
    "type": "raml",
    "version": "1.0"
  }'
```

---

## Quick Reference

### Design Center URLs

- **Home**: https://anypoint.mulesoft.com/designcenter/
- **API Designer**: https://anypoint.mulesoft.com/designcenter/designer
- **Exchange**: https://anypoint.mulesoft.com/exchange/

### Keyboard Shortcuts

| Action | Mac | Windows/Linux |
|--------|-----|---------------|
| Save | Cmd+S | Ctrl+S |
| Find | Cmd+F | Ctrl+F |
| Undo | Cmd+Z | Ctrl+Z |
| Redo | Cmd+Shift+Z | Ctrl+Shift+Z |
| Format | Cmd+Shift+F | Ctrl+Shift+F |

### Important Files

| File | Location | Purpose |
|------|----------|---------|
| Compliant RAML | `src/main/resources/api/hello-world-api-compliant.raml` | Source of truth |
| Original RAML | `src/main/resources/api/hello-world-api.raml.backup` | Backup |
| Implementation | `src/main/mule/hello-world-api-implementation.xml` | Mule flows |

---

## Success Criteria

✅ RAML validates in Design Center (0 errors)  
✅ Mocking service works for all endpoints  
✅ Published to Exchange successfully  
✅ Documentation displays correctly in Exchange  
✅ API Console is functional  
✅ No compliance violations  
✅ Implementation updated and deployed  
✅ Security policy applied  
✅ End-to-end testing passes

---

## Support

- **Design Center Docs**: https://docs.mulesoft.com/design-center/
- **RAML Spec**: https://github.com/raml-org/raml-spec
- **Your Files**: `/Users/vbolisetti/mulesoft-hello-world/`

---

**Ready to go!** Your compliant RAML is ready for Design Center. Just follow the steps above and you'll have a production-grade API published to Exchange! 🚀

