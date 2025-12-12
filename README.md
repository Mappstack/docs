# Stackure Developer Documentation

This repository contains developer documentation for integrating applications with Stackure for authentication and access control.

These guides are intended for developers and technical administrators building or maintaining applications that authenticate users and enforce access through Stackure.

## Start here

If you are integrating an application with Stackure, start with:

- [App Integration Guide](app-integration/getting-started.md)

This guide walks you through registering your app, assigning access, and selecting an integration approach.

## Integration paths

After registering your app, choose one of the following integration paths:

- [SDK Integration (JavaScript / TypeScript)](app-integration/sdk-js.md)  
  The recommended approach for adding passwordless authentication, session validation, and role-based access using the Stackure SDK.

- [Custom API Integration](app-integration/api-integration.md)  
  Direct REST API integration for full control over authentication and authorization flows.

## Scope

These documents focus exclusively on application integration.

They do not cover:
- end-user behavior or UI flows
- Stackure console usage
- administrative or headless automation workflows

The complete OpenAPI specification is available in the [`api`](api/) directory.

---