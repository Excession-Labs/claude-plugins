---
name: security-headers-review
description: Check the HTTP security headers of a public web page with the check_security_headers MCP tool, then explain each failed or warned header and give config snippets for nginx, Apache, Caddy and Express. Use when asked to review, grade or fix security headers.
---

# Security headers review

Use the `check_security_headers` tool from the `penetrationtestservices` MCP server (https://penetrationtestservices.com/mcp). It makes one GET request per redirect, like a browser visit, and reads headers only. It is not a penetration test, and it covers one URL.

## Steps

1. Ask for a public http or https URL if the user has not given one. Do not call the tool for a private or internal address.
2. Call `check_security_headers` with `url`.
3. Report the counts (pass, warn, fail), the final URL and the check time from the result.
4. For each FAIL and WARN, explain the risk in one sentence and give a snippet. Fix FAIL items first.
5. Tell the user to test in staging, because CSP and COOP can break scripts, embeds and sign-in pop-ups.
6. State the limit: headers do not show whether the application is vulnerable.

## Checks the tool reports

HTTPS, Strict-Transport-Security, Content-Security-Policy, clickjacking protection (CSP `frame-ancestors` or X-Frame-Options), X-Content-Type-Options, Referrer-Policy, Permissions-Policy, Cross-Origin-Opener-Policy, version disclosure and cookie flags.

## Snippets

Values come from OWASP guidance. Adapt before use.

nginx:

```nginx
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
add_header X-Content-Type-Options "nosniff" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Permissions-Policy "camera=(), microphone=(), geolocation=()" always;
add_header Content-Security-Policy "default-src 'self'; frame-ancestors 'self'" always;
server_tokens off;
```

Apache:

```apache
Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
Header always set X-Content-Type-Options "nosniff"
Header always set Referrer-Policy "strict-origin-when-cross-origin"
Header always set Permissions-Policy "camera=(), microphone=(), geolocation=()"
Header always set Content-Security-Policy "default-src 'self'; frame-ancestors 'self'"
ServerTokens Prod
```

Caddy:

```caddy
header {
  Strict-Transport-Security "max-age=31536000; includeSubDomains"
  X-Content-Type-Options "nosniff"
  Referrer-Policy "strict-origin-when-cross-origin"
  Permissions-Policy "camera=(), microphone=(), geolocation=()"
  Content-Security-Policy "default-src 'self'; frame-ancestors 'self'"
  -Server
}
```

Express (with the `helmet` package):

```js
import helmet from "helmet";
app.use(helmet());
app.disable("x-powered-by");
```

## Notes on specific headers

- A strict `default-src 'self'` CSP blocks inline scripts and third-party scripts. Start from the site's real needs, or use `Content-Security-Policy-Report-Only` first.
- Add `includeSubDomains` to HSTS only when every subdomain serves HTTPS.
- Add `Cross-Origin-Opener-Policy: same-origin` only after you check that sign-in pop-ups still work.
- Cookies: set `Secure`, `HttpOnly` and a `SameSite` value.

## Sources

- OWASP HTTP Headers Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Headers_Cheat_Sheet.html
- Free web checker: https://penetrationtestservices.com/tools/security-headers-checker/ (published by Excession Labs)
