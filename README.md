# David Allman Haulage — Agg Loads

Interactive demo of the **Agg Loads** docket module for **David Allman Haulage**,
prepared by DMC Consultancy Ltd. It uses the Site / Quarry docket flow from the earlier
Agg Loads wireframes, in black `#111111` and yellow `#FFC20E`. The company has no logo,
so the header shows a plain text wordmark (DAVID ALLMAN / HAULAGE).

One self-contained `public/index.html`. No build, no dependencies, and no network
requests.

## The flow

| # | Screen | What works |
| --- | --- | --- |
| 1 | **Sign In** | A single Employee button, which signs in as the demo driver |
| 2 | **Dashboard** | Clock band at the top (status line, then Clock In or Clock Out), a reminder when the day's vehicle check isn't done, today's loads, tonnes and open drafts, and two tiles: Vehicle Check and Agg Loads |
| — | **Clock in popup** | Clock In (on the dashboard band or the New Load reminder) opens a bottom sheet laid out like the app's *Update Timesheet* popup: drag handle, title with a close button, an accent line, **Job Type** (defaults to Driver) and **Select Vehicle** dropdowns, then Cancel / Submit. Submit without a truck outlines the field red |
| — | **Checks** | Today's status for the lorry you're clocked in on, a Start daily check button and the recent checks. Each one opens read only |
| — | **Vehicle Checklist** | Laid out like the app's Vehicle Checklist screen. It has an info panel (Employee, Reg, Date, live Time), **Select Vehicle** (required), **General Remarks**, **Odometer Reading** (required, can't go below the last reading) and an Item / Checked / Remark table. The table is grouped into **In Cab Checks**, **Walk-around Checks** and **On-the-Road**: 23 items for an artic, 22 for a rigid, which has no trailer coupling. Tapping the Checked box cycles blank → tick → defect (X) → blank, and a defect needs a remark. Below the table are a **NIL Defects** box that ticks everything, **Images (0/6)** with Camera / Upload File, and a required **Add Signature** row. Submit runs along the bottom |
| 3 | **Active Loads** | Seeded loads with drafts listed first. Filters for search (docket no. or customer), date range, status and collection type, plus an active-filter count |
| 4 | **New Load** | Please Select: **Site Collection** or **Quarry Collection** |
| 5 | **Step 1: Collection** | Auto docket number, customer, order no., collection address and delivery address (for a quarry: quarry address and site location), vehicle (Artic / Rigid / Low Loader) and reg |
| 6 | **Step 2: Material** | Category, then the type list for that category, weight, **Full Load** (fills 28 t for an Artic or 20 t for a Rigid) and comments |
| 7 | **Step 3: Sign & submit** | Load summary, draw-to-sign customer and driver signatures with printed names, and the docket image (Camera / Device / Scan) |
| 8 | **Submitted** | Confirmation, after which the docket appears at the top of Active Loads |

- **Clock in / out and the check** follow the earlier wireframes. The clock band comes from the Donohoe demo; the checklist follows the app's own Vehicle Checklist screen, with the Donohoe tap-to-cycle Checked box and Shannon Valley's rule that a defect needs a note. As in those demos, clocking in never needs the check first. The app reminds you on the dashboard and on New Load instead. A docket started while clocked in picks up that lorry's reg and type.
- **Checklist validation:** vehicle selected, odometer (can't go below the last reading), every item ticked or marked as a defect, a remark on each defect, and a signature. Each one gets its own message, and the screen scrolls to the first problem.
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

Customers, sites, quarries, materials, the three lorries, the check items and the driver (David Allman) are
placeholders. They need replacing with the company's real lists before anything is wired to the backend.
