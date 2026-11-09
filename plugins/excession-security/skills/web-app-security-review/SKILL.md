---
name: web-app-security-review
description: Review a web application for the OWASP Top 10 2025 risks, in impact order, with proof for each finding. Use when asked to security review, audit, pentest-check or test a web app, site or login flow, or to check a web app against the OWASP Top 10.
---

# Web app security review

## When to use

You own the app or have written authorization to test it, and you want a ranked list of proven issues. For one area, use the narrower skills: `access-control-review`, `auth-session-review`, `api-security-review`.

## Checklist, highest impact first

Names follow OWASP Top 10 2025.

1. **Broken access control (A01).** Request another user's record, an admin route and a write you should not have. Evidence: the request, the response, and a second-account read-back.
2. **Injection (A05).** Send a harmless probe (a quote, `{{7*7}}`, a unique marker) to each input. Evidence: the response or rendered page that shows the probe changed behavior. Never extract real data.
3. **Authentication failures (A07).** Check login, reset and MFA. Evidence: the request that bypasses or weakens the step.
4. **Security misconfiguration (A02).** Debug pages, default credentials, open storage, verbose errors, missing headers. Evidence: the URL and response, or the config line.
5. **Cryptographic failures (A04).** Plain HTTP, weak cookie flags, tokens in URLs. Evidence: the header or captured request.
6. **Insecure design (A06).** Business-logic gaps such as skipped steps or reused coupons. Evidence: the step sequence and responses.
7. **Software supply chain (A03) and integrity (A08).** Unpinned scripts without integrity attributes, outdated components with a known CVE. Evidence: the file, the version and the advisory.
8. **Logging (A09) and exception handling (A10).** Trigger an error and a failed login, then check what is logged and what the user sees. Evidence: the response body and the log entry.

## Safe testing

- Test only systems you own or may test in writing.
- Use test-owned accounts and records, labeled as test data.
- No destructive actions, no denial of service, no real user data. Bound any upload, password guess or email-sending test and state its cost.
- Stop and report if you reach data that is not yours.

## Evidence rule

A bare "200 OK" or "success" proves nothing. Show the request, the response, and for writes a read-back through another session.

## Report

For each finding give: finding, evidence, impact, fix. Label each as observed or inferred. Then add a "Controls that held" list: what you tried, the control that blocked it, and the response that shows it. This lets the team keep those protections when they fix the rest.

## Guide

OWASP Top 10 2025 explained with tests: https://penetrationtestservices.com/guides/owasp-top-10-2025/
