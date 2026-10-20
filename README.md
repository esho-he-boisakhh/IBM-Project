# Automated Web Security Scanner

A Python tool that audits websites for missing security headers, insecure cookie flags, information disclosure and CORS misconfigurations, then scores each target and exports HTML/CSV/JSON reports.

## Project Description

Web applications are often compromised through simple configuration mistakes rather than complex exploits. This project automates the checks a security tester would perform by hand:

| Module | What it checks |
|---|---|
| **Security headers** | HSTS, CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy (presence and weak values) |
| **CORS** | Reflected origins, `null` origin, wildcard with credentials, lookalike-domain trust, risky preflight methods |
| **Cookies** | `Secure`, `HttpOnly`, `SameSite` flags |
| **Disclosure** | Version leaks in `Server`, `X-Powered-By`, etc. |
| **Transport** | Plain HTTP vs HTTPS |

Every finding has a severity (Critical / High / Medium / Low), and each target receives a score out of 100 and a grade (A-F).

## Dataset

No external dataset is used. The notebook includes a **built-in local test lab** (three small HTTP servers: vulnerable, hardened and mixed) so results are reproducible offline.
Optional live practice targets (legal to test): `http://testphp.vulnweb.com`, `http://demo.testfire.net`, or a local [OWASP Juice Shop](https://github.com/juice-shop/juice-shop).

## Technologies Used

- Python 3.9+
- `requests` (HTTP requests)
- `pandas` (results and summaries)
- `matplotlib` (severity chart)
- Jupyter Notebook
- Standard library: `http.server`, `threading`, `re`, `html`, `json`

## Setup and Run Instructions

```bash
1. (Optional) creates a virtual environment
python -m venv .venv
.venv\Scripts\activate

2. Install dependencies
pip install -r requirements.txt

3. Launch the notebook
jupyter notebook Prabrisha_Bharadwaj_WebSecurityScanner.ipynb
```

Run all cells (**Kernel > Restart & Run All**). The notebook will:

1. Start the local lab servers on ports 8081-8083
2. Scan them and show a summary table, detailed findings and a chart
3. Export `scan_report.html`, `scan_results.csv`, `scan_results.json` and `severity_chart.png`

**Scanning real sites:** in Section 8 set `RUN_LIVE = True` and edit `LIVE_TARGETS`.
**Batch scanning:** put one URL per line in `targets.txt` and use the snippet in Section 10.

## Key Information

- **Sample results (local lab):** vulnerable server 0/100 (F), mixed server 60/100 (D), hardened server 100/100 (A).
- **Scoring:** 100 minus penalties (Critical 25, High 15, Medium 8, Low 3).
- **Limitations:** scans only the URLs supplied (no crawling); CSP analysis uses pattern checks, not a full parser; cannot detect route-specific headers.
- **Legal notice:** only scan systems you own or have written permission to test.

## Files

| File | Purpose |
|---|---|
| `Prabrisha_Bharadwaj_WebSecurityScanner.ipynb` | Complete project code |
| `requirements.txt` | Python dependencies |
| `Prabrisha_Bharadwaj_ProjectReport.docx` | Full project documentation |
| `README.md` | This file |
