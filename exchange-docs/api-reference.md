# API Reference

Complete reference documentation for all Hello World API endpoints.

## Base URL

```
https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io
```

---

## Endpoints

### 1. Get Simple Hello

Returns a simple hello world greeting message.

**Endpoint**: `GET /api/hello`

#### Request

No parameters required.

#### Response

**Status**: `200 OK`

**Content-Type**: `application/json`

**Body**:
```json
{
  "message": "Hello World from MuleSoft!",
  "timestamp": "2025-10-30T16:16:37.485921511Z",
  "version": "v1"
}
```

#### Response Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `message` | string | Yes | The greeting message |
| `timestamp` | datetime | Yes | ISO 8601 timestamp |
| `version` | string | Yes | API version identifier |

#### Example

```bash
curl -X GET https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello
```

---

### 2. Get Personalized Hello

Returns a personalized greeting for the specified name.

**Endpoint**: `GET /api/hello/{name}`

#### Request

**URL Parameters**:

| Parameter | Type | Required | Description | Constraints |
|-----------|------|----------|-------------|-------------|
| `name` | string | Yes | Name to greet | 1-50 characters |

#### Response

**Status**: `200 OK`

**Content-Type**: `application/json`

**Body**:
```json
{
  "message": "Hello, {name}!",
  "timestamp": "2025-10-30T16:16:37.804515965Z",
  "version": "v1"
}
```

#### Response Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `message` | string | Yes | Personalized greeting message |
| `timestamp` | datetime | Yes | ISO 8601 timestamp |
| `version` | string | Yes | API version identifier |

#### Examples

**Request with simple name**:
```bash
curl -X GET https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello/John
```

**Request with encoded name (spaces)**:
```bash
curl -X GET https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello/John%20Doe
```

---

### 3. Create Custom Greeting

Creates a custom greeting with optional language and greeting style.

**Endpoint**: `POST /api/greet`

#### Request

**Content-Type**: `application/json`

**Body Schema**:

| Field | Type | Required | Description | Constraints |
|-------|------|----------|-------------|-------------|
| `name` | string | Yes | Name of person to greet | Non-empty string |
| `greeting` | string | No | Custom greeting word | Default varies by language |
| `language` | string | No | Language code | `en`, `es`, or `fr`. Default: `en` |

**Request Body**:
```json
{
  "name": "Maria",
  "greeting": "Hola",
  "language": "es"
}
```

#### Response

**Status**: `200 OK`

**Content-Type**: `application/json`

**Body**:
```json
{
  "message": "Hola, Maria!",
  "language": "es",
  "timestamp": "2025-10-30T16:16:38.833063198Z",
  "version": "v1"
}
```

#### Response Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `message` | string | Yes | Custom greeting message |
| `language` | string | Yes | Language code used |
| `timestamp` | datetime | Yes | ISO 8601 timestamp |
| `version` | string | Yes | API version identifier |

#### Language Support

| Language | Code | Default Greeting |
|----------|------|------------------|
| English | `en` | Hello |
| Spanish | `es` | Hola |
| French | `fr` | Bonjour |

#### Examples

**English greeting (default)**:
```bash
curl -X POST https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet \
  -H "Content-Type: application/json" \
  -d '{
    "name": "John",
    "language": "en"
  }'
```

**Spanish greeting with custom text**:
```bash
curl -X POST https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Maria",
    "greeting": "Buenos días",
    "language": "es"
  }'
```

**French greeting**:
```bash
curl -X POST https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/greet \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Pierre",
    "language": "fr"
  }'
```

#### Error Responses

**Missing required field**:

**Status**: `400 Bad Request`

```json
{
  "error": "Bad Request",
  "message": "Name is required",
  "timestamp": "2025-10-30T16:16:40Z"
}
```

---

### 4. Health Check

Returns the operational status of the API.

**Endpoint**: `GET /api/health`

#### Request

No parameters required.

#### Response

**Status**: `200 OK`

**Content-Type**: `application/json`

**Body**:
```json
{
  "status": "UP",
  "timestamp": "2025-10-30T16:16:39.720927288Z",
  "version": "v1"
}
```

#### Response Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `status` | string | Yes | Health status (`UP` or `DOWN`) |
| `timestamp` | datetime | Yes | ISO 8601 timestamp |
| `version` | string | Yes | API version identifier |

#### Example

```bash
curl -X GET https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/health
```

#### Use Cases

- Kubernetes liveness/readiness probes
- Load balancer health checks
- Monitoring system integration
- Service mesh configuration

---

### 5. API Console

Interactive API documentation and testing interface.

**Endpoint**: `GET /console`

#### Access

Open in browser:
```
https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/console
```

#### Features

- Interactive request builder
- Live API testing
- Request/response examples
- Schema documentation
- RAML specification viewer

---

## HTTP Status Codes

The API uses standard HTTP status codes:

| Status Code | Meaning | Description |
|-------------|---------|-------------|
| 200 | OK | Request successful |
| 400 | Bad Request | Invalid request format or missing required fields |
| 404 | Not Found | Endpoint or resource not found |
| 405 | Method Not Allowed | HTTP method not supported for this endpoint |
| 406 | Not Acceptable | Requested content type not supported |
| 500 | Internal Server Error | Server-side error occurred |

---

## Response Headers

All responses include these headers:

| Header | Description |
|--------|-------------|
| `Content-Type` | Always `application/json` for API endpoints |
| `Date` | Server timestamp |
| `x-correlation-id` | Unique request identifier for tracking |

---

## Data Types

### DateTime Format

All timestamps follow ISO 8601 format:

```
2025-10-30T16:16:37.485921511Z
```

- Format: `YYYY-MM-DDTHH:mm:ss.SSSSSSSSSZ`
- Timezone: UTC (indicated by `Z`)
- Precision: Nanoseconds

### String Constraints

- **Name fields**: 1-50 characters
- **Encoding**: UTF-8
- **Special characters**: Supported

---

## RAML Specification

The complete API specification is available in RAML 1.0 format:

**Asset**: `hello-world-api:1.0.0`  
**Format**: RAML 1.0  
**Location**: Anypoint Exchange

---

## SDK & Client Libraries

### MuleSoft Connector

This API can be consumed using the HTTP Request connector in any Mule application.

**Example Configuration**:
```xml
<http:request-config name="Hello_World_API">
    <http:request-connection 
        host="mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io" 
        port="443"
        protocol="HTTPS" />
</http:request-config>
```

---

## Rate Limiting

Standard CloudHub 2.0 rate limits apply:
- **Default**: No explicit rate limits
- **Production**: Contact MuleSoft for custom limits

---

## Versioning

**Current Version**: v1

The API version is included in all responses via the `version` field. Future versions will be indicated through URL versioning (e.g., `/api/v2/hello`).

---

## Support

For technical support:
- **GitHub Issues**: [Report issues](https://github.com/raju-bvssn/mulesoft-hello-world/issues)
- **API Console**: Test and debug in real-time
- **Exchange**: View RAML specification

---

## Changelog

### Version 1.0.0 (Current)
- Initial release
- Four endpoints: hello, personalized hello, custom greet, health
- Multi-language support (EN, ES, FR)
- Interactive API Console
- CloudHub 2.0 deployment

---

**Last Updated**: October 30, 2025

