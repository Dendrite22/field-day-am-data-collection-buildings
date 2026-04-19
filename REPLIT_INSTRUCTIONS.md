# Replit Change Instructions — AM Data Collection: Buildings

The following changes are required for the **AM Data Collection: Buildings** module. Implement each item fully before moving to the next.

---

## 1. Excel Import Feature

Add an import button to the Buildings list view (web and PWA) that allows users to upload an `.xlsx` or `.csv` file to bulk-import buildings into the current job.

**Behaviour:**
- The import button should be clearly labelled **"Import Buildings"** and placed near the top of the buildings list.
- On file selection, parse the spreadsheet client-side and map columns to the following building fields:
  - `name`
  - `client_id`
  - `parent_asset`
  - `address`
  - `lat` / `lng` (geospatial coordinates, if present in the file)
  - `asset_id` (or any additional asset identifier columns present)
  - `components` (if included — match against the existing component taxonomy)
- Show the user a preview/mapping screen before confirming the import, so they can verify column alignment.
- On confirm, POST each row to the existing buildings + components endpoints in sequence.
- Report success/failure per row with a summary toast on completion.
- If a row fails, skip it and continue — do not abort the entire import.

---

## 2. "Add Building" Button (adjacent to Map icon)

Place an **"Add Building"** button directly next to the Map view icon in the Buildings list header/toolbar.

**On click — form fields (match the buildings database schema exactly):**
- `Name` — text input, required
- `Client ID` — UUID input (can be a text field — validate UUID format)
- `Parent Asset` — text input
- `Address` — text input
- `Geospatial Reference` — capture the device's current GPS coordinates (`lat` / `lng`) automatically at the moment the form is opened; display them as read-only fields so the inspector can see what was captured. Allow manual override if needed.

**On save:**
- POST the new building to the backend as normal.
- The building must immediately appear as a pin/marker on the Leaflet map without requiring a page refresh.
- If coordinates were captured, the map should pan/zoom to the new pin.

---

## 3. Delete Building (Swipe-to-Delete)

Add swipe-to-delete on each building row in the buildings list.

**Behaviour:**
- Swiping a building tab **right-to-left** reveals a red **"Delete"** button on the right side of the row.
- Tapping "Delete" shows a confirmation prompt: *"Delete [Building Name]? This cannot be undone."*
- On confirm, DELETE the building via the existing backend endpoint, remove it from the map, and remove the row from the list without a full page reload.
- This must work on both touch (mobile) and mouse (desktop drag) interactions.

---

## 4. Fix Dropdown Menu Issues

Audit and fix all dropdown/select menus throughout the module.

**Issues to resolve:**
- Dropdowns are currently not selectable — ensure click/tap correctly opens and selects options.
- Dropdown menus are rendering as underlays beneath other UI elements — fix z-index so dropdowns always appear on top of all other UI components (forms, maps, cards, etc.).
- Long option text must be horizontally scrollable within the dropdown so the full description is visible (do not truncate with ellipsis in a way that hides content permanently).
- Test all dropdowns: component taxonomy selectors (L1–L4), any status or type fields, and the cost library selectors.

---

## 5. Update Inspector Test Account Credentials

Locate the seeded inspector account with the current credentials below and update it:

| Field | Current value | New value |
|-------|---------------|-----------|
| Email / Login | `inspector@test.com` | `enquiries@fieldsadvisory.com.au` |
| Password | `inspector123` | `FA123` |

- Role must remain **Inspector** (sees buildings, no admin features).
- Update in both the database seed file and any hardcoded references in documentation or test helpers.
- Confirm the account can log in successfully with the new credentials after the change.
