# CarTiMapper Engine Master Blueprint
**Version:** v6.0.5 | **Date:** April 2026

## I. Engineering Protocols & Coding Rules

1. **[REF: DOC-01] Two-Tier Documentation (REF-TAGS):** Documentation is strictly bifurcated. The Master Blueprint contains the "Why" and "What" (Architecture, UI/UX Rules, Physics). The inline code comments contain the "How" (Mechanics, Loops, Variables). They are permanently interlinked using `[REF: TAG-NAME]` semantic anchors. **Zero duplication.**
2. **Spreadsheet REF-TAGS:** Because Google Sheets does not support native code comments, REF-TAGs are securely injected into the ETL formula using `LET()` dummy variables (e.g., `_ref_map01, "[REF: MAP-01]..."`) to preserve the documentation pipeline without breaking mathematical execution.
3. **Discuss First, Implement Later:** All bugs, physics, and UI/UX behaviors must be mathematically diagnosed and agreed upon before any code is generated.
4. **Strict Bottom-to-Top Patching:** To protect line structure and prevent cascading offsets, patches are always delivered in strictly reverse order (from the bottom of `index.html` to the top). The typical sequence is: `AppOrchestrator -> TimelineScrubber -> ContentSlider -> Global Variables -> Global Styles`.
5. **Semantic Boundary Replacements:** We rely exclusively on the indestructible `// === [ BLOCK NAME vX.X.X ] ===` boundary markers instead of fragile, hardcoded line numbers.
6. **No Default Bold/Capitalize:** We do not enforce `font-weight: bold` or `text-transform: uppercase` on narrative text through global CSS. The interface relies entirely on explicit manual HTML tags (e.g., `<b>`, `<strong>`) for emphasis.
7. **True Font Loading:** When utilizing Google Fonts (especially for Greek Polytonic characters), we explicitly download all Regular (400) and Bold (700) weights and Italics etc. variations to ensure the browser never generates jagged "faux-bolds", "faux-italics" etc.

---

## II. Active Requirements Matrix

### Met Requirements
* **[REF: ETL-01] Upstream Spatial ETL (Google Sheets):** The Google Sheets database acts as the primary translation engine, compiling raw data into mathematically pure WKT and GeoJSON arrays.
* **[REF: MAP-01] The WKT Mapping Engine:** Map natively parses Well-Known Text (WKT) via the `Wicket` library. Falls back safely to GeoJSON or standard Lat/Lon pairs.
* **[REF: MAP-02] Spatial Decluttering (Clustering):** The map utilizes an R-Tree spatial index (`MarkerCluster`) to group overlapping geographic elements into numbered bubbles based on a 40px screen radius.
* **[REF: MAP-03] Dynamic VIP Slide Focus:** Active narrative elements are mathematically extracted from the cluster bucket, styled as "Fire Pins" (`z-index: 1000`), and dropped back into the background when the user leaves the slide.
* **Layout Hardening:** Map/Text/Media panes perfectly respect Flexbox boundaries.
* **Smart Auto-Zoom (Timeline):** The timeline mathematically prevents text collisions while preserving dataset context.
* **Ergonomic Touch Navigation:** The text pane supports frictionless Left/Right tap-to-navigate zones (15% width).
* **Media Agnosticism:** Media pane natively renders images, YouTube iframes, Google Maps iframes, and raw HTTP links.

### Pending / Future Considerations
1. **Layout Memory:** Implement local-storage to save user-defined pane layouts/resizing states.
2. **URL Parameter Expansion:** Parse CLI/URL parameters like `?zoom=12` and `?theme=dark`.
3. **Directional Vectors:** Investigate adding direction arrows to line vectors (Trade routes/Troop Marches).
4. **Duration on X-Axis:** Display (Start - End) times on the X-axis for the currently active event without overcrowding.

---

## III. Core Architecture: The Mapping Engine (The "Why" and "What")

* **[REF: MAP-01] The WKT Fallback Engine:** Standard Lat/Lon and GeoJSON cannot natively group multiple distinct geographic shapes into a single narrative slide. We utilize Well-Known Text (WKT) via `Wicket` because it supports strict GIS `MultiPoint` and `GeometryCollection` definitions, allowing us to map complex, distributed events as a single database row.
* **[REF: MAP-02] Spatial Decluttering (MarkerCluster):** As the database grows, distant map elements mathematically overlap on screen. The R-Tree clustering plugin solves UI clutter without deleting data by dynamically merging pins based on zoom level.
* **[REF: MAP-03] Dynamic VIP "Slide Focus":** The map utilizes a "State-Based Layer Swapping" architecture. It builds an `O(1)` memory dictionary on load. On slide change, it uses the dictionary to instantly locate the active marker, pull it from the background cluster, elevate its Z-index, and restyle it, ensuring the active narrative is never hidden.
* **[REF: MAP-04] The Parallel Array (Sub-Labels):** When a `MultiPoint` contains multiple locations, a single label is insufficient. The engine reads a pipe-delimited string (`Camp 1 | Camp 2`) and mathematically aligns each substring to its respective point in the geometry array.

---

## IV. Data Schema & The Upstream ETL Pipeline 

* **[REF: ETL-01] Purpose of the ETL Formula:** The frontend JavaScript engine should *render* data, not *clean* it. The Google Sheets `LET()` formula acts as a mission-critical compiler. It intercepts messy human spreadsheet inputs, formats HTML hyperlinks, deduplicates categorical tags, and natively compiles all raw geographic data into valid, machine-readable JSON or WKT before the frontend ever sees it.
* **[REF: DATA-02] The Cartographer's Dilemma (Coordinate Schema Rule):** Standard navigational systems (Google Maps, humans) read coordinates as **[Latitude, Longitude]**. However, strict mathematical GIS standards (OGC, GeoJSON, WKT) operate on a Cartesian graph and absolutely demand **[Longitude, Latitude]** (X, Y).
    * **The Rule:** The database must never be corrupted to appease a frontend quirk. Users will input `Lat, Lon` into the spreadsheet, but the ETL formula MUST act as the translator, natively outputting `POINT(Lon Lat)` for WKT and `[Lon, Lat]` for GeoJSON. This guarantees the CarTiMapper CSV export remains completely interoperable with enterprise software like QGIS and PostGIS.
* **[REF: DATA-03] Spatial Deduplication:** When a single narrative event aggregates multiple spreadsheet rows of the same physical location, generating overlapping pins breaks Z-index visuals and overloads the DOM. The ETL pipeline wraps all geometry array builders in a `UNIQUE()` matrix, mathematically purging redundant geographic nodes *before* casting them into `GEOMETRYCOLLECTION` strings.

---

## V. UI/UX Elements & Design Solutions

* **The 15% Ergonomic Touch Zones:** Placed invisible 15% width `.click-zone` overlays on the left/right edges for mobile thumbs, preserving a 70% "safe-scrolling highway" in the center.
* **The Unified Status Bar Centering:** Applied strict Flexbox `flex: 1` geometry to Left, Center, and Right zones to mathematically force the `Prev/Next` buttons to absolute screen center.
* **The Staged Determinate Loading Screen:** Built a branded UI overlay with a linear progress bar driven by physical engine states (10% Initializing ➔ 100% Success) with a 500ms fade-out, eliminating "frozen app" anxiety.
* **Tag Lane "Sticky" Shielding:** Detached swimlanes from `overflow: hidden` wrappers and explicitly anchored their bottom bounds to `bottom: 28px` to prevent downward bleeding into the X-Axis.
* **Typography Hierarchy:** `EB Garamond` (or similar serif) used for Titles/Descriptions. `Fira Sans` (or similar sans-serif) used for UI elements.
* **[REF: UI-05] Visual Hierarchy (Map Pins):** Permanent geographical anchors (VIPs) bypass the cluster engine and render as distinct Gold pins. Active slide elements render as oversized Green pins. Inactive clustered elements render as standard Blue pins.

---

## VI. Algorithms, Analytics & Methodologies

* **1. Spatial Indexing (The R-Tree Engine):** Rather than measuring point-to-point distance (which freezes the browser), `MarkerCluster` uses `RBush` to divide the map into nested rectangular grids. It measures point-to-grid, allowing instant, `O(log n)` collision detection.
* **2. State-Based Layer Swapping & The O(1) Dictionary:** To bypass Leaflet's internal `_leaflet_id` amnesia, the engine builds a global dictionary (`markersRef`). Slide changes execute an `O(1)` instant lookup to swap layers in and out of the cluster bucket seamlessly.
* **3. The Dual-Heuristic Density Math (Timeline Auto-Zoom):** Solves the "Outer Space" problem by evaluating two vectors: `collisionZoom` (The 10px Rule for preventing overlap on the same lane) and `contextZoom` (The 150px Rule for keeping the nearest event visible). Executes `Math.max()` to adopt the safest zoom.
* **4. The URL Routing Engine:** `[REF: URL-01]` Upon data load, the engine intercepts CLI/URL parameters. If `?date=YYYY-MM-DD` is provided, it executes a mathematical distance calculation across the entire `valid` chronological array to locate the closest absolute time-node and injects it as the `activeIndex`. `?slide=X` overrides chronology to focus on a specific narrative slide. `?theme=dark` manipulates root CSS properties prior to physical DOM render.
* **5. The Visual-Center Scrolling Algorithm:** Calculates `blockPixelWidth` dynamically. Plots the geometric center (`StartPx + Width/2`). Commands the scrollbar to target `visualCenterPx - (Screen Width / 2)` to perfectly center wide text blocks.
* **6. HTML Filter Bifurcation:** Timeline Hexagons are processed through `stripHTML()` to guarantee layout breaks (`<br>`) do not physically break geometric rendering. Content Pane Titles are processed through `dangerouslySetInnerHTML` to permit manual formatting.
