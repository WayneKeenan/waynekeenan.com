# waynekeenan.com

Redirects `waynekeenan.com` and `www.waynekeenan.com` to <https://wkdlabs.com>, keeping the path.

GitHub Pages can't send a 301 to another domain, so `index.html` and `404.html` (identical) use a
meta refresh plus `location.replace()`. Unknown paths therefore return HTTP 404 with the redirect page.

## Hosting

- GitHub Pages, repo `WayneKeenan/waynekeenan.com`, deploy from branch `main`, root.
- Custom domain `waynekeenan.com` (`CNAME` file), HTTPS enforced.
- Domain verified on the WayneKeenan GitHub account (Settings → Pages → Verified domains).
  Without this GitHub reported the domain as "already taken".

Separate repo because the wkdlabs.com Pages repo (`WKDLabs/wkdlabs.github.io`) can serve only one domain.

## DNS (Mythic Beasts, ns1/ns2.mythic-beasts.com)

| Host | Type | Value |
|---|---|---|
| `@` | A | 185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153 |
| `@` | AAAA | 2606:50c0:8000::153, 2606:50c0:8001::153, 2606:50c0:8002::153, 2606:50c0:8003::153 |
| `www` | CNAME | `waynekeenan.github.io.` |
| `_github-pages-challenge-waynekeenan` | TXT | GitHub domain verification — keep it |

Before this change the zone had only NS records and `www` CNAME `www.wkdlabs.com.`.

Managed with the Mythic Beasts DNS API v2. Credentials live in `~/.netrc`
(`machine api.mythic-beasts.com login KEY_ID password SECRET`, chmod 600) — never in this repo.

```sh
API=https://api.mythic-beasts.com/dns/v2/zones/waynekeenan.com/records
curl -sn "$API"                                   # list records
curl -sn -X PUT -H 'Content-Type: application/json' "$API/www/CNAME" \
  -d '{"records":[{"host":"www","ttl":300,"type":"CNAME","data":"waynekeenan.github.io."}]}'
```

Host names must be lowercase (uppercase returns HTTP 400). Changes reach the nameservers in 1–2 minutes.

## Change the target

Edit the URL in `index.html` and `404.html`, then commit and push to `main`.
