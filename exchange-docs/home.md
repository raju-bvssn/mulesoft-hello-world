# Hello World API

A simple, production-ready REST API demonstrating best practices for MuleSoft API-first development with multi-language greeting capabilities.

## Overview

The Hello World API provides greeting endpoints with support for personalized messages in multiple languages (English, Spanish, and French). Built using API-first methodology with RAML 1.0 and APIkit, this API demonstrates proper REST API design patterns and can serve as a reference implementation for new MuleSoft projects.

## Key Features

- ✅ **API-First Design** - RAML specification drives implementation
- ✅ **Multi-language Support** - Greetings in English, Spanish, and French
- ✅ **APIkit Validation** - Automatic request/response validation
- ✅ **Interactive Console** - Built-in API testing interface
- ✅ **Health Monitoring** - Operational health check endpoint
- ✅ **CloudHub 2.0 Ready** - Optimized for cloud deployment

## Quick Start

### Base URL
```
https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io
```

### Try It Now

Simple greeting:
```bash
curl https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/api/hello
```

Response:
```json
{
  "message": "Hello World from MuleSoft!",
  "timestamp": "2025-10-30T16:16:37Z",
  "version": "v1"
}
```

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/hello` | Returns a simple hello world message |
| GET | `/api/hello/{name}` | Returns a personalized greeting |
| POST | `/api/greet` | Creates a custom greeting with language support |
| GET | `/api/health` | Returns API health status |
| GET | `/console` | Interactive API documentation console |

## Use Cases

### 1. Microservices Health Checks
Use the `/api/health` endpoint to monitor service availability in your microservices architecture.

### 2. Personalized User Greetings
Integrate the personalized greeting endpoint to welcome users in your application.

### 3. Internationalization (i18n)
Leverage multi-language support for global applications requiring localized greetings.

### 4. API Learning & Training
Perfect starter project for learning MuleSoft API development best practices.

## Technical Details

- **API Version**: v1
- **Protocol**: HTTPS
- **Format**: JSON
- **Authentication**: None (demo API)
- **Rate Limiting**: Standard CloudHub limits apply

## Getting Started

See the **Getting Started** section in the navigation for detailed implementation examples and code samples.

## Support

For issues, questions, or feature requests:
- GitHub: [mulesoft-hello-world](https://github.com/raju-bvssn/mulesoft-hello-world)
- Exchange: [Hello World API](https://anypoint.mulesoft.com/exchange/)

## Version History

### Version 1.0.0 (Current)
- Initial release
- Multi-language greeting support (EN, ES, FR)
- Health monitoring endpoint
- Interactive API Console
- CloudHub 2.0 deployment ready

---

**Built with ❤️ using MuleSoft API-First Development**

