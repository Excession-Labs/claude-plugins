---
name: api-security-review
description: Review a REST, GraphQL or JSON API against the OWASP API Security Top 10 2023, with proof for each finding. Use when asked to review, audit or test an API, endpoints, an OpenAPI spec, API keys or rate limits, or to check for BOLA, mass assignment or API abuse.
---

# API security review

## When to use

You own the API or have written authorization to test it. Start from the OpenAPI spec, a proxy capture or the route list.

## Checklist, highest impact first

OWASP API Security Top 10, 2023 edition.

1. **API1 Broken Object Level Authorization.** Swap object IDs between two accounts. Evidence: account B reads account A's record. See `access-control-review`.
2. **API5 Broken Function Level Authorization.** Call admin or write methods as a normal user. Evidence: request, response, and a read-back of the change.
3. **API2 Broken Authentication.** Missing, weak or unsigned tokens; no lockout. Evidence: the request that succeeds with a bad or absent token.
4. **API3 Broken Object Property Level Authorization.** Add fields such as `role` or `owner_id` to a write; look for extra fields in responses. Evidence: the response before and after, read back.
5. **API7 SSRF.** Give URL parameters a callback address you control. Evidence: the callback log line.
6. **API8 Security Misconfiguration.** CORS, verbose errors, debug routes, missing headers. Evidence: the response headers or body.
7. **API9 Improper Inventory Management.** Old versions, undocumented routes, test hosts. Evidence: the URL, and how it differs from the spec.
8. **API4 Unrestricted Resource Consumption and API6 Sensitive Business Flows.** Check pagination caps, rate limits and automation of flows such as sign-up. Use small bounded bursts only. Evidence: the status codes and counts.
9. **API10 Unsafe Consumption of APIs.** Check how the API trusts data from third-party APIs. Evidence: the code or a test input and its effect.

## Safe testing

- Own systems or written authorization only.
- Test-owned accounts and records, labeled as test data.
- No destructive calls on real data. Keep rate tests short and bounded.
- Write with a test record, then read it back through another session.

## Evidence rule

A `200` or `{"success":true}` is not proof. Show the changed record read back, or the data returned to the wrong account.

## Report

Per finding: finding, evidence (request and response), impact, fix. Then "Controls that held": the cases that returned `403`, `404` or a validation error, with the response, so fixes do not remove them.

## Guide

OWASP API Security Top 10 with test ideas: https://penetrationtestservices.com/guides/owasp-api-security-top-10/
