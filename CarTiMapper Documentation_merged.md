# CarTiMapper Engine Master Blueprint
**Version:** v6.8.1 | **Date:** May 1, 2026

**Source Repositories:**
* **CarTiMapper App:** 'CarTiMapper/cartimapper.v6.html at main · ageor7/CarTiMapper'
* **Master Blueprint:** CarTiMapper Documentation.md
* **ChangeLog:** ChangeLog.md

---

## I. Engineering Protocols & Coding Rules / Core Protocols & Standing Directives

1. **[REF: DOC-01] Two-Tier Documentation (REF-TAGS):** Documentation is strictly bifurcated. The Master Blueprint contains the "Why" and "What" (Architecture, UI/UX Rules, Physics). The inline code comments contain the "How" (Mechanics, Loops, Variables). They are permanently interlinked using `[REF: TAG-NAME]` semantic anchors. **Zero duplication.**
2. **Spreadsheet REF-TAGS:** Because Google Sheets does not support native code comments, REF-TAGs are securely injected into the ETL formula using `LET()` dummy variables (e.g., `_ref_map01, "[REF: MAP-01]..."`) to preserve the documentation pipeline without breaking mathematical execution.
3. **Discuss First, Implement Later:** All bugs, physics, and UI/UX behaviors must be mathematically diagnosed and agreed upon before any code is generated.
4. **Strict Bottom-to-Top Patching:** To protect line structure and prevent cascading offsets, patches are always delivered in strictly reverse order (from the bottom of `index.html` to the top). The typical sequence is: `AppOrchestrator -> TimelineScrubber -> ContentSlider -> Global Variables -> Global Styles`.
5. **Semantic Boundary Replacements:** We rely exclusively on the indestructible `// === [ BLOCK NAME vX.X.X ] ===` boundary markers instead of fragile, hardcoded line numbers.
6. **No Default Bold/Capitalize:** We do not enforce `font-weight: bold` or `text-transform: uppercase` on narrative text through global CSS. The interface relies entirely on explicit manual HTML tags (e.g., `<b>`, `<strong>`) for emphasis.
7. **True Font Loading:** When utilizing Google Fonts (especially for Greek Polytonic characters), we explicitly download all Regular (400) and Bold (700) weights and Italics etc. variations to ensure the browser never generates jagged "faux-bolds", "faux-italics" etc.
8. **[REF: ARCH-01] Two-Stage Data Pipeline:** To support dynamic on-the-fly re-rendering without network latency, the engine strictly separates raw data ingestion (`rawCsvRows`) from the compiled data state (`data`).
9. **[REF: DOC-02] Architectural Block Taxonomy:** To eliminate cascading errors and enable risk-free surgical patching, the codebase is strictly categorized into visual and logical boundaries.
    * **Major Blocks:** High-level components (e.g., AppOrchestrator, GlobalStyles). Must carry explicit version numbers. 
        * *Syntax:* `// === [ MAJOR BLOCK: ComponentName vX.X.X ] ===`
    * **SubBlocks:** Discrete chunks of logic within a Major Block.
        * *Syntax:* `// --- [ SubBlock: LogicDescription ] ---`
10. **[REF: VER-01] Strict Semantic Versioning:** Version numbers are manually tracked on both the global app and individual Major Blocks.
    * **Feature Directives:** Increment the MINOR version and reset the patch (e.g., `v6.0.15` → `v6.1.0`).
    * **Debugging & Hotfixes:** Increment the PATCH version by one (e.g., `v6.0.15` → `v6.0.16`).
11. **[REF: DOC-03] GitHub Artifact Generation:** Every code alteration requires explicit Markdown artifacts prior to code delivery: Commits/Changelogs for history, and Master Blueprint updates for architecture.
12. **[REF: PROT-01] Documentation Supremacy & Strict MD Formatting:** No code patch, architectural adjustment, or bug fix is considered complete without its corresponding Master Blueprint and Changelog updates. The developer AI must always provide these documentation updates strictly in isolated `.md` syntax blocks alongside every code delivery. This protocol guarantees zero architectural drift and prevents UI formatting corruption.
13. **[REF: PROT-02] Discuss First, Code Later:** The developer AI is physically prohibited from generating or outputting code blocks without prior explicit authorization. The mechanical operational loop is strictly locked to: 1) Diagnose Physics. 2) Propose Architecture. 3) Await Explicit User "Go-Ahead".
14. **[PROT-03] Safety & Functionality Verification:** Every proposed architectural patch must be mathematically and structurally cross-referenced against the current active functionality. No update shall be executed if it risks destabilizing existing flexbox layouts, mobile touch-targets, or spatial synchronization engines.
15. **[PROT-04] Architectural Pushback:** The engine (AI) is explicitly authorized and required to challenge user suggestions or design decisions if they violate DOM physics, introduce structural fragility, or break mobile responsiveness. A logic-based case and safe counter-proposals must always be presented.
16. **[PROT-05] Context Acquisition (The Block Gate):** If a diagnosis requires altering a component whose exact current state is not actively locked in the engine's immediate memory, the engine must explicitly request the user to paste the necessary component code block *alongside* the request for the execution "go-ahead". No blind overwriting is permitted.
17. **[PROT-06] Touch Target Preservation (Fitts’s Law):** The engine refuses structural UI requests that compromise safe touch interactions on mobile. Primary interaction labels cannot be wholly enveloped in `<a href>` routing tags, as this conflates discrete actions and causes UX failure on smaller viewports.

---

## II. Data Schema & The Upstream ETL Pipeline 

* **[REF: ETL-01] Purpose of the ETL Formula:** The frontend JavaScript engine should *render* data, not *clean* it. The Google Sheets `LET()` formula intercepts messy human inputs, deduplicates tags, and natively compiles valid JSON/WKT before the frontend sees it.
* **[REF: DATA-02] The Cartographer's Dilemma:** Humans use `[Lat, Lon]`. GIS standards (GeoJSON, WKT) demand Cartesian `[Lon, Lat]`. The spreadsheet translates this natively, ensuring export interoperability with QGIS/PostGIS.
* **[REF: DATA-03] Spatial Deduplication:** The ETL wraps arrays in `UNIQUE()`, purging redundant geographic nodes before casting them to prevent overlapping DOM elements and broken Z-indexes.
* **[REF: DATA-05] Direct Key Mapping:** Data is normalized via lowercase `norm` object mapping (`exactGet`). Legacy `fuzzy()` logic is deprecated to prevent column string collisions.
* **[REF: DATA-06] Strict Euro-Date Logic:** `parseEuroDate` forces `DD/MM/YYYY` parsing to prevent US-browser inversion.
* **[REF: DATA-09] Media & Metadata Extraction:** The `exactGet` ingestion loop explicitly extracts `media`, `caption`, and `credit` columns from the CSV payload. These strings are flattened using the `omniSplitRegex` (`/\||\r?\n/`) to guarantee parallel 1:1 array mapping.
* **[REF: DATA-10] Place/SubLabel Convergence:** Hover Tooltips are mathematically bound to the `Place` column, enforcing single-source-of-truth location labeling.
* **[REF: DATE-11] Time Preservation:** The parser decouples `HH:mm:ss` payloads from date strings using whitespace isolation to prevent `00:00:00` collision bugs in timeline zoom physics.
* **[REF: SUB-01] Universal SubLabel Delimiters:** RegEx parser `/[|\-·;]/` splits sublabels, capturing pipes, semicolons, dashes, and the Greek Ano Teleia (`·`).
* **[REF: MED-08] Aggressive CRLF Sanitization:** RegEx `/\||\r?\n/` identifies and strips invisible Windows-style carriage return `\r` ghosts, preventing them from fusing to URLs and causing HTTP 404s.
* **[REF: MED-11] The Omni-Splitter Matrix:** The engine utilizes a mathematically precise delimiter interceptor (`/[\r\n]+|\\n/`) during the CSV mapping phase to eliminate destructive parsing bugs where commas naturally occurring in filenames unintentionally fractured the URL strings.
* **[REF: TL-17] Timeline Omni-Splitter:** Intercepts incoming `item.tags` via `getParsedTags()`. Ensures horizontal swimlanes split properly during background generation, scaling vertical boundaries appropriately.
* **[REF: PERF-07] Local Safe-Strip Scoping:** Strict, localized closures (`safeStripHTML = (str) => ...`) utilizing rigorous Regex bounds (`/<[^>]*>?/gm`) are hardcoded directly into the Map and Timeline dependencies, hermetically sealing them against global scope HTML filter failures.
* **[REF: PERF-08] High-Precision Millisecond Chronology:** In high-density datasets, events assigned identical dates and hours result in an unstable Javascript `sort()` algorithm. The string ingestion loop parses down to `ss.000` (milliseconds), transmuting standard timestamps into highly specific absolute UNIX integers to lock simultaneous events into an immutable chronological progression.
* **[REF: UI-41 & UI-44] Multi-Provider Universal Binary Translation:** An active regex pipeline intercepts Google Drive and GitHub endpoints at the data ingestion phase, transposing them to raw API endpoints (`uc?export=view&id=` and `raw.githubusercontent.com`). This transforms web page links into direct, uncorrupted binary byte streams for the MediaViewer.

---

## III. Cartographic & Spatial Physics

* **[REF: MAP-01] WKT Engine:** The engine natively parses Well-Known Text (WKT) via the `Wicket` library. Falls back safely to GeoJSON or standard Lat/Lon pairs.
* **[REF: MAP-02] Spatial Decluttering (MarkerCluster):** The map utilizes an R-Tree spatial index (`MarkerCluster`) to group overlapping geographic elements into numbered bubbles based on a 40px screen radius.
* **[REF: MAP-03] Dynamic VIP "Slide Focus":** The map utilizes a "State-Based Layer Swapping" architecture. It builds an `O(1)` memory dictionary on load. On slide change, it uses the dictionary to instantly locate the active marker, pull it from the background cluster, elevate its Z-index, and restyle it.
* **[REF: MAP-04] Parallel Array (Sub-Labels):** Switched to a Strict Linear Map algorithm. If 3 geometries and 3 sub-labels are present, the engine mathematically locks them to prevent label duplication across LayerGroups.
* **[REF: MAP-05] Linear Sub-Label Mapping:** MultiPoint geometries are recursively flattened into a linear array to ensure 1:1 parity with the pipe-delimited sub-label string.
* **[REF: MAP-07] Tooltip Physics:** Tooltips are anchored with a `-48px` vertical offset to provide aesthetic breathing room above the Map Pin head.
* **[REF: MAP-08] Dynamic Radar Architecture (Secondary Map Sync):** The minimap dynamically tracks the main map's focal telemetry in real-time. By explicitly mapping the `zoomLevelOffset` to a global UI state variable (`minimapOffset`), the engine allows the user to dynamically adjust their geographic context "Radar" from `-2` to `-8` zoom levels out.
* **[REF: MAP-09] Telemetry Bridge (Stale Closure Fix):** Because React event listeners create stale closures, the engine bridges this gap by injecting a `useRef` hook (`minimapOffsetRef`) specifically to feed live data into the frozen map listener, guaranteeing the minimap dynamically respects offset changes in real-time.
* **[REF: MAP-10] Visual Projection Math (Minimum Viewport Clamp):** To circumvent the minimap aiming rectangle shrinking into an invisible dot at deep zoom levels, the engine overrides the true map bounds, projecting a synthetic, mathematically expanded set of `L.latLngBounds` to guarantee the reticle never visually shrinks below a 30-pixel readable limit.
* **[REF: MAP-11] The Basemap Core Registry:** Securely holds all required Leaflet construction data for a curated list of providers. When the user selects a new Basemap, the engine natively intercepts the telemetry and mathematically injects the new target layer into both the Main Map and the Minimap simultaneously to prevent z-index rendering collisions.
* **[REF: MAP-80] Dynamic Web Map Service (WMS) Routing:** To support institutional academic mapping platforms, the Google Sheet registry parses `format` and `wmsLayer` columns. When the `MapViewer` detects a `wms` format flag, it routes the layer through `L.tileLayer.wms()`, requesting a custom, single-image render directly from the institution's enterprise GeoServer.
* **[REF: MAP-85] WebGL & Vector Tile Architecture:** To support Protomaps vector data (`.mvt`/`.pbf`), the engine dynamically injects MapLibre GL JS and its Leaflet binding via asynchronous script loading. The WebGL engine is only spun up when a vector basemap is actively selected, preserving application performance.
* **[REF: MAP-86] Layer Casing (Cartographic Styling):** Vector basemap aesthetics are controlled via a local `style.json`. To achieve high-end cartographic roads, the engine utilizes WebGL layer casing (a thicker casing layer underneath a thinner fill layer). The styling utilizes the `stops` array to dynamically scale `line-width` based on zoom level.
* **[REF: PERF-02] O(1) Spatial State Architecture (The Delta Tracker):** The engine uses a mathematically pure $O(1)$ index tracker (`prevActiveIndexRef`). During navigation, it explicitly targets and demotes *only* the old marker, and targets and promotes *only* the new marker, dropping rendering complexity by 99.8%.
* **[REF: PERF-03] Spatial Animation Interpolation Brake:** Injecting an absolute `map.stop()` instantly aborts the active physics frame during rapid keyboard navigation, freeing the thread to calculate the next flight path cleanly.
* **[REF: PERF-04] The Transmission Clutch (Map Debouncing):** To fully decouple the Map DOM from rapid user telemetry, a `debouncedIndex` transmission clutch suspends heavy spatial math until a `100ms` pause in keystrokes is detected.
* **[REF: PERF-05] MarkerCluster Bulk Ingestion Engine:** Geometries are collected into an invisible JavaScript array (`bulkClusterMarkers`). A singular `clusterLayer.addLayers()` command is executed post-loop, reducing geometric grid generation to a single millisecond execution phase.
* **[REF: UI-42] InvalidateSize Base-Tile Forcing:** The engine mechanically circumvents React Flexbox layout race conditions by delaying execution for 250ms and forcing `mapInstance.invalidateSize()`, commanding Leaflet to poll the true DOM dimensions and trigger the pending HTTP tile requests.
* **[REF: CRASH-01] Dynamic Zoom Ceilings & Tile Stretching:** The engine dynamically calculates an absolute `maxGlobalZoom` from the highest available CMS layer. All spatial layers utilize a decoupled `maxNativeZoom` vs `maxZoom` matrix, commanding Leaflet to physically stretch lower-resolution tiles to seamlessly fill the viewport when the user zooms deep into high-resolution historical overlays.
* **[REF: UI-71] Real-Time Layer Opacity Engine:** When an overlay is toggled `Active`, the Layers HUD mounts an HTML `<input type="range">` slider. This slider is structurally decoupled from the `<label>` parent, allowing users to scrub the `L.tileLayer.setOpacity()` WebGL rendering in real-time.

---

## IV. UI/UX Elements & Design Solutions

* **[REF: UI-05] Visual Hierarchy (Map Pins):** Permanent geographical anchors (VIPs) = Gold pins. Active elements = Oversized Green pins. Inactive clusters = Standard Blue pins.
* **[REF: UI-08] Multi-Context Branding:** Logo Hex-Frame Chrono-Compass scales from Splash Context to Status Bar Context.
* **[REF: UI-13] Metadata Hierarchy:** The `📍 Place` badge utilizes a non-bold, pill-shaped aesthetic to distinguish metadata from narrative text.
* **[REF: UI-32] Smart Date Deduplication (Same-Day Delta):** The formatting engine mathematically strips midnight `00:00:00` signatures. If an event begins and ends on the exact same Day/Month/Year but at different times, the engine isolates the time signature and outputs a clean temporal delta (e.g., `15:45 — 16:00`).
* **[REF: UI-39] Shrink-Wrapped Sticky Header (Direct-Child Architecture):** The `ContentSlider` enforces a strict geographic-first hierarchy (`Date ➔ Places ➔ Tags ➔ Title`). The header is a *direct child* of the `overflow-y: auto` parent, guaranteeing it physically freezes at `top: 0`. Contextual metadata (Places & Tags) are dynamically flex-aligned to the right-hand side.
* **[REF: UI-40] Golden SVG Tag Identity:** Semantic Tags abandon native OS Emojis in favor of a mathematically plotted inline `<svg>` path (`#ffc107`), universally rendering a flawless geometric identity across all browser engines.
* **[REF: UI-46] Left-Aligned Menu Dock:** Secondary utility tools (Settings, Monitor) are condensed into a single popover menu. The `☰ Menu` button is rigidly anchored to the absolute far-left edge of the status bar.
* **[REF: UI-47] The 75ch Readability Clamp:** The narrative description block (`.slide-desc`) is mathematically clamped to `max-width: 75ch` and automatically centers itself, preserving the natural vertical scroll while maintaining an optimal typographical line-length.
* **[REF: UI-49] Typographical Outset (The Hero Overhang):** The sticky header utilizes a `max-width: 90ch` envelope, while the narrative description utilizes a `max-width: 75ch` envelope. This creates a subtle typographic overhang, allowing long Event Titles and Place names to comfortably expand beyond the text margins without forcing ugly, premature line breaks.
* **[REF: UI-51] Fluid Axis Rotation (The "Reading Room" Architecture):** 
    * *< 1024px Portrait:* Vertical layout stack. Primary resizer acts as a horizontal drag-bar calculating Y-Axis deltas.
    * *>= 1024px Desktop:* The geometric axis rotates 90 degrees. The Text pane claims 50-60% of the left screen. Map & Media are pushed to the right sidebar. The Javascript resizer strictly detects this orientation shift and mathematically switches its calculation logic to the X-Axis.
* **[REF: UI-52] Layout Boundary Mathematical Reset:** The absolute instant the browser boundary crosses the `1024px` threshold, the app automatically zeroes both CSS structural variables back to `50%`, ensuring perfect proportional integrity.
* **[REF: UI-53] Dynamic Secondary Viewport Collapse:** If `activeSlide.media.length === 0`, the engine absolutely hides the Media pane and Secondary Resizer via `display: none`, and seamlessly expands the Map instance to consume `100%` of the available quadrant.
* **[REF: UI-61] CSS Native Touch Override (The Panning Hijack Fix):** The CSS `touch-action: none;` directive is surgically applied directly to the `.resizer-dyn` classes. This explicitly commands the OS to ignore all native pan/scroll interpretations and yield 100% of the raw touch coordinates to the JavaScript delta engine.
* **[REF: UI-62] Strict Android Flexbox Containment Cage:** The global stylesheet enforces an absolute CSS cage (`max-width: 150px !important; flex-shrink: 0 !important`) entirely forbidding the Android rendering engine from expanding the Minimap node.
* **[REF: UI-65] The Micro-Scroll Ribbon (Dynamic Header):** Breaching the 40px scroll threshold initiates a CSS matrix transition: the header's padding compresses, the Title shrinks, and all metadata arrays merge into a single `white-space: nowrap` horizontal flex-row.
* **[REF: UI-66] Omni-Search Engine & LocalStorage Persistence:** A dedicated memory-layer React Modal simultaneously queries all dataset parameters. A secondary state hook captures executed query strings to present a clickable "Recent Searches" history pool.
* **[REF: UI-67] Minimalist Icon-Only Mode:** Text labels are mathematically stripped from the Bottom Navigation Bar and Map HUD, leaving only clean, universally recognizable SVGs.
* **[REF: UI-69] Ribbon Alignment & Telemetry Compression:** A fluid CSS spacer (`margin-left: auto`) is injected into the first metadata badge, forcing the Date to anchor permanently to the left, while instantly pushing all Tags and Places to the far right.
* **[REF: UI-70] Map HUD Typography & Interaction Matrix:** The isolated zoom controls utilize a vertical "Sandwich" flexbox column (`[+]`, `[14z]`, `[-]`), anchoring the focal integer directly between the action triggers.
* **[REF: UI-72] Global Maximization Matrix:** Utilizing global states (`maxPane` & `isTimelineExpanded`), the `AppOrchestrator` injects scoped CSS wrapper classes. This natively expands the active pane to `100% width/height` while safely unmounting sibling panes without destroying the virtual DOM.
* **[REF: UI-83] Top-Right Spatial Constraints:** External source links, window maximization toggles, and active media counters are physically extracted from their parent nodes and locked to floating, absolute top-right spatial axes for consistent muscle-memory accessibility.
* **[REF: UI-85] Spatial Window Management:** Timeline controls are strictly divided. Window Management (Minimize/Maximize) is anchored to the absolute top-right, while Temporal Navigation (Zoom in/out, Auto-lock) is docked lower down to prevent accidental UX destruction.
* **[REF: UI-86] The Drawer & FAB Synergy:** Minimizing the timeline shrinks the container height to `0px`. To prevent Leaflet's WebGL context from crashing due to sudden container expansion, a CSS transition buffers the resize, giving the engine's `ResizeObserver` time to gracefully redraw the tiles. A Floating Action Button (FAB) dynamically mounts over the map to allow users to restore the timeline drawer.
* **[REF: UI-87] The Android Bloat Fix (CSS Reset):** Mobile OS accessibility settings forcefully inflate typography, distorting buttons. The UI architecture completely abandons emojis in favor of mathematically defined SVGs. Combined with `-webkit-appearance: none;` and hardcoded dimensions, the buttons are permanently immune to OS-level geometry distortion.
* **[REF: UI-88] The Phantom Hitbox:** To maintain a minimalist aesthetic without sacrificing mobile usability, critical interactive elements (`.status-btn`) utilize an `::after` pseudo-element. This projects an invisible, absolute-positioned hitbox 8px beyond the visual boundaries of the button, preventing fat-finger misclicks while keeping the visual grid tight.
* **[REF: PERF-12] Scroll-Tracking Indexing:** The `MediaViewer` abandons React-state-driven indexing in favor of native CSS `scroll-snap-type: x mandatory`. The active image index and counter are passively updated by reading the browser's native `scrollLeft` property, ensuring buttery-smooth swipe performance on mobile devices.

---

## V. Algorithms, Analytics & Methodologies

* **1. Spatial Indexing (The R-Tree Engine):** `MarkerCluster` uses `RBush` to divide the map into nested rectangular grids. It measures point-to-grid, allowing instant, `O(log n)` collision detection.
* **2. State-Based Layer Swapping & The O(1) Dictionary:** To bypass Leaflet's internal `_leaflet_id` amnesia, the engine builds a global dictionary (`markersRef`). Slide changes execute an `O(1)` instant lookup to swap layers in and out of the cluster bucket seamlessly.
* **3. The Dual-Heuristic Density Math (Timeline Auto-Zoom):** Evaluates two vectors: `collisionZoom` (The 10px Rule for preventing overlap on the same lane) and `contextZoom` (The 150px Rule for keeping the nearest event visible). Executes `Math.max()` to adopt the safest zoom.
* **4. The URL Routing Engine:** `[REF: URL-01]` Intercepts CLI/URL parameters. `?date=` executes a mathematical distance calculation across the array to locate the closest absolute time-node. `?slide=X` overrides chronology. `?theme=dark` manipulates root CSS properties prior to physical DOM render. `getOmniParam()` sanitizes `amp;` corruptions natively and falls back to Hash Routing ensuring 100% deep-link execution parity.
* **5. The Visual-Center Scrolling Algorithm:** Calculates `blockPixelWidth` dynamically. Plots the geometric center (`StartPx + Width/2`). Commands the scrollbar to target `visualCenterPx - (Screen Width / 2)` to perfectly center wide text blocks.
* **6. HTML Filter Bifurcation:** Timeline Hexagons are processed through `stripHTML()` to guarantee layout breaks (`<br>`) do not physically break geometric rendering. Content Pane Titles are processed through `dangerouslySetInnerHTML` to permit manual formatting.
* **7. Canvas Memory Leak Assassination (Singleton Hoisting):** `[REF: PERF-06]` A singular, immutable `_globalTmCtx` singleton variable is instantiated once during script load and serves as a permanent, zero-leak mathematical reference for all global geometry canvas calculations.
* **8. Strict Mode Memory Leak Assassination:** `[REF: PERF-01]` The global keyboard spatial navigation engine decouples history state mutation (`setSlideHistory`) from the active state update loop. By wrapping the side-effects in a mathematically pure `jumpToSlide` closure, we prevent recursive Virtual DOM re-renders.

---

## VI. System Stability & Error Boundaries

* **[REF: BOOT-CRASH-01] Boot Safety Validation:** To prevent fatal unrecoverable blank screens, all Native JS constructors (`new URLSearchParams`) must be strictly validated against syntax typos prior to React's first render hook.
* **[REF: TL-CLAMP-01] Span Geometry Clamp:** The Timeline Scrubber must strictly enforce a minimum temporal visual width of 24 hours (`86400000` ms) to prevent `0px` layout collapse in single-event datasets.
* **[REF: TL-CLAMP-02] Infinity Geometry Clamp:** The Timeline Scrubber's automatic scaling math must enforce a strict `60000` ms floor on event time gaps to mathematically ensure the engine never divides by zero.
* **[REF: DIAG-01] Active Sensor Telemetry:** The Vibe Monitor passively exposes the application version manifest and the raw location string of the currently active dataset row.
* **[REF: CRASH-01] Library Polyfill Injection:** The `MapViewer` implements a native string-interception polyfill that explicitly deconstructs `GEOMETRYCOLLECTION` wrappers and passes isolated internal primitives through the parser.
* **[REF: CRASH-02] React Prop Continuity:** Cross-component navigational functions (e.g., `jumpToSlide`) must be explicitly passed via React component props to prevent fatal unrecoverable ReferenceErrors inside isolated Modals.
