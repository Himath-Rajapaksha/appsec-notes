# HTTP response header audit

**Noted:** 2026-11-06
**Time:** about 10 minutes per host
**Goal:** catch missing or misconfigured security headers before they turn into a finding

## Why this drill

Header problems are cheap to find, cheap to fix, and each one maps to a concrete bug class. Running it as a fixed checklist stops me from checking CSP and forgetting the rest.

## Collection

```bash
curl -sS -D - -o /dev/null https://target.tld | sort -f
```

Run it against the apex, `www`, the login route, an authenticated route, and an API route. The same app often ships different headers per path.

## Checklist

| header | good looks like | what it prevents |
| --- | --- | --- |
| `Strict-Transport-Security` | `max-age=31536000; includeSubDomains` | SSL-strip, downgrade, cookie theft on the first request |
| `Content-Security-Policy` | no `unsafe-inline`, no wildcard, `object-src 'none'` | XSS impact, and limits data exfiltration |
| `X-Content-Type-Options` | `nosniff` | MIME sniffing turning an upload into script |
| `Referrer-Policy` | `strict-origin-when-cross-origin` or stricter | token and URL leakage to third parties |
| `Permissions-Policy` | explicit denies (`camera=(), microphone=(), geolocation=()`) | feature abuse through embedded frames |
| `Cross-Origin-Opener-Policy` | `same-origin` | XS-Leaks, window handle abuse |
| `Cross-Origin-Resource-Policy` | `same-origin` or `same-site` | cross-origin reads of responses |
| `X-Frame-Options` | `DENY`, or CSP `frame-ancestors` | clickjacking |
| `Cache-Control` on auth routes | `no-store` | cached PII after logout |

## Things I keep forgetting

- HSTS only means something over HTTPS and is ignored on the first plaintext request — preload matters
- `Content-Security-Policy-Report-Only` is not enforcement; read the header name, not the intent
- A strong CSP on the marketing site says nothing about the app subdomain
- `Server` / `X-Powered-By` versions are not a header finding by themselves, but they set the patch-verification baseline
- Check error responses too (401/403/500) — frameworks often skip middleware there

## What I would put in a report

One finding per missing control, with the header, the request that shows it missing, the bug class it enables, and the exact value to set. Severity is usually low or medium individually; the framing that lands is "clickjacking is possible on the account settings page", not "missing X-Frame-Options".

## Sources

- OWASP Secure Headers Project: https://owasp.org/www-project-secure-headers/
- MDN HTTP headers: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers
