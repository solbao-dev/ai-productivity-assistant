# Security Policy

## Public deployment model

The public build of this project is intentionally credential-free.

- It does not request GitHub Personal Access Tokens (PATs).
- It does not request API keys.
- It does not store credentials in localStorage, sessionStorage, cookies, or application state.
- It does not call GitHub Models or another external AI API.
- Task prioritization is performed locally using explicit rules.

## Why the AI credential flow was removed

The original prototype accepted a user-provided GitHub PAT in the browser and used it to authenticate requests to GitHub Models. During the public-deployment review, this design was reconsidered because a static client cannot provide the same credential isolation as a server-side architecture.

The review considered persistent browser storage, in-memory token handling, client-side script access, XSS exposure, malicious extensions, shared-device risks, accidental screen/log exposure, and the lack of a server-side trust boundary. The final decision was to remove credential handling from the public build rather than merely reduce persistence.

The original implementation is retained in `archive/original-ai-prototype` for learning-history purposes. It is not the recommended public deployment.

## Threat model

The public build aims to reduce these risks:

| Risk | Public-build mitigation |
|---|---|
| Repository secret leakage | No application secret is required |
| Visitor PAT exposure | No PAT input or storage exists |
| Credential persistence | No credential persistence exists |
| Task leakage through AI analysis | No external AI request is made |
| HTML injection from task text | User task text is rendered with `textContent` |
| Unexpected outbound browser requests | CSP sets `connect-src 'none'` |
| Misleading AI claims | Rule-based results are explicitly labeled |

## Residual risk

No browser application can guarantee absolute security. A compromised browser, operating system, hosting platform, extension, or user device is outside the protection boundary of this static application. Security controls reduce risk; they do not prove that every possible attack is impossible.

## Production architecture

If AI functionality is restored for a production service, the recommended direction is:

1. Keep provider secrets on a backend/BFF rather than in public client code.
2. Authenticate and authorize users before protected operations.
3. Validate and constrain input server-side.
4. Add rate limiting, abuse protection, and request-size limits.
5. Redact secrets and sensitive task data from logs.
6. Store secrets in an appropriate server-side secret manager/environment configuration.
7. Define retention and privacy rules for user task data.
8. Review the AI provider's data-handling terms before transmitting user content.
9. Use HTTPS and appropriate security headers.
10. Maintain dependency and secret-scanning practices as the project grows.

## Local development safety

The current `main` branch requires no PAT or API key. Do not add real credentials to source files, screenshots, issues, commits, or example configuration. If a credential is accidentally exposed, revoke/rotate it at the provider immediately; deleting it from the latest file alone is not sufficient because Git history and caches may retain it.

## Reporting a security issue

This repository is a portfolio/learning project. Please avoid posting real credentials or sensitive personal data in a public issue. If demonstrating a problem, use redacted or synthetic values only.
