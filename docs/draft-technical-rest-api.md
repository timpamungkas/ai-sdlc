# Draft Technical Standard - REST API

## Document Information
- Feature Name: REST API Technical Standard
- Author: copilot
- Date:
- Version:

## Table of Contents
- [Background](#background)
- [In Scope](#in-scope)
- [Constraints](#constraints)
- [Technical Standard](#technical-standard)
  - [Resource Naming Convention](#resource-naming-convention)
  - [Versioning](#versioning)
  - [HTTP Methods](#http-methods)
  - [Request & Response Format](#request--response-format)
  - [Query Parameters](#query-parameters)
  - [Pagination](#pagination)
  - [HTTP Status Codes](#http-status-codes)
  - [Error Response Format](#error-response-format)
  - [Idempotency](#idempotency)
  - [Security](#security)
- [Non-Functional Requirements](#non-functional-requirements)
- [AI usage disclaimer](#ai-usage-disclaimer)

## Background
As the company expands from car sales into car rental, multiple new features (e.g., Vehicle Onboarding, Reservation Allocation, Maintenance Scheduling, Car Management) are being designed with REST APIs. This document proposes a common technical standard so that every REST API produced across features is consistent, predictable, and easy to integrate with, regardless of which team or feature builds it.

## In Scope
- Naming conventions for URLs, path parameters, query parameters, and JSON fields.
- API versioning strategy.
- Standard usage of HTTP methods and HTTP status codes.
- Standard request/response body structure, including pagination and error format.
- General security requirements applicable to all REST APIs.

## Constraints
- This document is a cross-feature standard only. Feature-specific endpoints, request/response fields, and business validation rules remain defined in each feature's own TRD.
- This document does not cover non-REST protocols (e.g., GraphQL, gRPC, WebSocket); those remain case-by-case decisions justified in the relevant TRD.
- Infrastructure concerns (API gateway, load balancing, rate limiting implementation) are out of scope.

## Technical Standard

### Resource Naming Convention
- URLs must use nouns that represent resources (collections), not verbs/actions. Example: `/vehicles`, not `/getVehicles`.
- Collection names must be plural and lower kebab-case for multi-word resources. Example: `/maintenance-schedules`.
- Nested resources represent ownership/hierarchy. Example: `/vehicles/{vehicleId}/maintenance-records`.
- Path parameters use camelCase and are wrapped in curly braces in documentation. Example: `{vehicleId}`.
- Query parameters use camelCase. Example: `?locationId=...&sortBy=createdAt`.
- JSON field names (in request and response bodies) use camelCase. Example: `licensePlate`, `homeLocationId`.
- Avoid abbreviations unless they are widely understood (e.g., `id`, `vin`).

### Versioning
- The API version must be included in the URL path as the first segment: `/api/v{n}/...`. Example: `/api/v1/vehicles`.
- Version numbers are integers, incremented only on breaking changes (e.g., removing/renaming a field, changing a field's type, changing behavior of an existing endpoint).
- Additive, backward-compatible changes (e.g., adding an optional field) do not require a new version.
- At least one prior major version must remain available for a defined deprecation period after a new version is released, to be agreed upon per feature.

### HTTP Methods
| Method | Usage |
| --- | --- |
| GET | Retrieve a resource or collection. Must not have side effects. |
| POST | Create a new resource, or trigger an action that does not map to a single resource state change. |
| PUT | Replace a resource entirely. All fields must be provided. |
| PATCH | Partially update a resource. Only provided fields are changed. |
| DELETE | Remove a resource. |

- Lifecycle/status transitions (e.g., changing a vehicle's status) should be modeled as a `PATCH` or a sub-resource action endpoint (e.g., `POST /vehicles/{vehicleId}/status-transitions`), decided per feature and documented in its TRD.

### Request & Response Format
- All request and response bodies must use `Content-Type: application/json`, encoded as UTF-8.
- Dates and timestamps must use ISO 8601 format (e.g., `2025-01-31` for dates, `2025-01-31T10:00:00Z` for timestamps).
- Monetary values must be represented as decimal numbers (not floating point) with an explicit currency field when applicable.
- A single resource response body must return the resource object directly (no unnecessary wrapper) unless the endpoint returns a collection, in which case the [Pagination](#pagination) envelope applies.

### Query Parameters
- Filtering: `?field=value` for exact-match filters (e.g., `?status=Active`).
- Sorting: `?sortBy=fieldName&sortDirection=asc|desc`.
- Field selection (optional, if supported): `?fields=field1,field2`.

### Pagination
- Collection endpoints must support pagination using `page` and `pageSize` query parameters.
- Collection responses must use the following envelope:
  | Field | Type | Notes |
  | --- | --- | --- |
  | data | array | The list of resources for the current page. |
  | page | integer | Current page number (1-based). |
  | pageSize | integer | Number of items per page. |
  | totalItems | integer | Total number of items across all pages. |
  | totalPages | integer | Total number of pages. |

### HTTP Status Codes
| Status Code | Usage |
| --- | --- |
| 200 OK | Successful `GET`, `PUT`, `PATCH`, or `DELETE` returning a body. |
| 201 Created | Successful `POST` that creates a resource. Must include the created resource in the response body. |
| 204 No Content | Successful request with no response body (e.g., `DELETE`). |
| 400 Bad Request | Request is malformed or fails validation. |
| 401 Unauthorized | Missing or invalid authentication credentials. |
| 403 Forbidden | Authenticated but not authorized to perform the action. |
| 404 Not Found | Resource does not exist. |
| 409 Conflict | Request conflicts with the current state of the resource (e.g., duplicate unique field, invalid status transition). |
| 422 Unprocessable Entity | Request is syntactically correct but fails business rule validation. |
| 429 Too Many Requests | Client exceeded rate limit. |
| 500 Internal Server Error | Unexpected server-side failure. |

### Error Response Format
All error responses (4xx/5xx) must use a consistent structure:
| Field | Type | Notes |
| --- | --- | --- |
| errorCode | string | Machine-readable error identifier, e.g., `VALIDATION_ERROR`. |
| message | string | Human-readable summary of the error. |
| details | array (optional) | List of field-level errors, each with `field` and `message`. |
| traceId | string | Correlation identifier for tracing the request across logs. |

### Idempotency
- `GET`, `PUT`, `DELETE` must be idempotent: repeating the same request produces the same result without unintended side effects.
- `POST` endpoints that create resources should support an optional `Idempotency-Key` request header so that clients can safely retry without creating duplicate resources.

### Security
- All REST APIs must be served over HTTPS only.
- All endpoints (except explicitly public ones) must require authentication via a bearer token (e.g., JWT) in the `Authorization` header.
- Authorization checks must be enforced per endpoint based on the authenticated user's role/permissions.
- Input must be validated and sanitized on the server side regardless of client-side validation, to prevent injection attacks.
- Sensitive fields (e.g., passwords, tokens) must never appear in logs or error responses.

## Non-Functional Requirements
Blank

## AI usage disclaimer
*This document was generated with the assistance of artificial intelligence and should be reviewed by a human for accuracy and completeness.*
