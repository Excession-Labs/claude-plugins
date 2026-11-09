---
name: secrets-and-config-review
description: Find exposed secrets and risky configuration in a repo, build, deployment or public site. Use when asked to check for leaked API keys, tokens, .env files, hardcoded credentials, exposed storage, debug mode, CORS, CI secrets or cloud and server config.
---

# Secrets and config review

## When to use

You own the code or deployment, or have written authorization, and want leaked credentials and unsafe settings found.

## Checklist, highest impact first

1. **Live credentials in source or history.** Scan the working tree and `git log -p` for keys, tokens and private keys. Evidence: file path, commit and line, with the value masked.
2. **Secrets in shipped files.** Check JavaScript bundles, source maps, mobile builds and Docker images. Evidence: the URL or layer and the masked match.
3. **Exposed files.** `/.env`, `/.git/`, backups, `phpinfo`, admin consoles. Evidence: the request and the status plus first bytes, masked.
4. **Open storage and databases.** Public buckets, unauthenticated dashboards. Evidence: a listing request that returns objects. Do not download data.
5. **CI and deploy config.** Secrets echoed in logs, broad tokens, untrusted pull requests with secret access. Evidence: the workflow line.
6. **Runtime settings.** Debug mode, verbose errors, default credentials, permissive CORS with credentials. Evidence: the config line or response header.
7. **Rotation state.** For each leaked secret, check whether it is still valid, using a read-only call on your own account. Evidence: the status of one harmless request.

## Safe testing

- Own systems or written authorization only.
- Never print, store or share a secret. Mask all but the first four characters in notes.
- Test a found key only if the owner approves, and only with read-only calls. Never use it to reach other people's data.
- Do not delete, write or change anything while checking.

## Evidence rule

"Looks exposed" is not proof. Show the file and line or the request and response, with values masked. Say whether the secret is valid or revoked.

## Report

Finding, evidence (masked), impact (what the secret can reach), fix (rotate first, then remove from history and add scanning). Add "Controls that held": for example blocked `/.env`, private buckets, secrets read from the environment.

## Guide

Check security headers and config online: https://penetrationtestservices.com/tools/security-headers-checker/
