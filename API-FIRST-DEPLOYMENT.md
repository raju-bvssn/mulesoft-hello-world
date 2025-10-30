# API-First Implementation & Deployment Guide

## ✅ What We've Accomplished

### 1. **RAML API Specification Created** 
   - Location: `src/main/resources/api/hello-world-api.raml`
   - Defines 4 endpoints:
     - `GET /hello` - Simple hello world
     - `GET /hello/{name}` - Personalized greeting
     - `POST /greet` - Custom greeting with multi-language support
     - `GET /health` - Health check endpoint

### 2. **API Published to Anypoint Exchange**
   - **Organization**: `d7699a02-16e5-43d5-ae1c-875f3a9196e0`
   - **Asset ID**: `hello-world-api`
   - **Version**: `1.0.0`
   - **Type**: REST API (RAML)
   - **View in Exchange**: https://anypoint.mulesoft.com/exchange/d7699a02-16e5-43d5-ae1c-875f3a9196e0/hello-world-api/

### 3. **APIkit Implementation Scaffolded**
   - Implementation: `src/main/mule/hello-world-api-implementation.xml`
   - Uses APIkit router to auto-validate requests against RAML
   - Pulls API spec directly from Exchange as Maven dependency
   - Includes API Console for testing at `/console`

### 4. **Build Successful**
   - Application packaged: `target/mulesoft-hello-world-1.0.0-SNAPSHOT-mule-application.jar`
   - Ready for deployment to CloudHub 2.0

---

## 📦 Application Structure

```
mulesoft-hello-world/
├── src/main/
│   ├── mule/
│   │   ├── hello-world.xml                        # Original simple implementation
│   │   └── hello-world-api-implementation.xml     # ✨ NEW: API-first implementation
│   └── resources/api/
│       ├── hello-world-api.raml                   # ✨ RAML specification
│       ├── exchange.json                          # Exchange metadata
│       └── pom.xml                                # API publishing config
├── pom.xml                                        # Updated with APIkit dependencies
└── CLOUDHUB2-DEPLOYMENT.md                        # Deployment instructions
```

---

## 🚀 Deployment Options

### Option 1: Deploy via Anypoint Studio (Recommended)

1. **Open Anypoint Studio**
   ```bash
   open /Applications/AnypointStudio.app
   ```

2. **Import Project** (if not already imported)
   - File → Import → Anypoint Studio project from File System
   - Browse to: `/Users/vbolisetti/mulesoft-hello-world`

3. **Run Locally to Test**
   - Right-click project → Run As → Mule Application
   - Test endpoints:
     - http://localhost:8081/api/v1/hello
     - http://localhost:8081/api/v1/health
     - http://localhost:8081/console (API Console)

4. **Deploy to CloudHub 2.0**
   - Right-click project → Anypoint Platform → Deploy to CloudHub
   - Select:
     - **Environment**: Sandbox
     - **Target**: Cloudhub-US-East-2
     - **Runtime**: 4.9.10
     - **Application Name**: mulesoft-hello-world-dev
   - Click **Deploy**

### Option 2: Deploy via Anypoint Platform UI

1. **Package the Application**
   ```bash
   cd /Users/vbolisetti/mulesoft-hello-world
   mvn clean package
   ```

2. **Upload to CloudHub 2.0**
   - Go to https://anypoint.mulesoft.com
   - Runtime Manager → Deploy Application
   - **Upload JAR**: `target/mulesoft-hello-world-1.0.0-SNAPSHOT-mule-application.jar`
   - Configure:
     - **Name**: mulesoft-hello-world-dev
     - **Environment**: Sandbox
     - **Target**: Cloudhub-US-East-2
     - **Runtime**: 4.9.10
     - **vCores**: 0.1
     - **Replicas**: 1

### Option 3: Deploy via Maven (Advanced)

```bash
cd /Users/vbolisetti/mulesoft-hello-world
mvn clean package mule:deploy -DmuleDeploy
```

---

## 🧪 Testing the API

### After Deployment

Your API will be available at:
```
https://mulesoft-hello-world-dev.us-e2.cloudhub.io
```

### Test Endpoints

#### 1. Simple Hello
```bash
curl https://mulesoft-hello-world-dev.us-e2.cloudhub.io/api/v1/hello
```

**Expected Response:**
```json
{
  "message": "Hello World from MuleSoft!",
  "timestamp": "2025-10-30T15:45:00Z",
  "version": "v1"
}
```

#### 2. Personalized Hello
```bash
curl https://mulesoft-hello-world-dev.us-e2.cloudhub.io/api/v1/hello/John
```

**Expected Response:**
```json
{
  "message": "Hello, John!",
  "timestamp": "2025-10-30T15:45:00Z",
  "version": "v1"
}
```

#### 3. Custom Greeting (Multi-language)
```bash
curl -X POST https://mulesoft-hello-world-dev.us-e2.cloudhub.io/api/v1/greet \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Maria",
    "greeting": "Hola",
    "language": "es"
  }'
```

**Expected Response:**
```json
{
  "message": "Hola, Maria!",
  "language": "es",
  "timestamp": "2025-10-30T15:45:00Z",
  "version": "v1"
}
```

#### 4. Health Check
```bash
curl https://mulesoft-hello-world-dev.us-e2.cloudhub.io/api/v1/health
```

**Expected Response:**
```json
{
  "status": "UP",
  "timestamp": "2025-10-30T15:45:00Z",
  "version": "v1"
}
```

### 5. API Console

Access the interactive API documentation:
```
https://mulesoft-hello-world-dev.us-e2.cloudhub.io/console
```

---

## 📊 Key Features

### ✨ API-First Benefits

1. **Contract-First Development**
   - API specification defined before implementation
   - Ensures consistency across teams

2. **Auto-Validation**
   - APIkit automatically validates requests against RAML
   - Returns proper error responses for invalid requests

3. **Interactive Documentation**
   - API Console provides live testing interface
   - Automatically generated from RAML

4. **Exchange Integration**
   - API spec stored in Anypoint Exchange
   - Reusable across multiple projects
   - Version controlled

5. **Multi-Language Support**
   - Greeting endpoint supports English, Spanish, French
   - Easily extensible to more languages

---

## 🔍 API Endpoints Summary

| Method | Endpoint | Description | Request Body |
|--------|----------|-------------|--------------|
| GET | `/api/v1/hello` | Simple hello world | - |
| GET | `/api/v1/hello/{name}` | Personalized greeting | - |
| POST | `/api/v1/greet` | Custom multi-language greeting | `{"name", "greeting?", "language?"}` |
| GET | `/api/v1/health` | Health check | - |
| GET | `/console` | API Documentation Console | - |

---

## 📝 Configuration Details

### CloudHub 2.0 Settings
- **Environment**: Sandbox (Dev)
- **Region**: US-East-2
- **Runtime**: Mule 4.9.10
- **vCores**: 0.1
- **Replicas**: 1
- **Auto-scaling**: Disabled (can be enabled)

### Dependencies
- **HTTP Connector**: 1.9.3
- **APIkit**: 1.10.4
- **Sockets Connector**: 1.2.4
- **API Spec from Exchange**: hello-world-api:1.0.0

---

## 🎯 Next Steps

### Enhancement Ideas

1. **Add More Endpoints**
   - Update RAML with new endpoints
   - Republish to Exchange
   - Add implementations

2. **Add Authentication**
   - Update RAML with security schemes
   - Implement OAuth2 or Basic Auth

3. **Add Data Persistence**
   - Connect to database
   - Store greeting history

4. **Add Error Handling**
   - Custom error responses
   - Logging and monitoring

5. **CI/CD Pipeline**
   - GitHub Actions for auto-deployment
   - Automated testing

### Update API Specification

To update the API:

1. **Modify RAML**
   ```bash
   cd src/main/resources/api
   # Edit hello-world-api.raml
   ```

2. **Publish New Version**
   ```bash
   anypoint-cli exchange asset upload \
     --organization=d7699a02-16e5-43d5-ae1c-875f3a9196e0 \
     --name="Hello World API" \
     --apiVersion=v1 \
     --mainFile=hello-world-api.raml \
     --classifier=raml \
     d7699a02-16e5-43d5-ae1c-875f3a9196e0/hello-world-api/1.0.1 \
     hello-world-api.raml
   ```

3. **Update POM Dependency**
   ```xml
   <version>1.0.1</version>
   ```

4. **Rebuild and Redeploy**

---

## 📚 Resources

- **Exchange Asset**: https://anypoint.mulesoft.com/exchange/d7699a02-16e5-43d5-ae1c-875f3a9196e0/hello-world-api/
- **GitHub Repository**: https://github.com/raju-bvssn/mulesoft-hello-world
- **Dev Branch**: https://github.com/raju-bvssn/mulesoft-hello-world/tree/dev
- **APIkit Documentation**: https://docs.mulesoft.com/apikit/latest/
- **CloudHub 2.0 Docs**: https://docs.mulesoft.com/cloudhub-2/

---

## ✅ Checklist for Deployment

- [x] RAML API specification created
- [x] API published to Anypoint Exchange
- [x] APIkit implementation scaffolded
- [x] Application builds successfully
- [x] Code committed to dev branch
- [x] Code pushed to GitHub
- [ ] Deploy to CloudHub 2.0 (via Studio or Platform UI)
- [ ] Test all endpoints
- [ ] Verify API Console is accessible

---

**Ready to Deploy!** 🚀 Follow Option 1 or Option 2 above to deploy your API-first MuleSoft application to CloudHub 2.0.

