# Exchange Documentation

This directory contains comprehensive documentation for the Hello World API that can be published to Anypoint Exchange.

## Documentation Structure

| File | Description |
|------|-------------|
| `home.md` | Main landing page with overview and quick start |
| `getting-started.md` | Step-by-step guide for first-time users |
| `api-reference.md` | Complete API endpoint documentation |
| `use-cases.md` | Real-world integration patterns and examples |

## Publishing to Exchange

### Option 1: Manual Upload via Anypoint Platform

1. Log in to [Anypoint Platform](https://anypoint.mulesoft.com)
2. Navigate to **Exchange**
3. Find your **Hello World API** asset
4. Click **Edit** (pencil icon)
5. Go to **Documentation** section
6. Upload markdown files:
   - Upload `home.md` as the main page
   - Add `getting-started.md`, `api-reference.md`, and `use-cases.md` as additional pages
7. Click **Publish** to make documentation live

### Option 2: Using Exchange Maven Plugin

The documentation can be included during asset publication by placing these files in the `src/main/resources/api/exchange-docs/` directory when publishing via Maven.

## Documentation Guidelines

### Format
- All documentation is in **Markdown** format
- Supports standard Markdown syntax
- Code blocks use triple backticks with language identifiers
- Tables for structured data presentation

### Content Guidelines
1. **Keep it current** - Update examples with actual endpoint URLs
2. **Be comprehensive** - Include request/response examples
3. **Show real code** - Provide working code snippets
4. **Version properly** - Document changes in version history

## Maintenance

When updating the API:
1. Update the RAML specification first
2. Update code examples in documentation
3. Add new sections for new features
4. Update version history
5. Republish to Exchange

## Local Preview

To preview Markdown files locally:

```bash
# Using VSCode
code exchange-docs/home.md

# Using Markdown Preview
grip exchange-docs/home.md
```

## Quick Links

- **API Endpoint**: https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io
- **API Console**: https://mule-hello-world-2aofst.rajrd4-2.usa-e1.cloudhub.io/console
- **GitHub Repo**: https://github.com/raju-bvssn/mulesoft-hello-world
- **Anypoint Exchange**: [View in Exchange](https://anypoint.mulesoft.com/exchange/)

## File Descriptions

### home.md
The main landing page that visitors see first. Contains:
- API overview and key features
- Quick start example
- Endpoint summary table
- Use cases overview
- Version history

### getting-started.md
Detailed guide for new users:
- Prerequisites
- First API call walkthrough
- Code examples in multiple languages (cURL, JavaScript, Python, Java)
- Interactive console usage
- Error handling basics

### api-reference.md
Complete technical reference:
- All endpoints documented
- Request/response schemas
- Parameters and constraints
- HTTP status codes
- Data type specifications
- RAML specification reference

### use-cases.md
Real-world integration examples:
- Microservices health monitoring
- Multi-language user welcome
- API Gateway integration
- Event-driven greetings
- Chatbot integration
- Mobile app integration
- Email personalization
- Learning & training scenarios

## Contributing

When adding new documentation:
1. Follow existing structure and style
2. Include working code examples
3. Test all code snippets
4. Update this README if adding new files
5. Commit with descriptive messages

## Notes

- All code examples use the actual CloudHub 2.0 endpoint
- Documentation is designed for Anypoint Exchange display
- Images can be added to a separate `/images` subdirectory if needed
- Custom CSS styling is handled by Exchange platform

---

**Last Updated**: October 30, 2025  
**Documentation Version**: 1.0.0

