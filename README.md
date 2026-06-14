# Quantum MF Terminal

An institutional-grade, single-file mutual fund analytics dashboard for Indian mutual funds. Built as a self-contained `index.html` — no build step, no backend, just open in a browser.

Live NAV data is sourced from the public [MFAPI.in](https://www.mfapi.in/) endpoints.

---

## ✨ Features

### Portfolio & Trading
- **Live Portfolio Dispatch** — Commit real holdings with scheme, units, entry NAV, and investment date.
- **Sandbox Trading** — Paper-trade ideas without touching the live portfolio.
- **Auto Entry NAV** — Selecting a scheme or changing the investment date automatically fetches the historical NAV on (or closest to) that date from MFAPI.
- **Loading-state guards** — Spinner appears inside the NAV field while fetching; Save / Dispatch buttons are disabled until the NAV refresh completes, preventing stale-price submissions.

### Analytics
- Scheme search and metadata lookup
- Historical NAV charts
- Returns, drawdown, and risk metrics
- Portfolio aggregation across holdings

### Quality
- Built-in **regression test harness** — append `?test=1` to the URL to run automated tests that verify:
  - Entry NAV auto-refreshes when the **Investment Date** changes (Sandbox + Live)
  - Entry NAV auto-refreshes when the **Scheme** changes (Sandbox + Live)
  - Spinner is shown during fetch
  - Save / Dispatch buttons are disabled while loading
  - Submissions are blocked while a NAV fetch is in flight
- Results render in a floating, color-coded pass/fail panel.

---

## 🚀 Getting Started

```bash
git clone https://github.com/<you>/quantum-mf-terminal.git
cd quantum-mf-terminal
open mf_dashboard_3.html        # macOS
# or just double-click the file
```

That's it. No `npm install`, no server, no API keys.

### Running the regression tests
```
file:///path/to/index.html?test=1
```
A floating panel reports each assertion. All rows should be green.

---

## 🧠 How the NAV Auto-Refresh Works

1. User picks a scheme **or** edits the Investment Date in either modal.
2. `updateNavForDate(schemeCode, date, navInputId, wrapId, submitBtnId)` runs:
   - Marks the input as `loading` (spinner shown, save button disabled, placeholder → `Fetching NAV…`).
   - Calls `https://api.mfapi.in/mf/{schemeCode}` and walks the history to find the NAV on, or the most recent trading day before, the chosen date.
   - Writes the resolved NAV back into the input and dispatches an `input` event so downstream calculations recompute.
3. A `finally` block always clears the loading state, even on network errors.
4. Submit handlers refuse to fire while `navInp.dataset.loading === '1'` and surface a toast instead.

---

## 📁 Project Structure

```
mf_dashboard_3.html      # The entire app — HTML, CSS, JS, tests
README.md                # This file
```

---

## 🌐 Data Source

NAV data is provided by [MFAPI.in](https://www.mfapi.in/) — a free public API mirroring AMFI NAV history. No authentication required. Please respect their fair-use policy.

---

## ⚠️ Disclaimer

This project is for **educational and analytical purposes only**. It is **not** investment advice. Always verify NAV values and consult a SEBI-registered advisor before making investment decisions.

---

## 📄 License

MIT — see `LICENSE` (add your own).
