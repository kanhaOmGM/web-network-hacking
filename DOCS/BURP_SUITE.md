# Burp Suite — Overview & Key Features

**Summary:** Burp Suite captures and enables manipulation of HTTP/HTTPS traffic between a browser and a web server. This capability is the backbone of the framework and is widely used for web and mobile application testing. :contentReference[oaicite:12]{index=12}

## Editions
- **Community:** basic proxy, repeater, decoder, etc.
- **Professional:** adds an automated vulnerability scanner, fuzzer/brute-forcer (no rate limits), saving projects, API, unlimited extensions, Burp Collaborator, and reporting.
- **Enterprise:** server-based continuous scanning for automated scans similar to Nessus.

## Key Tools
- **Proxy:** intercept and modify requests/responses.
- **Repeater:** craft and resend requests (good for manual testing like SQLi).
- **Intruder:** brute-force/fuzzing (rate-limited in Community).
- **Decoder:** encode/decode payloads quickly.
- **Comparer:** compare responses at word/byte level.
- **Sequencer:** analyze randomness of tokens (session cookies, etc).

## Workflow highlights
- Intercept requests via Proxy; forward, drop, edit, or send to other modules.
- Use Repeater for trial-and-error payload crafting.
- Use Intruder for spraying/fuzzing endpoints.
- Logs: HTTP history and WebSockets history help retrospective analysis.
- Site map: automatically maps pages visited when proxy is active — useful for enumeration and API discovery.
- Scope settings: define what to capture/log to avoid noise; add a host to scope to focus testing.

## Settings
- **Global vs Project settings:** Global affect entire installation; Project apply per-session (Community does not save projects).
- Search and filters in Settings make finding options easier.

## Shortcuts
- `Ctrl+Shift+D` — Dashboard
- `Ctrl+Shift+T` — Target
- `Ctrl+Shift+P` — Proxy
- `Ctrl+Shift+I` — Intruder
- `Ctrl+Shift+R` — Repeater

## Notes & Best practices
- Use scope to reduce noise and focus tests.
- Capture and log carefully — large captures can be overwhelming.
- Professional edition’s Issue/Advisory sections provide vulnerability details and suggested remediation.

(Adapted from the original module notes.) :contentReference[oaicite:13]{index=13}
