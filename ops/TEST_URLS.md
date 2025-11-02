# /proxy quick test URLs

Replace <DOMAIN> with your domain.

## JSON tests (should return JSON, status 200)
1. https://<DOMAIN>/proxy/https%3A%2F%2Fjszyapi.com%2Fapi.php%2Fprovide%2Fvod%3Fac%3Dvideolist%26wd%3Dtest
2. https://<DOMAIN>/proxy/https%3A%2F%2Fbfzyapi.com%2Fapi.php%2Fprovide%2Fvod%3Fac%3Dvideolist%26wd%3Dtest
3. https://<DOMAIN>/proxy/https%3A%2F%2Fwolongzyw.com%2Fapi.php%2Fprovide%2Fvod%3Fac%3Dvideolist%26wd%3Dtest

## M3U8 test (should return #EXTM3U text, status 200)
4. https://<DOMAIN>/proxy/https%3A%2F%2Ftest-streams.mux.dev%2Fx36xhzz%2Fx36xhzz.m3u8

## What success looks like
- JSON tests: Content-Type application/json; body with fields like `list`.
- M3U8 test: Content-Type application/vnd.apple.mpegurl; body begins with `#EXTM3U` and media segment URLs are rewritten to `/proxy/...`.

## If it fails
- 404 or HTML page: your Nginx didn’t route `/proxy/*` to backend.
- 403 from CDN/WAF: allowlist path `/proxy/*` with encoded http/https URLs.
- Timeout: check backend Node/Express is running on 8080 and reachable from Nginx.

## Quick curl
curl -I https://<DOMAIN>/proxy/https%3A%2F%2Fjszyapi.com%2Fapi.php%2Fprovide%2Fvod%3Fac%3Dvideolist%26wd%3Dtest
curl -s https://<DOMAIN>/proxy/https%3A%2F%2Fjszyapi.com%2Fapi.php%2Fprovide%2Fvod%3Fac%3Dvideolist%26wd%3Dtest | head -c 400
