# Automated Web Security Scanner

A Python tool that audits websites for missing security headers, insecure cookie flags, information disclosure and CORS misconfigurations, then scores each target and exports HTML, CSV and JSON reports.

## Project Description

Web applications are often compromised through simple configuration mistakes rather than complex exploits. This project automates the checks a security tester would perform by hand:

| Module | What it checks |
|---|---|
| **Security headers** | HSTS, CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy and Permissions-Policy (presence and weak values) |
| **CORS** | Reflected origins, `null` origin, wildcard with credentials, lookalike-domain trust and risky preflight methods |
| **Cookies** | `Secure`, `HttpOnly` and `SameSite` flags |
| **Disclosure** | Version leaks in `Server`, `X-Powered-By` and similar headers |
| **Transport** | Plain HTTP versus HTTPS |

Every finding has a severity (Critical, High, Medium or Low), and each target receives a score out of 100 and a grade from A to F. Targets that cannot be reached are reported as `ERROR` and are not graded.

## Dataset

No external dataset is used. The notebook includes a built-in local test lab of three small HTTP servers (vulnerable, hardened and mixed), so results are reproducible offline.

Optional live targets for testing: `http://testphp.vulnweb.com`, `http://demo.testfire.net`, `https://httpbin.org`, or a local [OWASP Juice Shop](https://github.com/juice-shop/juice-shop).

## Technologies Used

- Python 3.9 or newer
- `requests` for HTTP requests
- `pandas` for results and summaries
- `matplotlib` for the severity chart
- Jupyter Notebook
- Standard library modules: `http.server`, `threading`, `re`, `html` and `json`

## Setup and Run Instructions

```bash
# 1. Create and activate a virtual environment
python -m venv venv
Windows: venv\Scripts\activate

# 2. Install the dependencies
pip install -r requirements.txt

# 3. Start Jupyter and open the project notebook (.ipynb)
jupyter notebook
```

Run all cells (**Kernel > Restart & Run All**). The notebook then:

1. Starts the local lab servers on ports 8081 to 8083.
2. Scans them and shows a summary table, the detailed findings and a chart.
3. Scans the live targets when `RUN_LIVE` is `True` (Section 9).
4. Exports `scan_report.html`, `scan_results.csv`, `scan_results.json` and `severity_chart.png`.

Live targets are changed through `LIVE_TARGETS` in Section 9. Setting `RUN_LIVE = False` skips the live scan.

## Key Information

- **Local lab results:** vulnerable server 0/100 (F), mixed server 60/100 (D), hardened server 100/100 (A).
- **Live results:** `demo.testfire.net` 54/100 (D), `httpbin.org` 0/100 (F), `testphp.vulnweb.com` not reachable from the test network (ERROR).
- **Scoring:** 100 minus penalties (Critical 25, High 15, Medium 8, Low 3), with a minimum of 0.
- **Limitations:** only the URLs supplied are scanned (no crawling); CSP analysis uses pattern checks rather than a full parser; route-specific headers are not detected.
- **Legal notice:** only systems that are owned or that grant written permission should be scanned.



## Files

| File | Purpose |
|---|---|
| `PrabrishaBharadwaj_WebSecurityScanner.ipynb` | Complete project code |
| `requirements.txt` | Python dependencies |
| `PrabrishaBharadwaj_ProjectReport.docx` | Full project documentation |
| `README.md` | This file |
