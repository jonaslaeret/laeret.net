# laeret.net: setup

Static personal page (`index.html`). Hosted free on **GitHub Pages** with HTTPS. Email on **Microsoft 365** using the same domain (already set up; do not touch the mail records). DNS is at **Namecheap** (Basic DNS, `dns1/dns2.registrar-servers.com`).

Files served: `index.html`, `fonts/`, `portrait.jpg`, `og-image.png` (link preview), `favicon.svg`, `favicon-32.png`, `apple-touch-icon.png`, `CNAME`. This file is excluded from the site by `_config.yml`.

## 1. Publish the page (GitHub Pages)

```bash
cd C:/dev/laeret.net
git init -b main && git add . && git commit -m "Personal page"
gh repo create jonaslaeret/laeret.net --public --source . --push
gh api -X POST repos/jonaslaeret/laeret.net/pages -f "source[branch]=main" -f "source[path]=/"
gh api -X PUT  repos/jonaslaeret/laeret.net/pages -f cname=laeret.net
```

When DNS (below) has propagated, turn on HTTPS:

```bash
gh api -X PUT repos/jonaslaeret/laeret.net/pages -F https_enforced=true
```

## 2. Verify the domain with GitHub

Protects the domain from being claimed by another GitHub account. GitHub → Settings (your account) → Pages → **Add a domain** → `laeret.net`. GitHub shows a TXT record:

| Type | Host | Value |
|---|---|---|
| TXT | `_github-pages-challenge-jonaslaeret` | *(value shown by GitHub)* |

Add it at Namecheap, then click **Verify**.

## 3. DNS records at Namecheap

Namecheap → Domain List → laeret.net → **Advanced DNS**.

**Remove:** the existing A record for `@` (the old address) and any URL redirect or parking records.

**Add:**

| Type | Host | Value |
|---|---|---|
| A | @ | 185.199.108.153 |
| A | @ | 185.199.109.153 |
| A | @ | 185.199.110.153 |
| A | @ | 185.199.111.153 |
| CNAME | www | jonaslaeret.github.io. |

**Keep as they are (Microsoft 365 mail):** TXT `MS=…`, TXT SPF, MX, CNAME `autodiscover`, CNAME `selector1._domainkey` / `selector2._domainkey`, TXT `_dmarc`.

## 4. Check

```bash
nslookup -type=A laeret.net 8.8.8.8
nslookup -type=CNAME www.laeret.net 8.8.8.8
curl -sI https://laeret.net | head -1
```

Then paste https://laeret.net into LinkedIn's Post Inspector to check the link preview.
