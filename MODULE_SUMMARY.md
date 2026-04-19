# AM Data Collection: Buildings — Module Summary

**Slug:** `am-data-collection-buildings`
**Backend:** `/api/modules/am-data-collection-buildings/*`
**Frontend:** `/module/am-data-collection-buildings/*`

---

## Purpose

Field-inspector workflow for capturing building asset data on-site: walk a property, list its buildings, break each building down into priced components, attach geo-tagged photos, and export the result. Built mobile-first as an installable PWA so it works on a phone with no signal.

---

## What it does today (Phases 1–3, all merged)

### Buildings
- CRUD for buildings under a job. Each building has location (lat/lng), site metadata, and a version for safe concurrent edits.
- Map view (Leaflet + Esri imagery) showing all buildings on a job.

### Components
- Each building has a list of priced components, classified by a four-level taxonomy (L1 → L4).
- Optimistic concurrency: stale PATCH returns `409 { currentVersion }`; the UI re-fetches and lets the user retry.
- Free-text fields use an autofill input backed by a vocabulary endpoint that ranks suggestions by frequency (per-building first, then portfolio-wide).

### Cost Library (admin)
- Admin/reviewer-only CRUD screen for the L1–L4 priced catalog that components draw from.
- Inspectors don't see the nav entry; direct URL hits a friendly "Restricted" fallback. Backend writes are gated by `requireRole(["admin","reviewer"], ...)`.

### Photos
- Capture-from-camera or upload, stored via App Storage, geo-tagged where the device permits.

### Vocabulary
- `/vocabulary` endpoint serving frequency-ranked term suggestions per field, scoped to the current building first.

### Exports
- Endpoint to export a job's buildings + components for downstream reporting.

### Offline + sync
- Reads wrap a generated React Query hook with `useCachedQuery`, persisting to IndexedDB.
- Writes go through a sync queue that retries when the network returns; the offline indicator is sticky in the UI.

### Mobile polish (§13)
- All form grids are `grid-cols-1 sm:grid-cols-2`, all interactive controls are `≥44px` (`h-11`), no horizontal scroll at `375×812`, every interactive element carries a `data-testid`.

---

## Reference paths

| Layer | Path |
|-------|------|
| Backend | `artifacts/api-server/src/modules/am-data-collection-buildings/` (routes: buildings, components, photos, costLibrary, vocabulary, exports) |
| Frontend | `artifacts/field-day/src/modules/am-data-collection-buildings/` (pages/, components/AutofillInput.tsx, offline/{db,useCachedQuery,sync}.ts, lib/constants.ts) |
| Spec | `lib/api-spec/openapi.yaml` — search for the `amBuildings` tag |

---

> This module is the canonical reference for every pattern in the module guide — when adding a new module, copy from here.
