# Hacker-Man Suite

A browser-based bug bounty recon and passive vulnerability detection toolkit with a hacker/CRT terminal aesthetic. Single HTML file, no install, no backend — just open it and go.

## ⚠️ Authorized Use Only

This tool is for testing targets you **own** or have **explicit permission** to test (e.g. a program listed on HackerOne, Bugcrowd, or Google's VRP). Running it against anything else may be illegal. Always read the program's scope before testing.

## What's Inside

| Tab | What it does |
|---|---|
| **◎ Recon** | Real DNS, IP, ASN, MX/SPF/DMARC, subdomain discovery (hackertarget.com), and optional Shodan integration for ports, service banners, and known CVEs |
| **⚡ Vuln Scan** | Passive checks: missing security headers, CSP weaknesses, CORS misconfig, cookie flags (HttpOnly/Secure/SameSite), exposed sensitive files (`.git`, `.env`, swagger, etc.), and subdomain takeover signals |
| **🎯 Advanced** | Host header injection signals, SSRF candidate params, XXE/XML endpoint detection, prototype pollution signals, and session/OAuth analysis — all mapped to what Google's VRP actually pays for |
| **💉 SQLi Detect** | Error-based, boolean-based blind, and time-based blind SQL injection detection. Confirms *that* an endpoint is injectable without ever extracting data |
| **🔍 XSS · IDOR** | Crawls the target and flags URL parameters that look like XSS, IDOR, or open-redirect candidates for manual verification |
| **📄 Report Builder** | Auto-fills with a properly formatted vulnerability report (title, severity, steps, PoC, impact, remediation) whenever SQLi Detect finds something. Copy or download as `.txt`, ready to paste into HackerOne/Bugcrowd |
| **▶ Workflow / ⚙ Tools / 📖 Vuln Guide / ⌨ Cheatsheet / 💰 Payouts** | A built-in field guide — the real bug bounty process, real tool links (Burp, sqlmap, nuclei, etc.), vulnerability class explanations, copy-paste commands, and payout ranges by severity |

## How to Run

No build step, no dependencies. Just serve the file locally (opening it directly as `file://` will break API calls due to browser CORS rules):

```bash
cd ~/Downloads
python3 -m http.server 8080
```

Then open:
```
http://localhost:8080/hackerman-combined.html
```

## Using It

1. Enter a target domain in the **Recon** tab (no `https://`, no `www.` — just `example.com`)
2. *(Optional)* Paste a free [Shodan](https://account.shodan.io/register) API key for real port/service/CVE data
3. Run **Recon** first — it populates subdomains used by later tabs
4. Run **Vuln Scan**, **Advanced**, **SQLi Detect**, and **XSS · IDOR** in any order
5. Check the **Report Builder** tab — review/edit the auto-filled draft, then copy or download it

## Important Notes

- **Everything here is passive detection, not exploitation.** SQLi checks confirm an endpoint is injectable without ever pulling data. No payloads that modify, delete, or extract real data are ever sent.
- **Findings are signals, not confirmed bugs.** Always manually verify in Burp Suite before submitting a report — automated-looking submissions get marked as duplicate/invalid fast on real programs.
- Recon relies on free public APIs (`dns.google`, `ip-api.com`, `hackertarget.com`) which are rate-limited. If something fails, wait a bit and retry.
- Port scanning requires a free Shodan API key — browsers cannot perform raw TCP port scans.

## Troubleshooting

### "Uncaught SyntaxError" or blank page on load
You likely downloaded an older/corrupted copy. Re-download `hackerman-combined.html` fresh and replace your old file. Don't edit the file in a word processor (Word, Pages) — only a plain text editor or code editor.

### Nothing happens when I click a button
Open browser DevTools (`F12` or `Cmd+Opt+I`) → **Console** tab → look for a red error. The most common cause is **opening the file directly** (`file://...`) instead of serving it locally. Browsers block API calls from `file://` pages due to CORS security rules.

**Fix:**
```bash
cd ~/Downloads
python3 -m http.server 8080
```
Then open `http://localhost:8080/hackerman-combined.html` — not the file directly.

### "NetworkError when attempting to fetch resource"
The free public API (hackertarget.com, dns.google, or Shodan) is either down, rate-limiting you, or the target is blocking automated requests. Wait 30–60 seconds and retry. If it persists on every target, the public API itself may be temporarily down — check the Console for the specific error code (see table below).

### Shodan shows no port data
- Confirm you pasted a valid API key in the **Shodan Key** field
- Free Shodan accounts have limited daily query credits — you may have hit the cap
- Shodan only has data for IPs it has already scanned; brand-new or rarely-scanned IPs may return nothing

### Subdomain / crt.sh lookups return nothing
`hackertarget.com`'s free tier has a daily request cap shared across all its users worldwide. If you're testing many domains in one session, you may hit that shared limit — this is not specific to your key or IP.

### Recon works but Vuln Scan / SQLi / Advanced tabs find nothing
Run **Recon** first — several other tabs (subdomain takeover checks, etc.) depend on data Recon collects. Also confirm the **Target** field at the top is filled in before switching tabs.

## Error Codes

Every failure in the tool logs a code like `[E101]` to the terminal panel and to the browser Console (`F12`). Use this table to look up what went wrong:

| Code | Meaning |
|---|---|
| `E101` | DNS lookup failed — `dns.google` unreachable |
| `E102` | IP geolocation lookup failed — `ip-api.com` unreachable or rate-limited |
| `E103` | Subdomain enumeration failed — `hackertarget.com` unreachable, rate-limited, or `crt.sh` blocked the request |
| `E201` | Shodan API request failed — bad key, no credits left, or rate-limited |
| `E301` | HTTP header fetch failed — `hackertarget.com` unreachable, or the target site is blocking automated/proxy requests |
| `E303` | Subdomain takeover check failed for one or more discovered subdomains |
| `E401` | URL crawl failed — `hackertarget.com` pagelinks endpoint unreachable |
| `E501` | SQLi probe request failed — target or proxy unreachable mid-test |
| `E601` | Host header / cache signal check failed |
| `E602` | SSRF candidate scan failed |
| `E603` | XXE candidate scan failed |
| `E604` | Prototype pollution scan failed |
| `E605` | Session/OAuth analysis failed |
| `E901` | Target field is empty or not a valid domain (must contain a `.`, e.g. `example.com`) |
| `E000` | Startup self-check failed — shown only in browser Console, not the terminal panel. Usually means `file://` protocol or no internet connectivity |

If you see a code not in this table, or the same code repeatedly on every target, open the browser Console (`F12`) for the full error detail and check that:
1. You're running via `http://localhost:8080/...`, not `file://`
2. Your internet connection is active
3. The free API isn't experiencing an outage (rare, but it happens — these are third-party services with no uptime guarantee)

## Files in This Zip

| File | Description |
|---|---|
| `hackerman-combined.html` | **The one to use.** Sidebar nav + all real detection engines |
| `hackerman-suite.html` | Dashboard layout (all panels visible at once) version of the same engines |
| `hackerman-hub.html` | Earlier sidebar-nav version |
| `hackerman-bugbounty.html` | Standalone field guide only (no scanning tools) |
| `hackerman.html` | Original v3.0 — simulated dashboard for demo purposes only |

## Credits

Built with [Claude](https://claude.ai) using free public APIs: [dns.google](https://dns.google), [ip-api.com](https://ip-api.com), [hackertarget.com](https://hackertarget.com), and [Shodan](https://shodan.io).
