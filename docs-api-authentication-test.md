# API Authentication Test

## Objective
Verify that the Medusa Store API requires a publishable API key for access to the products endpoint.

## Test 1 — Request without API key
Endpoint: GET /store/products

Result: 400 Bad Request

The server rejected the request because the required x-publishable-api-key header was missing.

## Test 2 — Request with valid publishable API key
Endpoint: GET /store/products

Result: 200 OK

The server accepted the authenticated request and returned the products response.

## Security Finding
The /store/products endpoint enforces the required publishable API key.

This demonstrates that unauthenticated requests are rejected while requests containing the required API credential are accepted.
