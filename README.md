# ValueLens 4.0 — All JavaScript, one Netlify project

React/Vite frontend + JavaScript Netlify Function + pure-JavaScript screening/DCF engine. **No Python, FastAPI, Render or Streamlit.**

## Quick start
Node.js 20.19+ or compatible modern Node recommended.

```bash
npm install
cp .env.example .env
# For safe demo mode: set VALUELENS_DEMO=true in .env
npm run dev
```
Open the Netlify CLI URL (usually http://localhost:8888), **not** the Vite-only port: only Netlify CLI serves `/.netlify/functions/screen`. Run `npm test` to check scoring/DCF. `npm run build` generates `dist`.

## Real financial data
Set `FMP_API_KEY` as a **server-side Netlify environment variable** (Financial Modeling Prep API). Turn off `VALUELENS_DEMO`. FMP stable API endpoint access, US/TSX coverage and request limits depend on your subscription. Verify the endpoints and licensing against your plan; some free plans cannot supply the required fundamentals. Never add your key to a `VITE_` variable or commit `.env`.

## Deploy on Netlify
1. Upload the contents of this directory to a GitHub repository (root contains `package.json` and `netlify.toml`).
2. Netlify > Add new project > Import existing repository. Base directory: repository root (leave blank); build command `npm run build`; publish `dist`; functions `netlify/functions` (all also specified in `netlify.toml`).
3. Set environment variables `FMP_API_KEY` (or `VALUELENS_DEMO=true` for explicitly labeled synthetic demonstration). Set scope for Functions and rebuild/redeploy.
4. Deploy. Test `/.netlify/functions/screen` using the UI. Do not open this endpoint in a browser expecting a page; it requires POST.
5. To integrate with your existing Netlify website, link to the deployed app or merge this project into a separate route after reviewing your existing site's build configuration. Do not overwrite the existing site inadvertently.

## Limitations
- Relative rankings depend on the user-selected universe, require >=6 available metrics and >=2 pillars. The scores are **not buy signals**.
- DCF uses simplified equity free cash flow, five years, terminal growth, and reported shares. No fair value for nonpositive FCF or invalid inputs. Confirm currencies, diluted shares, cash flows and provider period consistency before investment use.
- Provider requests are sequential and may exceed serverless execution limits for large universes; begin with 3–5 symbols. A production scanner should add licensed batch endpoints, durable caching, scheduled ingestion, authentication, rate limits and error monitoring.
- FMP API responses vary by plan and endpoint. Missing fields are represented as null and warnings identify failed endpoints. Historical-price endpoint is optional; without it, momentum is missing.
- `VALUELENS_DEMO=true` uses synthetic values and is clearly labeled. It is **not live market data**.
- No trading, brokerage connection or personalized advice.
