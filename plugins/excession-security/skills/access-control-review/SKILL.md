---
name: access-control-review
description: Test access control, IDOR, BOLA and tenant isolation with the two-account read-back method. Use when asked to check IDOR, BOLA, broken access control, privilege escalation, role checks, multi-tenant isolation or whether one customer can see another's data.
---

# Access control review

## When to use

You need proof that one user or tenant cannot read or change another's data. You own the system or have written authorization.

## Method: two accounts and a read-back

1. Create account A and account B in different tenants, plus an admin and a plain user if roles exist. Mark all as test data.
2. As A, create records and note their IDs.
3. As B, request A's IDs: read, update, delete, list, export and file download.
4. Read back as A. A change proves a write. A response alone does not.
5. Repeat with anonymous requests and with B's token on A's routes.

## Checklist, highest impact first

1. **Cross-tenant read.** B fetches A's object by ID. Evidence: A's data in B's response.
2. **Cross-tenant write or delete.** B edits A's object. Evidence: A reads back the changed record.
3. **Function-level gaps.** A plain user calls admin routes. Evidence: request, response, read-back.
4. **Role fields in writes.** Add `role`, `tenant_id` or `owner_id` to a request. Evidence: the stored value, read back.
5. **List and search leaks.** Filters, exports, autocomplete, webhooks and background jobs that skip the tenant check. Evidence: foreign rows in the result.
6. **Indirect references.** Files, signed URLs, GraphQL nodes, websocket channels. Evidence: foreign object returned.

## Controls to run beside each test

Use a nonexistent ID, a wrong record type, another account's token and an anonymous call. Expected controls return `403` or `404` with no data.

## Safe testing

- Own systems or written authorization only.
- Test-owned accounts and records only. Never enumerate real users' IDs.
- Change only your own test records. Delete nothing that is not yours.

## Report

Finding, evidence (both accounts' requests, responses and the read-back), impact (what data or action crosses the boundary), fix (a server-side ownership check on every object access). Add "Controls that held" with each blocked case and its response.

## Guides

IDOR testing: https://penetrationtestservices.com/guides/idor-testing/. Tenant isolation: https://penetrationtestservices.com/guides/multi-tenant-isolation-testing/
