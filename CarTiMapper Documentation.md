# CarTiMapper Engine Master Blueprint
**Version:** v6.0.6| **Date:** April 2026

## I. Engineering Protocols & Coding Rules

1. **[REF: DOC-01] Two-Tier Documentation (REF-TAGS):** Documentation is strictly bifurcated. The Master Blueprint contains the "Why" and "What" (Architecture, UI/UX Rules, Physics). The inline code comments contain the "How" (Mechanics, Loops, Variables). They are permanently interlinked using `[REF: TAG-NAME]` semantic anchors. **Zero duplication.**
2. **Spreadsheet REF-TAGS:** Because Google Sheets does not support native code comments, REF-TAGs are securely injected into the ETL formula using `LET()` dummy variables (e.g., `_ref_map01, "[REF: MAP-01]..."`) to preserve the documentation pipeline without breaking mathematical execution.
3. **Discuss First, Implement Later:** All bugs, physics, and UI/UX behaviors must be mathematically diagnosed and agreed upon before any code is generated.
4. **Strict Bottom-to-Top Patching:** To protect line structure and prevent cascading offsets, patches are always delivered in strictly reverse order (from the bottom of `index.html` to the top). The typical sequence is: `AppOrchestrator -> TimelineScrubber -> ContentSlider -> Global Variables -> Global Styles`.
5. **Semantic Boundary Replacements:** We rely exclusively on the indestructible `// === [ BLOCK NAME vX.X.X ] ===` boundary markers instead of fragile, hardcoded line numbers.
6. **No Default Bold/Capitalize:** We do not enforce `font-weight: bold` or `text-transform: uppercase` on narrative text through global CSS. The interface relies entirely on explicit manual HTML tags (e.g., `<b>`, `<strong>`) for emphasis.
7. **True Font Loading:** When utilizing Google Fonts (especially for Greek Polytonic characters), we explicitly download all Regular (400) and Bold (700) weights and Italics etc. variations to ensure the browser never generates jagged "faux-bolds", "faux-italics" etc.
* **[REF: ARCH-01] Two-Stage Data Pipeline:** To support dynamic on-the-fly re-rendering without network latency, the engine strictly separates raw data ingestion (`rawCsvRows`) from the compiled data state (`data`).

*(Added following v6.0.15 Audit)*

7. **[REF: DOC-02] Architectural Block Taxonomy:** To eliminate cascading errors and enable risk-free surgical patching, the codebase is strictly categorized into visual and logical boundaries.
    * **Major Blocks:** High-level components (e.g., AppOrchestrator, GlobalStyles). Must carry explicit version numbers. 
        * *Syntax:* `// === [ MAJOR BLOCK: ComponentName vX.X.X ] ===`
    * **SubBlocks:** Discrete chunks of logic within a Major Block (e.g., parsers, rendering loops, state management).
        * *Syntax:* `// --- [ SubBlock: LogicDescription ] ---`

8. **[REF: VER-01] Strict Semantic Versioning:** Version numbers are manually tracked on both the global app and individual Major Blocks.
    * **Feature Directives:** Increment the MINOR version and reset the patch (e.g., `v6.0.15` → `v6.1.0`).
    * **Debugging & Hotfixes:** Increment the PATCH version by one (e.g., `v6.0.15` → `v6.0.16`).

9. **[REF: DOC-03] GitHub Artifact Generation:** Every code alteration requires explicit Markdown artifacts prior to code delivery:
    * **Commits & Changelogs:** Generated for every code change to track debugging history.
    * **Master Blueprint Updates:** Generated for any design changes, new directives, or architectural shifts to maintain the global "Source of Truth."
* **[REF: DOC-02] Architectural Block Taxonomy:** The codebase is now fully wrapped in semantic tags.
    * *Major Blocks:* `// === [ MAJOR BLOCK: Name vX.X.X ] ===`
    * *SubBlocks:* `// --- [ SubBlock: LogicDescription ] ---`
* **[REF: VER-01] Strict Semantic Versioning:** Enabled. This baseline is established as **v6.0.17**.

---

## II. Active Requirements Matrix

### Met Requirements
* **[REF: ETL-01] Upstream Spatial ETL: (Updated) The engine now relies on a compulsory source URL parameter for initialization.
* **[REF: MAP-01] The WKT Mapping Engine:** Map natively parses Well-Known Text (WKT) via the `Wicket` library. Falls back safely to GeoJSON or standard Lat/Lon pairs.
* **[REF: MAP-02] Spatial Decluttering (Clustering):** The map utilizes an R-Tree spatial index (`MarkerCluster`) to group overlapping geographic elements into numbered bubbles based on a 40px screen radius.
* **[REF: MAP-03] Dynamic VIP Slide Focus:** Active narrative elements are mathematically extracted from the cluster bucket, styled as "Fire Pins" (`z-index: 1000`), and dropped back into the background when the user leaves the slide.
* **[REF: MAP-05] Linear Sub-Label Mapping: (New) MultiPoint geometries are recursively flattened into a linear array to ensure 1:1 parity with the pipe-delimited sub-label string.
* **[REF: URL-01] Deep-Linking Engine: Fully supports ?source=, ?slide=, ?date=, ?theme=, and ?mapzoom=.* **Layout Hardening:** Map/Text/Media panes perfectly respect Flexbox boundaries.
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

* **[REF: MAP-01] WKT Engine: (Updated) Removed redundant coordinate inversion logic. The engine trusts the upstream ETL output of POINT(Lon Lat) to maintain QGIS/PostGIS interoperability.
* **[REF: MAP-02] Spatial Decluttering (MarkerCluster):** As the database grows, distant map elements mathematically overlap on screen. The R-Tree clustering plugin solves UI clutter without deleting data by dynamically merging pins based on zoom level.
* **[REF: MAP-03] Dynamic VIP "Slide Focus":** The map utilizes a "State-Based Layer Swapping" architecture. It builds an `O(1)` memory dictionary on load. On slide change, it uses the dictionary to instantly locate the active marker, pull it from the background cluster, elevate its Z-index, and restyle it, ensuring the active narrative is never hidden.
* **[REF: MAP-04] Parallel Array (Sub-Labels): (Updated) Switched to a Strict Linear Map algorithm. If 3 geometries and 3 sub-labels are present, the engine mathematically locks them to prevent label duplication across LayerGroups.
* **[REF: MAP-07] Tooltip Physics: Tooltips are anchored with a -48px vertical offset to provide aesthetic breathing room above the Map Pin head.
  
---

## IV. Data Schema & The Upstream ETL Pipeline 

* **[REF: ETL-01] Purpose of the ETL Formula:** The frontend JavaScript engine should *render* data, not *clean* it. The Google Sheets `LET()` formula acts as a mission-critical compiler. It intercepts messy human spreadsheet inputs, formats HTML hyperlinks, deduplicates categorical tags, and natively compiles all raw geographic data into valid, machine-readable JSON or WKT before the frontend ever sees it.
* **[REF: DATA-02] The Cartographer's Dilemma (Coordinate Schema Rule):** Standard navigational systems (Google Maps, humans) read coordinates as **[Latitude, Longitude]**. However, strict mathematical GIS standards (OGC, GeoJSON, WKT) operate on a Cartesian graph and absolutely demand **[Longitude, Latitude]** (X, Y).
    * **The Rule:** The database must never be corrupted to appease a frontend quirk. Users will input `Lat, Lon` into the spreadsheet, but the ETL formula MUST act as the translator, natively outputting `POINT(Lon Lat)` for WKT and `[Lon, Lat]` for GeoJSON. This guarantees the CarTiMapper CSV export remains completely interoperable with enterprise software like QGIS and PostGIS.
* **[REF: DATA-03] Spatial Deduplication:** When a single narrative event aggregates multiple spreadsheet rows of the same physical location, generating overlapping pins breaks Z-index visuals and overloads the DOM. The ETL pipeline wraps all geometry array builders in a `UNIQUE()` matrix, mathematically purging redundant geographic nodes *before* casting them into `GEOMETRYCOLLECTION` strings.
* **[REF: DATA-05] Direct Key Mapping: Abandoned fuzzy header searching in favor of a Case-Independent Normalization layer. All spreadsheet headers are lowercased and trimmed upon ingest, permitting human-variable entries like "Description" or "DESCRIPTION" without breaking the code contract.
* **[REF: DATA-06] Strict Euro-Date Logic: The engine executes a manual split on date strings to prioritize Day/Month/Year formats, specifically mitigating the "Global Paradox" where browsers misinterpret European dates as US formats.
* **[REF: DATA-05] Direct Key Mapping:** Data is normalized via lowercase `norm` object mapping. Legacy `fuzzy()` logic is deprecated.
* **[REF: DATA-06] Strict Euro-Date Logic:** `parseEuroDate` forces `DD/MM/YYYY` parsing to prevent US-browser inversion.
* **[REF: MAP-08] Standard GeoJSON Conformity:** The `MapViewer` relies purely on Leaflet's native parser for GeoJSON objects. Custom coordinate inversion overrides are strictly prohibited to ensure parity with standard PostGIS/QGIS `[Lon, Lat]` exports.
* **[REF: DATA-08] Strict Schema Contract (`exactGet`):** The engine operates on a locked spreadsheet schema. To prevent string collisions (e.g., fetching "Media" vs "Media Caption") and minimize memory overhead, data extraction relies on strict, case-normalized 1:1 key mapping (`exactGet`).
* **[REF: DATA-09] Memory Pruning:** Extraneous spreadsheet columns not utilized by the engine (e.g., `Web Page`, `Source`, `Source URL`) are intentionally omitted from the data object mapping to minimize JSON payload size and optimize browser memory retention.
* **[REF: DATA-10] Place/SubLabel Convergence:** The legacy `subLabels` metadata requirement for Map Hover Tooltips is mathematically bound to the `Place` column, enforcing single-source-of-truth location labeling.
* **[REF: DATE-11] Time Preservation:** The temporal parser must explicitly decouple `HH:mm:ss` payloads from date strings using whitespace isolation. Stripping time signatures is strictly prohibited to prevent `00:00:00` collision bugs in the Timeline zoom physics.
* **[REF: DATE-12] Dynamic Temporal Locale:** The parser dynamically routes matrix array indices to Year/Month/Day components based on user-defined UI state, allowing instant hot-swapping between European (`DD/MM`) and US (`MM/DD`) spreadsheet exports.
---

## V. UI/UX Elements & Design Solutions

* **The 15% Ergonomic Touch Zones:** Placed invisible 15% width `.click-zone` overlays on the left/right edges for mobile thumbs, preserving a 70% "safe-scrolling highway" in the center.
* **The Unified Status Bar Centering:** Applied strict Flexbox `flex: 1` geometry to Left, Center, and Right zones to mathematically force the `Prev/Next` buttons to absolute screen center.
* **The Staged Determinate Loading Screen:** Built a branded UI overlay with a linear progress bar driven by physical engine states (10% Initializing ➔ 100% Success) with a 500ms fade-out, eliminating "frozen app" anxiety.
* **Tag Lane "Sticky" Shielding:** Detached swimlanes from `overflow: hidden` wrappers and explicitly anchored their bottom bounds to `bottom: 28px` to prevent downward bleeding into the X-Axis.
* **Typography Hierarchy:** `EB Garamond` (or similar serif) used for Titles/Descriptions. `Fira Sans` (or similar sans-serif) used for UI elements.
* **[REF: UI-05] Visual Hierarchy (Map Pins):** Permanent geographical anchors (VIPs) bypass the cluster engine and render as distinct Gold pins. Active slide elements render as oversized Green pins. Inactive clustered elements render as standard Blue pins.
* **Info Window URL Reference:** To ensure end-user visibility of deep-linking capabilities, the frontend "About" modal must explicitly mirror and document all active `[REF: URL-01]` routing parameters (e.g., `?slide=`, `?date=`, `?theme=`, `?mapzoom=`) as a stylized technical reference card.
* **[REF: UI-08] Multi-Context Branding: The brand identity is separated into a Logomark (The Hex-Frame Chrono-Compass) and Logotype (CarTiMapper).
  * Splash Context: Large vertical stack (300px) featuring the engine version.
  * Status Context: 24px micro-icon in the status bar to maximize content real estate.
  * About Context: High-fidelity branding center with a blur-backdrop and "Click-Outside-to-Close" behavior.
* **[REF: UI-13] Metadata Hierarchy: The 📍 Place badge is right-aligned beneath the Tags, utilizing a unified, non-bold, pill-shaped aesthetic to distinguish metadata from narrative text.
* **[REF: UI-14] Safe History Navigation:** Deep-linking and manual jumps push to a passive `slideHistory` state array, allowing safe "Back" functionality without corrupting child component prop signatures.
* **[REF: UI-15] Pin Geometry Anchoring:** Map pins strictly enforce `position: relative` to prevent absolute-positioned heads and stems from decoupling during zoom physics.
* **[REF: UI-SET-01] Engine Settings Modal:** A unified settings interface (accessible via the Status Bar ⚙️) provides explicit control over engine-level physics, such as data locale formats.
* **[REF: MAP-09] Global Locale Enforcement:** The `MapViewer` TileLayer explicitly utilizes English-enforced raster tiles to ensure global readability, bypassing native local-language tile renders.
* **[REF: MAP-10] Active State Elevation (Unclustering):** The visual engine must guarantee spatial visibility of the `activeIndex`. Active markers are programmatically detached from the `MarkerClusterGroup` and elevated to the root map layer to prevent them from being hidden inside cluster aggregates.
* **[REF: MAP-11] Cluster Metadata Aggregation:** Map clusters utilize dynamic event listeners (`clustermouseover`) to aggregate and display a list of child-marker tooltips upon user hover, providing immediate context into collapsed regions.
---

## VI. Algorithms, Analytics & Methodologies

* **1. Spatial Indexing (The R-Tree Engine):** Rather than measuring point-to-point distance (which freezes the browser), `MarkerCluster` uses `RBush` to divide the map into nested rectangular grids. It measures point-to-grid, allowing instant, `O(log n)` collision detection.
* **2. State-Based Layer Swapping & The O(1) Dictionary:** To bypass Leaflet's internal `_leaflet_id` amnesia, the engine builds a global dictionary (`markersRef`). Slide changes execute an `O(1)` instant lookup to swap layers in and out of the cluster bucket seamlessly.
* **3. The Dual-Heuristic Density Math (Timeline Auto-Zoom):** Solves the "Outer Space" problem by evaluating two vectors: `collisionZoom` (The 10px Rule for preventing overlap on the same lane) and `contextZoom` (The 150px Rule for keeping the nearest event visible). Executes `Math.max()` to adopt the safest zoom.
* **4. The URL Routing Engine:** `[REF: URL-01]` Upon data load, the engine intercepts CLI/URL parameters. If `?date=YYYY-MM-DD` is provided, it executes a mathematical distance calculation across the entire `valid` chronological array to locate the closest absolute time-node and injects it as the `activeIndex`. `?slide=X` overrides chronology to focus on a specific narrative slide. `?theme=dark` manipulates root CSS properties prior to physical DOM render.
* **Info Window URL Reference:** To ensure end-user visibility of deep-linking capabilities, the frontend "About" modal must explicitly mirror and document all active `[REF: URL-01]` routing parameters (e.g., `?slide=`, `?date=`, `?theme=`, `?mapzoom=`) as a stylized technical reference card.* **5. The Visual-Center Scrolling Algorithm:** Calculates `blockPixelWidth` dynamically. Plots the geometric center (`StartPx + Width/2`). Commands the scrollbar to target `visualCenterPx - (Screen Width / 2)` to perfectly center wide text blocks.
* **6. HTML Filter Bifurcation:** Timeline Hexagons are processed through `stripHTML()` to guarantee layout breaks (`<br>`) do not physically break geometric rendering. Content Pane Titles are processed through `dangerouslySetInnerHTML` to permit manual formatting.

## VII. System Stability & Error Boundaries
* **[REF: BOOT-CRASH-01] Boot Safety Validation:** To prevent fatal `ReferenceError` crashes during early component mounting (which result in unrecoverable blank screens), all Native JS constructors (`new URLSearchParams`, `new Date`, `new Set`) are strictly validated against typos and scope constraints prior to execution within React's `useEffect` hooks.
* **[REF: BOOT-CRASH-01] Boot Safety Validation:** To prevent fatal unrecoverable blank screens, all Native JS constructors (`new URLSearchParams`) must be strictly validated against syntax typos prior to React's first render hook.
* **[REF: TL-CLAMP-01] Span Geometry Clamp:** The Timeline Scrubber must strictly enforce a minimum temporal visual width of 24 hours (`86400000` ms) to prevent `0px` layout collapse in single-event datasets.
* **[REF: TL-CLAMP-02] Infinity Geometry Clamp:** The Timeline Scrubber's automatic scaling math must enforce a strict `60000` ms floor on event time gaps. This mathematically ensures the engine never divides by zero and triggers an infinite zoom collapse when multiple events share an identical timestamp.
