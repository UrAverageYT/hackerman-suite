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
