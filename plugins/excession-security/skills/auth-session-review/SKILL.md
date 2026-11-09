---
name: auth-session-review
description: Review login, password reset, MFA, OAuth, JWT and session handling for weaknesses, with proof for each. Use when asked to check authentication, sessions, cookies, tokens, password reset, 2FA, OAuth or JWT security, or login brute-force protection.
---

# Auth and session review

## When to use

You own the app or have written authorization, and you want to know if sign-in and session handling hold up.

## Checklist, highest impact first

1. **Reset and recovery.** Tokens that are guessable, reusable, long-lived or sent in a Host-controlled link. Evidence: the reset request and the second use of the same token.
2. **MFA bypass.** Skip the second step, reuse a code, or call the post-MFA route directly. Evidence: the request that returns a session without the code.
3. **Token validation.** JWT with `alg: none`, an unchecked signature, wrong audience or no expiry; OAuth `redirect_uri` and `state` handling. Evidence: the modified token and the accepted response.
4. **Session lifecycle.** New session ID after login, invalidation on logout and password change. Evidence: an old cookie replayed after logout.
5. **Cookie flags.** `Secure`, `HttpOnly`, `SameSite`, scope. Evidence: the `Set-Cookie` header.
6. **Brute-force limits.** Lockout or throttling on login, reset and MFA. Send a small bounded burst on a test account. Evidence: status codes and counts.
7. **Account enumeration.** Different messages or timing for known and unknown users. Evidence: both responses side by side.
8. **Password rules and storage signals.** Minimum length, breached-password checks, credentials in URLs or logs. Evidence: the policy response or the log line.

## Safe testing

- Own systems or written authorization only.
- Test-owned accounts only. Never guess passwords for real users.
- Keep burst tests small and stop at the first block. Do not trigger email floods or paid SMS beyond a stated limit.
- Do not lock out accounts that are not yours.

## Evidence rule

"Login works" or "token accepted" alone is not proof. Show the tampered input, the response, and an authenticated action that succeeds with it.

## Report

Finding, evidence, impact, fix. Then "Controls that held": for example a rejected unsigned token, a reset token that failed on second use, a lockout that triggered, each with its response.

## Guide

Authentication testing checklist: https://penetrationtestservices.com/guides/authentication-testing-checklist/
