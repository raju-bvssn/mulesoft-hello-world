# Postman Collection Guide

## 📦 Files Included

1. **MuleSoft-Hello-World.postman_collection.json** - Main API collection
2. **MuleSoft-Hello-World.postman_environment.json** - Environment variables (local)

---

## 🚀 Quick Start

### Step 1: Import Collection

1. Open **Postman**
2. Click **Import** (top left)
3. Select **files** and import:
   - `MuleSoft-Hello-World.postman_collection.json`
   - `MuleSoft-Hello-World.postman_environment.json`
4. Click **Import**

### Step 2: Select Environment

1. In Postman, find the environment dropdown (top right)
2. Select: **MuleSoft Hello World - Local**

### Step 3: Ensure Application is Running

Make sure your MuleSoft application is running in Anypoint Studio:
```bash
# Test with curl to verify:
curl http://localhost:8081/hello
```

### Step 4: Run Requests

Click on any request in the collection and hit **Send**!

---

## 📋 Collection Structure

### 1. Health Check
Simple requests to verify the application is running:

#### **Get Hello World**
- **Method**: GET
- **Endpoint**: `/hello`
- **Expected Response**: `Hello World from MuleSoft!`
- **Status Code**: 200
- **Includes Tests**:
  - ✅ Status code is 200
  - ✅ Response time < 200ms
  - ✅ Content-Type is text/plain
  - ✅ Response body matches expected text
  - ✅ Response is not empty

#### **Get Hello World (Browser)**
- Simulates browser request with browser headers
- Useful for testing different client types

### 2. Load Testing

#### **Multiple Requests (Sequential)**
- Designed for use with Postman Collection Runner
- Tracks response times and calculates statistics
- See "Running Load Tests" section below

### 3. Error Scenarios

#### **Invalid Endpoint (404)**
- Tests 404 error handling
- Ensures proper error responses

#### **Wrong HTTP Method (405)**
- Tests that only GET is supported
- Verifies method validation

---

## 🧪 Running Tests

### Single Request Test

1. Select a request (e.g., "Get Hello World")
2. Click **Send**
3. View the **Test Results** tab (bottom)
4. All tests should **PASS** ✅

### Collection Runner (Load Test)

1. Right-click on the collection → **Run collection**
2. Configure:
   - **Iterations**: 100 (or any number)
   - **Delay**: 0 ms (for max speed) or add delay
   - **Data**: None needed
3. Click **Run MuleSoft Hello World API**
4. View results:
   - Pass/Fail for each iteration
   - Response times graph
   - Statistics

**Example Configuration**:
```
Iterations: 100
Delay: 0 ms
Keep variable values: Checked
```

**Expected Results**:
- ✅ 100% success rate
- ✅ Consistent response times
- ✅ All tests passing

---

## 🔧 Environment Variables

The environment file includes:

| Variable | Value | Description |
|----------|-------|-------------|
| `baseUrl` | `http://localhost:8081` | Base URL for the API |
| `environment` | `local` | Environment name |
| `applicationName` | `mulesoft-hello-world` | Application name |
| `version` | `1.0.0-SNAPSHOT` | Application version |

### Modifying Variables

You can modify these in Postman:
1. Click the **eye icon** (top right)
2. Select your environment
3. Edit values as needed

**For different environments**:
- **Local**: `http://localhost:8081`
- **CloudHub**: `http://your-app.cloudhub.io`
- **Custom**: `http://your-server:port`

---

## 📊 Test Scripts Included

### Pre-request Scripts
- Logs request details (URL, method, timestamp)
- Sets collection-level variables
- Tracks request count

### Test Scripts
- Validates status codes
- Checks response content
- Measures response times
- Tracks success rate
- Calculates statistics

### Collection-level Scripts
- Runs before/after every request
- Tracks total requests
- Counts successful requests
- Monitors overall collection health

---

## 💡 Advanced Usage

### Monitoring Response Times

The collection automatically tracks response times. After running multiple requests:

1. Open **Postman Console** (View → Show Postman Console)
2. See detailed logs including:
   - Request details
   - Response times
   - Test results
   - Statistics (after 10 requests)

### Automated Testing

Add the collection to **Postman Monitors**:

1. Click collection → **Monitors**
2. Create monitor
3. Set schedule (e.g., every hour)
4. Get alerts if tests fail

### CI/CD Integration

Run collection via Newman (Postman CLI):

```bash
# Install Newman
npm install -g newman

# Run collection
newman run MuleSoft-Hello-World.postman_collection.json \
  -e MuleSoft-Hello-World.postman_environment.json \
  -n 100 \
  --reporters cli,json

# With HTML reporter
newman run MuleSoft-Hello-World.postman_collection.json \
  -e MuleSoft-Hello-World.postman_environment.json \
  --reporters cli,htmlextra \
  --reporter-htmlextra-export report.html
```

---

## 📈 Performance Benchmarks

Based on load testing (using Apache Bench):

| Metric | Value |
|--------|-------|
| Peak Throughput | 7,102 req/sec |
| Median Response Time | 9 ms |
| 95th Percentile | 127 ms |
| Success Rate | 100% (36,100 requests) |

**Your Postman tests should show similar results!**

---

## 🐛 Troubleshooting

### Connection Refused
**Error**: `Could not send request`
**Solution**: Ensure MuleSoft app is running in Anypoint Studio

```bash
# Test manually:
curl http://localhost:8081/hello
```

### Tests Failing
**Error**: Test assertions fail
**Solution**: 
- Check response in **Body** tab
- View **Test Results** for specific failure
- Ensure application is fully deployed

### Wrong Port
**Error**: Connection to port 8081 fails
**Solution**: 
- Check if app is using different port
- Update `baseUrl` in environment to correct port

### Timeout
**Error**: Request timeout
**Solution**:
- Increase timeout in Postman settings
- Check application logs in Anypoint Studio
- Verify system resources

---

## 📚 Additional Resources

- **Postman Documentation**: https://learning.postman.com/
- **Newman CLI**: https://www.npmjs.com/package/newman
- **MuleSoft Docs**: https://docs.mulesoft.com
- **Load Test Results**: See `LOAD-TEST-RESULTS.md`

---

## ✅ Quick Validation Checklist

Before using the collection:

- [ ] MuleSoft app is running in Anypoint Studio
- [ ] Application shows "DEPLOYED" status
- [ ] Manual curl test succeeds: `curl http://localhost:8081/hello`
- [ ] Postman collection imported
- [ ] Postman environment imported and selected
- [ ] Environment dropdown shows "MuleSoft Hello World - Local"

**You're ready to test!** 🚀

---

## 📝 Example Test Run Output

```
✅ Status code is 200
✅ Response time is less than 200ms
✅ Content-Type is text/plain
✅ Response body contains 'Hello World from MuleSoft!'
✅ Response body is not empty

Response Time: 28ms
Response Body: Hello World from MuleSoft!

5 / 5 tests passed
```

---

**Collection Version**: 1.0.0  
**Last Updated**: October 30, 2025  
**Status**: Production Ready ✅

