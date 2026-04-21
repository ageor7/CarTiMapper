# CarTiMapper Changelog
All notable changes to this project will be documented in this file.

Changelog (v6.0.26)
GlobalStyles (v4.6.0): * [FIX: MAP-UI-02] Overrode default Leaflet Cluster CSS to deploy the Warm Segregation Palette.

[FIX: UI-TYPO-01] Reconfigured .slide-place-badge constraints to allow dynamic multi-line text wrapping that aligns cleanly with the standard .slide-desc left margin.

MapViewer (v3.14.0): * [FIX: MAP-UI-01] Restored native Leaflet zoom controls (topright) and the custom Floating Zoom Indicator.

[FIX: SUB-01] Added dash and ano teleia to the sublabels RegEx split logic.

[FIX: MAP-09] Enabled hot-swapping between the English Esri BaseMap and the Local CartoDB map based on application state.

[FIX: MAP-10] Implemented the Elevation hook. The active marker is dynamically removed from the cluster group to guarantee visibility.

[FIX: MAP-11] Bound getAllChildMarkers() to the cluster hover state to render a list of swallowed location names.

AppOrchestrator (v1.31.0): [FIX: MAP-SET-01] Injected maxAutoZoom and mapLocale state variables and built the corresponding dropdown interfaces in the Settings Modal to route data to the MapViewer.

---

Changelog Strategy (v6.0.25)
MapViewer (v3.13.0): * [FIX: MAP-UI-01] Restored native Leaflet zoom controls (topright) and the custom Floating Zoom Indicator synced to the map's native zoomend events.

[FIX: MAP-09] Swapped CartoDB light_all for Esri World Street Map to force standard English labeling globally.

[FIX: MAP-10] Implemented the Elevation hook. The active marker is dynamically removed from the cluster group and rendered on the root layer to ensure 100% visibility.

[FIX: MAP-11] Bound getAllChildMarkers() mapping logic to the cluster hover state to dynamically render a list of swallowed location names.

---

Changelog (v6.0.24)
AppOrchestrator (v1.30.1): [FIX: BOOT-CRASH-01] Patched a critical syntax typo in Stage 1 data fetch (new URLSearchParams). This resolves the catastrophic halting bug that permanently trapped users on the Initialization blank screen.

ContentSlider (v3.8.1): [FIX: UI-NAV-01] Restored the missing slideHistory and goBack properties to the component signature. The "Back" button successfully renders again inside the unified status bar.

TimelineScrubber (v23.5.0): * [FIX: TL-CLAMP-01] Deployed a mathematical span floor that forces the timeline to render a minimum of 24 hours.

[FIX: TL-CLAMP-02] Deployed an Infinity Clamp that locks the minimum time gap to 60 seconds, preventing divide-by-zero crashes when multiple events fall on the exact same chronological tick.

---

Changelog (v6.0.23)
AppOrchestrator (v1.30.1): [FIX: BOOT-CRASH-01] Patched a critical syntax typo in Stage 1 data fetch (new URLSearchParams). This resolves the catastrophic halting bug that permanently trapped users on the Initialization loading sequence.

ContentSlider (v3.8.1): [FIX: UI-NAV-01] Restored the missing slideHistory and goBack properties to the component signature. The "Back" button successfully renders again inside the unified status bar.

---

Changelog (v6.0.22)
ContentSlider (v3.8.0): * [FIX: UI-SET-01] Injected the Settings trigger button into the Status Bar.

[FIX: DATE-13] Upgraded formatSmartDate to conditionally render HH:mm on the narrative slide if an intra-day time is detected.

TimelineScrubber (v23.5.0): * [FIX: TL-CLAMP-01] Deployed a mathematical span floor that forces the timeline to render a minimum of 24 hours.

[FIX: TL-CLAMP-02] Deployed an Infinity Clamp that locks the minimum time gap to 60 seconds, preventing divide-by-zero crashes when multiple events fall on the exact same chronological tick.

AppOrchestrator (v1.30.0): * [FIX: ARCH-01] Implemented the Two-Stage Data Pipeline, caching raw PapaParse rows in memory.

[FIX: DATE-11 & 12] Replaced parseEuroDate with dynamicDateParse, which isolates HH:mm:ss payloads and mathematically routes date components based on the active format state.

[FIX: UI-SET-01] Injected the Settings Modal UI overlay.

---

Changelog (v6.0.21)
AppOrchestrator (v1.29.0): * [FIX: DATA-08] Replaced the partial-match smartGet algorithm with a high-performance exactGet mapper to strictly adhere to the defined Google Sheet schema.

[FIX: DATA-09] Activated extraction for media caption and media credit, binding them to the internal engine state so they render correctly in the MediaViewer carousel.

[FIX: DATA-10] Pruned unused fields (Source, Web Page) to optimize memory, and mathematically pointed the map's subLabels logic directly at the Place column for unified UI hover states.

---

Changelog (v6.0.18)
AppOrchestrator (v1.28.1): [FIX: DATA-03] Expanded normalization fallbacks for tags and media to catch variations like group and images. Implemented a rigorous .filter(t => t !== "") chain to obliterate falsy arrays and ghost lanes.

TimelineScrubber (v23.2.0): [FIX: UI-TL-02] Encapsulated the Absolute Zoom Controls and the Scroll Wrapper inside a singular strict parent container. This restores architectural integrity and prevents the flexbox engine from crushing the timeline to 0px.

---

Changelog / Commit Description: v6.0.17 Micro-Patch
MapViewer (v3.12.0): [FIX: GEO-01] Resolved GeoJSON coordinate inversion by deleting the custom coordsToLatLng override. The engine now trusts Leaflet's native parser to accurately process standard [Lon, Lat] GeoJSON outputs.

ContentSlider (v3.7.0): [FIX: DATE-02] Augmented the formatSmartDate logic to include the weekday: 'short' property, successfully restoring the Day of Week visibility across all narrative slides.

TimelineScrubber (v23.1.0): [FIX: UI-TL-01] Restored the complete multi-lane visualizer logic that was structurally truncated in the previous baseline. Redesigned the Zoom Controls container using isolated absolute positioning to prevent flexbox containment breaks, resolving the UI misalignment.
---

## [v6.0.16] - Micro-Patch 1: Date Parsing
###🛠️ Data Engine Fixes (index.html)
* Strict Euro/ISO Date Parser ([FIX: DATE-01]): Resolved the "Global Paradox" date inversion bug. Replaced the generic new Date() constructor with a manual parseEuroDate function. The engine now explicitly splits standard DD/MM/YYYY or DD-MM-YYYY formats and forces the array order to Day, Month, Year. This guarantees that US-localized browsers will no longer silently invert European dates (e.g., misreading August 5th as May 8th).
---

## [v6.0.15] - April 2026
### Code Engine (`index.html`)
* **Map Engine:** Removed the redundant `flipLeafletGeometry` function. Since v6.0.9's dependency fix, Wicket correctly handles OGC standards; the manual flipper was causing a re-inversion.
* **Map Engine:** Fixed Sub-Label mapping. Sub-labels now correctly iterate through flattened geometry layers even when nested in WKT LayerGroups.
* **UI/UX:** Increased Splash Screen logo width to 300px to match text width.
* **UI/UX:** Re-introduced the full-size branding logo into the Info/About modal.

---

## [v6.0.14] - April 2026
### Code Engine (`index.html`)
* **Map Engine:** Resolved the Multipoint Sub-Label duplication bug `[REF: MAP-06]`. Implemented recursive geometry tunneling (`extractLayers`) to shatter nested Wicket `LayerGroups` into flat arrays of individual pins. This prevents Leaflet from improperly cascading identical tooltips across entire spatial groups.
* **Map Engine:** Increased the vertical Y-offset of custom map tooltips from `-38px` to `-48px`, providing 10 additional pixels of "breathing room" to prevent the tooltip box from overlapping with the custom Map Pin UI.

---

## [v6.0.13] - April 2026
### UI/UX Engine
* **Map Engine:** Permanently resolved a double-offset CSS bug (`[REF: UI-12]`). Previously, overlapping negative margins between custom CSS and Leaflet's native `iconAnchor` pushed visual map pins away from their mathematical coordinates, causing Tooltips to improperly cover the icons and default to the right side.
* **Branding Layout:** Replaced the legacy text button in the status bar with the sleek new CarTiMapper Logo icon, fully integrated into the UI. The large logo block has been removed from the Content Slider header, reclaiming massive vertical reading space for text.
* **Typography Hierarchy:** Moved the `📍 Place` metadata badge to the right-hand column beneath the Tags `[REF: UI-13]`. Stripped its bold blue background and applied the gray, pill-shaped aesthetic of the Tag badges to create a fully unified spatial metadata aesthetic.

---

## [v6.0.12] - April 2026
### Code Engine (`index.html`)
* **Branding:** Officially integrated the final CarTiMapper branding system. 
* **Design Metaphor:** Replaced the legacy text title with the new Dark Film Hex-Frame logo—combining a 16:9 cinematic frame with pure white sprockets (Carousel), an off-axis timeline label shape (Timeline), and a glowing Chrono-Compass dial featuring Engine Blue and Active Green map pins as clock hands (Mapper).
* **UI/UX:** Dynamically embedded the engine version (`v6.0.12`) natively into the splash screen loader.
* **Favicon:** Replaced the generic icon lookup with a pure, URL-encoded Hex-Frame SVG directly inside the HTML `<head>`.

---

## [v6.0.11] - April 2026
### Code Engine (`index.html`)
* **Map Engine:** Deployed hotfix for a fatal `ReferenceError` (`new URLSearchParams` typo) in the `MapViewer` component that was preventing the application from mounting, resulting in a blank screen.

---

## [v6.0.10] - April 2026
### Code Engine (`index.html`)
* **Data Schema:** Expanded the CSV ingestor to capture the explicit `Place` column (Column J) from the upstream Google Sheet. 
* **Map Engine:** Overhauled Map Tooltip UI. Instead of blindly pasting full, paragraph-length Event Titles over single pins, the engine now utilizes a strict labeling hierarchy: `Sub-Label` (if MultiPoint) -> `Place Name` (e.g., "Omaha Beach") -> `Event Title` (Fallback).
* **UI/UX:** Added a dedicated `📍 [Place Name]` spatial context badge directly into the Content Slider layout beneath the active date.

---

* ## [v6.0.9] - April 2026
### Code Engine (`index.html`)
* **Architecture:** Bumped `APP_VERSION` to `v6.0.9`.
* **Map Engine:** Diagnosed and fixed the catastrophic WKT inversion bug. The core `wicket.min.js` library was omitted from the `<head>` in `v6.0.0`, causing silent failures that forced the engine to rely on a primitive Regex fallback (which dutifully inverted OGC standard coordinates). Restoring the core library allows native, flawless WKT-to-Leaflet translation.
* **Architecture (URL):** Fixed the "Sequence Initiated" boot freeze. The engine now defaults to the Master Database URL if `?source=` is omitted from the browser address bar.
* **Diagnostics:** Refined `VibeMonitor` telemetry. If a WKT string fails to parse, the execution block now catches and explicitly logs the stack trace instead of silently reverting to standard Regex pins.

---

## [v6.0.8] - April 2026
### Code Engine (`index.html`)
* **Architecture:** Bumped `APP_VERSION` to `v6.0.8`.
* **Map Engine:** Deployed `[REF: MAP-05]` Recursive Leaflet Geometry Flipper (`flipLeafletGeometry`). Intercepts `wkt.toObject()` arrays and safely iterates through deep Polylines and Polygons to forcibly mutate `layer.setLatLngs()` in Leaflet memory, permanently resolving the `Wicket` translation inversion.
* **Diagnostics:** Wired the map parser directly into the `VibeMonitor`. The engine now outputs verbose telemetry on the very first WKT object it reads (Raw String -> Wicket Translation -> Post-Flip Correction) to prove spatial accuracy.

---

## [v6.0.7] - April 2026
### Code Engine (`index.html`)
* **Architecture:** Bumped `APP_VERSION` to `v6.0.7`.
* **Map Engine:** Removed the `v6.0.6` Regex coordinate flipper. Because the upstream Google Sheets ETL (`v6.0.3`) guarantees strict OGC standard WKT (`POINT(Lon Lat)`), `Wicket` parses and translates the coordinates into Leaflet natively without requiring manual manipulation (Note: Replaced by deeper memory intervention in v6.0.8).
* **UI/UX:** Added `?source=URL` parameter documentation into the "About" modal. Clarified standard web URL syntax for end-users, explaining that query chains must be linked using ampersands (`&`) rather than multiple question marks.

---

## [v6.0.6] - April 2026
### Code Engine (`index.html`)
* **Architecture:** Bumped `APP_VERSION` to `v6.0.6`.
* **Map Engine:** Deployed an ultra-fast Regex Pre-Parser string flipper (`locStr.replace(/.../g, '$2 $1')`) to mathematically intercept and swap WKT coordinates (Reverted in v6.0.7).
* **UI/UX:** Rebuilt the `about-modal` JSX to render a stylized, technical reference card explaining all active `[REF: URL-01]` routing parameters.

### Master Blueprint
* **Documentation:** Added requirement to mirror all active URL Routing parameters inside the frontend Info Window modal for end-user visibility.

---

## [v6.0.5] - April 2026
### Code Engine (`index.html`)
* **Architecture:** Bumped `APP_VERSION` to `v6.0.5`. Updated "About" button UI to explicitly display `ℹ️ CarTiMapper v6.0.5`.
* **Navigation:** Built `[REF: URL-01]` URL Routing Engine. App now intercepts `?slide=X`, `?date=YYYY-MM-DD`, `?theme=dark`, and `?mapzoom=X` via `URLSearchParams` to instantly override default UI states and calculate the closest chronological slide for immediate deep-linking.

### Master Blueprint
* **Architecture:** Formally registered the URL Routing Engine (`[REF: URL-01]`) under Section VI. Algorithms, Analytics & Methodologies.

---

## [v6.0.4] - April 2026
### Code Engine (`index.html`)
* **Architecture:** Bypassed Wicket `.toObject()` inversion bug by routing parsed WKT through `.toJson()` into Leaflet's highly compliant `.geoJSON()` engine natively (Reverted in v6.0.7).
* **UI/UX:** Added `.custom-div-icon` CSS override to strip away Leaflet's forced white bounding boxes on SVG pins.
* **UI/UX:** Repositioned map tooltips strictly above the pin head `[0, -38]` using `.centered-tooltip`.
* **Physics:** Hardened spatial math. Raised bounding box max-zoom tolerances to `16`, allowing the camera to reach Acropolis/neighborhood-level fidelity in dense clusters. Added live zoom-state UI tracker.

---

## [v6.0.3] - April 2026
### Google Sheets ETL Pipeline
* **Architecture:** Officially brought the upstream Google Sheets ETL formula under the CarTiMapper versioning umbrella (`v6.0.3`).
* **Logic:** Removed temporary Lat/Lon flip. Enforced strict OGC Cartesian schema (`POINT(Lon Lat)`) for all WKT generation to ensure downstream compatibility with QGIS/PostGIS.
* **Documentation:** Bypassed Google Sheets' lack of native comment support by utilizing `LET` dummy variables (`_ref_etl`) to securely inject Two-Tier REF-TAG documentation directly into the spreadsheet logic stream.

### Master Blueprint
* **Architecture:** Authored **Section IV: Data Schema & The Upstream ETL Pipeline**. Documented the "Cartographer's Dilemma" (`[REF: DATA-02]`) and Spatial Deduplication (`[REF: DATA-03]`).

---

## [v6.0.2] - April 2026
### Google Sheets ETL Pipeline
* **Logic:** Wrapped the GeoJSON and WKT array builders in `UNIQUE(FILTER(...))` logic. The upstream database will now mathematically purge redundant coordinate nodes before casting strings into collections, drastically reducing DOM rendering loads.

---

## [v6.0.1] - April 2026
### Code Engine (`index.html`)
* **UI/UX:** Reassigned mapping hierarchy colors (Inactive = Blue, Active = Green, VIP = Gold).
* **Data Schema:** "Smartened" Google Sheets `col_idx=11` formula. Now intelligently detects legacy GeoJSON vs WKT elements, safely maps raw Lat/Lon, and merges into proper GeoJSON `GeometryCollection` or WKT `GEOMETRYCOLLECTION` conditionally. Solves `MULTIPOINT` parsing bug.

---

## [v6.0.0] - April 2026
### Code Engine (`index.html`)
* **Architecture:** Bumped major version to `v6.0.0`. Replaced basic Leaflet plotting with an enterprise-grade spatial engine utilizing Well-Known Text (WKT) parsing (`Wicket`) and R-Tree spatial indexing (`MarkerCluster`).
* **Logic:** Built an `O(1)` memory dictionary (`markersRef`) to handle "State-Based Layer Swapping" for instantaneous dynamic VIP slide focus, pulling active narrative pins out of background clusters.

### Master Blueprint
* **Documentation:** Formally established the Two-Tier Documentation Protocol. Injected `[REF: TAG-X]` semantic anchors across the entire codebase to permanently link inline logic with the Master Blueprint.

---

## [v5.23.0] - April 2026
### Code Engine (`index.html`)
* **Data Schema:** Updated the `PapaParse` fuzzy ingestor to capture the new upstream ETL parameters (`clean_geo`, `sub_labels`, `priority`).
* **Media:** Hardened the Media Agnosticism pipeline to natively support YouTube iframes, HTTP links, and standard images within a unified flex container.

---

## [v1.0.0 – v5.13.0] - Legacy Foundation Rollup
*(Note: Granular commit history prior to v5.23.0 is consolidated to reflect the architectural baseline established in Blueprint v5.13.0).*
* **Core Layout:** Established strict Flexbox boundaries with a Unified Status Bar and 15% Ergonomic Touch Zones for mobile navigation.
* **Timeline
