# Getting Started with Hello World API

This guide will help you quickly integrate and start using the Hello World API in your applications.

## Prerequisites

- Basic understanding of REST APIs
- HTTP client (cURL, Postman, or any HTTP library)
- Internet connection to access CloudHub

## Base URL

All API endpoints are relative to the base URL:

```
https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io
```

## Authentication

This is a demo API and does not require authentication. For production use, implement appropriate security policies.

---

## Your First API Call

### Using cURL

```bash
curl https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello
```

### Expected Response

```json
{
  "message": "Hello World from MuleSoft!",
  "timestamp": "2025-10-30T16:16:37.485Z",
  "version": "v1"
}
```

### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| `message` | string | The greeting message |
| `timestamp` | datetime | ISO 8601 timestamp of when the message was generated |
| `version` | string | API version |

---

## Example 1: Personalized Greeting

Get a greeting with a specific name.

### Request

```bash
curl https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello/John
```

### Response

```json
{
  "message": "Hello, John!",
  "timestamp": "2025-10-30T16:16:37.804Z",
  "version": "v1"
}
```

---

## Example 2: Custom Multi-Language Greeting

Create a custom greeting in different languages.

### English Greeting

```bash
curl -X POST https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Maria",
    "language": "en"
  }'
```

**Response:**
```json
{
  "message": "Hello, Maria!",
  "language": "en",
  "timestamp": "2025-10-30T16:16:38.371Z",
  "version": "v1"
}
```

### Spanish Greeting

```bash
curl -X POST https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Maria",
    "greeting": "Hola",
    "language": "es"
  }'
```

**Response:**
```json
{
  "message": "Hola, Maria!",
  "language": "es",
  "timestamp": "2025-10-30T16:16:38.833Z",
  "version": "v1"
}
```

### French Greeting

```bash
curl -X POST https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Pierre",
    "greeting": "Bonjour",
    "language": "fr"
  }'
```

**Response:**
```json
{
  "message": "Bonjour, Pierre!",
  "language": "fr",
  "timestamp": "2025-10-30T16:16:39.278Z",
  "version": "v1"
}
```

---

## Example 3: Health Check

Monitor API availability with the health endpoint.

### Request

```bash
curl https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/health
```

### Response

```json
{
  "status": "UP",
  "timestamp": "2025-10-30T16:16:39.720Z",
  "version": "v1"
}
```

---

## Using Different Languages

### JavaScript (Fetch API)

```javascript
// Simple Hello
fetch('https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello')
  .then(response => response.json())
  .then(data => console.log(data.message));

// Custom Greeting
fetch('https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'Maria',
    language: 'es'
  })
})
  .then(response => response.json())
  .then(data => console.log(data.message));
```

### Python

```python
import requests

# Simple Hello
response = requests.get('https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello')
print(response.json()['message'])

# Custom Greeting
payload = {
    "name": "Maria",
    "language": "es"
}
response = requests.post(
    'https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet',
    json=payload
)
print(response.json()['message'])
```

### Java

```java
import java.net.http.*;
import java.net.URI;

public class HelloWorldClient {
    public static void main(String[] args) throws Exception {
        HttpClient client = HttpClient.newHttpClient();
        
        // Simple Hello
        HttpRequest request = HttpRequest.newBuilder()
            .uri(URI.create("https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello"))
            .GET()
            .build();
        
        HttpResponse<String> response = client.send(request, 
            HttpResponse.BodyHandlers.ofString());
        
        System.out.println(response.body());
    }
}
```

### Node.js (Axios)

```javascript
const axios = require('axios');

const baseURL = 'https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io';

// Simple Hello
axios.get(`${baseURL}/api/hello`)
  .then(response => console.log(response.data.message))
  .catch(error => console.error(error));

// Custom Greeting
axios.post(`${baseURL}/api/greet`, {
  name: 'Maria',
  language: 'es'
})
  .then(response => console.log(response.data.message))
  .catch(error => console.error(error));
```

---

## Interactive API Console

For interactive testing and exploration, use the built-in API Console:

**URL**: https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/console

The console provides:
- Interactive request builder
- Request/response examples
- Schema validation
- Try-it functionality

---

## Error Handling

The API returns standard HTTP status codes:

| Status Code | Description |
|-------------|-------------|
| 200 | Success |
| 400 | Bad Request - Invalid input |
| 404 | Not Found - Endpoint doesn't exist |
| 405 | Method Not Allowed |
| 500 | Internal Server Error |

### Error Response Format

```json
{
  "error": "Bad Request",
  "message": "Name is required",
  "timestamp": "2025-10-30T16:16:40Z"
}
```

---

## Rate Limits

Standard CloudHub rate limits apply. For production use with higher throughput requirements, contact your MuleSoft account representative.

---

## Next Steps

1. **Explore the RAML specification** - See the complete API contract
2. **Try the API Console** - Interactive testing interface
3. **Check API Reference** - Detailed endpoint documentation
4. **Review Use Cases** - Common integration patterns

---

## Support

Need help? Check out:
- [API Reference](./api-reference.md) - Detailed endpoint documentation
- [Use Cases](./use-cases.md) - Common implementation patterns
- [GitHub Repository](https://github.com/raju-bvssn/mulesoft-hello-world) - Source code and examples

---

**Happy coding! 🚀**

