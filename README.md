# Allman Contracts — Agg Loads

Interactive demo of the **Agg Loads** docket module for **Allman Contracts Ltd**,
prepared by DMC Consultancy Ltd. It uses the Site / Quarry docket flow from the earlier
Agg Loads wireframes, in Allman colours: navy `#06192E` and orange `#E8973E`, both
sampled from the logo. The logo is embedded in the page as a data URI.

One self-contained `public/index.html`. No build, no dependencies, and no network
requests.

## The flow

| # | Screen | What works |
| --- | --- | --- |
| 1 | **Sign In** | A single Employee button, which signs in as the demo driver |
| 2 | **Dashboard** | Today's loads, tonnes and open drafts, all live. Agg Loads is the only module; nothing else from the app is shown |
| 3 | **Active Loads** | Seeded loads with drafts listed first. Filters for search (docket no. or customer), date range, status and collection type, plus an active-filter count |
| 4 | **New Load** | Please Select: **Site Collection** or **Quarry Collection** |
| 5 | **Step 1: Collection** | Auto docket number, customer, order no., collection address and delivery address (for a quarry: quarry address and site location), vehicle (Artic / Rigid / Low Loader) and reg |
| 6 | **Step 2: Material** | Category, then the type list for that category, weight, **Full Load** (fills 28 t for an Artic or 20 t for a Rigid) and comments |
| 7 | **Step 3: Sign & submit** | Load summary, draw-to-sign customer and driver signatures with printed names, and the docket image (Camera / Device / Scan) |
| 8 | **Submitted** | Confirmation, after which the docket appears at the top of Active Loads |

- **Validation:** each step checks its required fields before moving on. A missing field is outlined red and the screen scrolls to it. Other checks: the collection and delivery addresses can't be the same, the weight must be more than 0 and no more than 45 t, both signatures are required, and the docket image is required for quarry collections (optional for site).
- **Drafts:** the bookmark icon on any step, or **Save draft** on step 3, saves the docket as an amber *Draft*. Tapping a draft row reopens the form exactly where it was left.
- **Submitted dockets** open as a read-only docket sheet showing signatures, the docket image and a Share PDF stub.
- The **flow rail** beside the phone highlights the current step, so the demo can be followed on a shared screen.

Dates are taken from the day the demo is opened, so "today" always reads correctly.
State is saved in `localStorage`. **Reset demo** restores the seeded data.

## Running it

There's no Node on this machine, so serve the folder with Python:

```bash
python3 -m http.server 8794 --directory public
```

Then open http://localhost:8794

With Node installed, `npm start` does the same thing through `server.js`, which is also what Railway runs.

## Deploying to Railway

New Project → Deploy from GitHub repo → this repository. Railway reads
`railway.json` and runs `npm start`. The server binds to `process.env.PORT`, and the health check is at `/healthz`.

## Demo data

Customers, sites, quarries, materials and the driver (David Allman, 231-KE-4471) are
placeholders. They need replacing with Allman's real lists before anything is wired to the backend.
