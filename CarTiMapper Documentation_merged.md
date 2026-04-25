# CarTiMapper Engine Master Blueprint
**Version:** v6.4.9 | **Date:** April 25, 2026

**Source Repositories:**
* **CarTiMapper App:** 'CarTiMapper/cartimapper.v6.html at main · ageor7/CarTiMapper' (<https://github.com/ageor7/CarTiMapper/blob/main/index.html>)
* **Master Blueprint:** [CarTiMapper Documentation.v6.4.7.md](https://raw.githubusercontent.com/ageor7/CarTiMapper/main/CarTiMapper%20Documentation.v6.4.7.md)
* **ChangeLog:** [ChangeLog.v6.4.7.md](https://raw.githubusercontent.com/ageor7/CarTiMapper/main/ChangeLog.v6.4.7.md)

---

## I. Engineering Protocols & Coding Rules

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
12. **[REF: PROT-01] Documentation Supremacy:** No code patch, architectural adjustment, or bug fix is considered complete without its corresponding Master Blueprint and Changelog updates formatted in strict Markdown. This acts as a legal binding contract to prevent documentation desynchronization.
13. **[REF: PROT-02] Discuss First, Code Later:** The developer AI is physically prohibited from generating or outputting code blocks without prior explicit authorization. The mechanical operational loop is strictly locked to: 1) Diagnose Physics. 2) Propose Architecture. 3) Await Explicit User "Go-Ahead". This permanently terminates version fragmentation and unrequested UI drift.
1. **[REF: DOC-01] Two-Tier Documentation:** Master Blueprint contains "Why/What". Inline comments contain "How".
2. **[REF: PROT-01] Documentation Supremacy:** No code patch is complete without Master Blueprint and ChangeLog updates. **Furthermore, all documentation must be delivered strictly in `.md` format.**
3. **[REF: PROT-02] Discuss First, Code Later:** Mechanical loop: 1) Diagnose. 2) Propose. 3) Await "Go-Ahead".
* **[REF: PROT-01] Documentation Supremacy & Strict MD Formatting:** No code patch, architectural adjustment, or bug fix is considered complete without its corresponding Master Blueprint and Changelog updates. **Furthermore, all documentation deliverables must be formatted strictly in isolated `.md` syntax blocks.** This protocol guarantees zero architectural drift and prevents UI formatting corruption across platforms.
---

## II. Active Requirements Matrix

### Met Requirements
* **[REF: ETL-01] Upstream Spatial ETL:** The engine now relies on a compulsory source URL parameter for initialization.
* **[REF: MAP-01] The WKT Mapping Engine:** Map natively parses Well-Known Text (WKT) via the `Wicket` library. Falls back safely to GeoJSON or standard Lat/Lon pairs.
* **[REF: MAP-02] Spatial Decluttering (Clustering):** The map utilizes an R-Tree spatial index (`MarkerCluster`) to group overlapping geographic elements into numbered bubbles based on a 40px screen radius.
* **[REF: MAP-03] Dynamic VIP Slide Focus:** Active narrative elements are mathematically extracted from the cluster bucket, styled as "Fire Pins", and dropped back into the background when the user leaves the slide.
* **[REF: MAP-05] Linear Sub-Label Mapping:** MultiPoint geometries are recursively flattened into a linear array to ensure 1:1 parity with the pipe-delimited sub-label string.
* **[REF: URL-01] Deep-Linking Engine:** Fully supports `?source=`, `?slide=`, `?date=`, `?theme=`, and `?mapzoom=`.
* **Layout Hardening:** Map/Text/Media panes perfectly respect Flexbox boundaries.
* **Smart Auto-Zoom (Timeline):** The timeline mathematically prevents text collisions while preserving dataset context.
* **Ergonomic Touch Navigation:** The text pane supports frictionless Left/Right tap-to-navigate zones (15% width).
* **Media Agnosticism:** Media pane natively renders images, YouTube iframes, Google Maps iframes, and raw HTTP links.

### Pending / Future Considerations
1. **Layout Memory:** Implement local-storage to save user-defined pane layouts/resizing states.
2. **Directional Vectors:** Investigate adding direction arrows to line vectors (Trade routes/Troop Marches).
3. **Duration on X-Axis:** Display (Start - End) times on the X-axis for the currently active event without overcrowding.

---

## III. Core Architecture: The Mapping Engine (The "Why" and "What")

* **[REF: MAP-01] WKT Engine:** Removed redundant coordinate inversion logic. The engine trusts the upstream ETL output of `POINT(Lon Lat)` to maintain QGIS/PostGIS interoperability.
* **[REF: MAP-02] Spatial Decluttering (MarkerCluster):** As the database grows, distant map elements mathematically overlap on screen. The R-Tree clustering plugin solves UI clutter without deleting data by dynamically merging pins based on zoom level.
* **[REF: MAP-03] Dynamic VIP "Slide Focus":** The map utilizes a "State-Based Layer Swapping" architecture. It builds an `O(1)` memory dictionary on load. On slide change, it uses the dictionary to instantly locate the active marker, pull it from the background cluster, elevate its Z-index, and restyle it.
* **[REF: MAP-04] Parallel Array (Sub-Labels):** Switched to a Strict Linear Map algorithm. If 3 geometries and 3 sub-labels are present, the engine mathematically locks them to prevent label duplication across LayerGroups.
* **[REF: MAP-07] Tooltip Physics:** Tooltips are anchored with a `-48px` vertical offset to provide aesthetic breathing room above the Map Pin head.
* **[REF: PERF-02] O(1) Spatial State Architecture (The Delta Tracker):** Previous iterations forced Leaflet to destroy and repaint the entire `L.divIcon` HTML payload for all 500+ markers on every slide change (an `O(N)` loop), catastrophically freezing the browser. The engine now uses a mathematically pure $O(1)$ index tracker (`prevActiveIndexRef`). During navigation, it explicitly targets and demotes *only* the old marker, and targets and promotes *only* the new marker. The other 498 elements are completely ignored by the JS thread, dropping rendering complexity by 99.8%.
* **[REF: PERF-03] Spatial Animation Interpolation Brake:** Rapid keyboard navigation queued multiple `map.flyToBounds()` camera geodesics concurrently. Because Leaflet attempts to calculate 3D flight arcs mathematically, overlapping destination requests forced the CPU to calculate infinite competing curves, hanging the tab. Injecting an absolute `map.stop()` instantly aborts the active physics frame, freeing the thread to calculate the next flight path cleanly.
* **[REF: PERF-04] The Transmission Clutch (Map Debouncing):** To fully decouple the Map DOM from rapid user telemetry (e.g., holding down the Arrow Key at 30fps), a `debouncedIndex` transmission clutch intercepts the `activeIndex`. The Timeline and UI execute instantly, but the heavy spatial math (cluster extraction, Z-index elevation) is suspended until a `100ms` pause in keystrokes is detected.
* **[REF: PERF-05] MarkerCluster Bulk Ingestion Engine:** Leaflet's internal collision logic relies on an exponentially heavy $O(N^2)$ algorithm. Feeding WKT elements one-by-one via `addLayer()` within a data loop forced the engine to recalculate the entire grid hundreds of times in a fraction of a second, crashing Firefox/Chrome on boot. Geometries are now collected into an invisible JavaScript array (`bulkClusterMarkers`). A singular `clusterLayer.addLayers()` command is executed post-loop, reducing geometric grid generation to a single millisecond execution phase.
* **[REF: UI-42] InvalidateSize Base-Tile Forcing:** When executing inside complex React Flexbox layouts, Leaflet routinely initializes a fraction of a second before the CSS DOM settles, causing it to read its container size as `0x0` pixels. Consequently, it refuses to request background maps, resulting in a permanent blank screen. The engine mechanically circumvents this by delaying execution for 250ms and forcing `mapInstance.invalidateSize()`, commanding Leaflet to poll the true DOM dimensions and trigger the pending HTTP Esri/OSM tile requests.CarTiMapper Documentation

---

## IV. Data Schema & The Upstream ETL Pipeline 

* **[REF: ETL-01] Purpose of the ETL Formula:** The frontend JavaScript engine should *render* data, not *clean* it. The Google Sheets `LET()` formula intercepts messy human inputs, deduplicates tags, and natively compiles valid JSON/WKT before the frontend sees it.
* **[REF: DATA-02] The Cartographer's Dilemma:** Humans use `[Lat, Lon]`. GIS standards (GeoJSON, WKT) demand Cartesian `[Lon, Lat]`. The spreadsheet translates this natively, ensuring export interoperability with QGIS/PostGIS.
* **[REF: DATA-03] Spatial Deduplication:** The ETL wraps arrays in `UNIQUE()`, purging redundant geographic nodes before casting them to prevent overlapping DOM elements and broken Z-indexes.
* **[REF: DATA-05] Direct Key Mapping:** Data is normalized via lowercase `norm` object mapping (`exactGet`). Legacy `fuzzy()` logic is deprecated to prevent column string collisions.
* **[REF: DATA-06] Strict Euro-Date Logic:** `parseEuroDate` forces `DD/MM/YYYY` parsing to prevent US-browser inversion.
* **[REF: DATA-09] Memory Pruning:** Extraneous spreadsheet columns are omitted from data mapping to minimize JSON payload size.
* **[REF: DATA-10] Place/SubLabel Convergence:** Hover Tooltips are mathematically bound to the `Place` column, enforcing single-source-of-truth location labeling.
* **[REF: DATE-11] Time Preservation:** The parser decouples `HH:mm:ss` payloads from date strings using whitespace isolation to prevent `00:00:00` collision bugs in timeline zoom physics.
* **[REF: SUB-01] Universal SubLabel Delimiters:** RegEx parser `/[|\-·;]/` splits sublabels, capturing pipes, semicolons, dashes, and the Greek Ano Teleia (`·`).
* **[REF: MED-08] Aggressive CRLF Sanitization:** RegEx `/\||\r?\n/` identifies and strips invisible Windows-style carriage return `\r` ghosts, preventing them from fusing to URLs and causing HTTP 404s.
* **[REF: MED-18] The Omni-Splitter Matrix:** Universal regex interceptor `/[\r\n]+|\\n/` combats unpredictable cross-platform CSV string encoding (lone `\r`, standard `\n`, escaped `\\n`), guaranteeing arrays split properly.
* **[REF: TL-17] Timeline Omni-Splitter:** Intercepts incoming `item.tags` via `getParsedTags()`. Ensures horizontal swimlanes split properly during background generation, scaling vertical boundaries appropriately.
* **[REF: PERF-08] High-Precision Millisecond Chronology:** In high-density datasets, events assigned identical dates and hours result in an unstable Javascript `sort()` algorithm, causing items to jump and flicker. The string ingestion loop was surgically rebuilt to parse down to `ss.000` (milliseconds) via the exact format requirement: `dd/mm/yyyy HH:mm:ss.000`. By transmuting standard timestamps into highly specific absolute UNIX integers, the engine mathematically locks simultaneous events into an immutable chronological progression.
* **[REF: UI-41 & UI-44] Multi-Provider Universal Binary Translation:** Inserting raw Google Drive (`/view`) or GitHub (`/blob/`) links into a spreadsheet forces the MediaViewer `<img>` to attempt to render an entire HTML DOM wrapper website, resulting in broken image icons and CORS rejections. An active regex pipeline intercepts these endpoints at the data ingestion phase: GitHub paths are geometrically transposed to `raw.githubusercontent.com`, and Google Drive paths are rewritten to access the hidden `uc?export=view&id=` API endpoint. This transforms web page links into direct, uncorrupted binary byte streams.
* **[REF: PERF-07] Local Safe-Strip Scoping:** To prevent rendering tooltips loaded with messy HTML tags (`<b>`, `<i>`), the engine relied on a global `stripHTML()` utility. If the map initialized before the utility was hoisted, or if a null-value was passed, it triggered a silent, unrecoverable `ReferenceError` that entirely aborted the React component loop. Strict, localized closures (`safeStripHTML = (str) => ...`) utilizing rigorous Regex bounds (`/<[^>]*>?/gm`) were hardcoded directly into the Map and Timeline dependencies, hermetically sealing them against global scope failures.

---

## V. UI/UX Elements & Design Solutions

* **The 15% Ergonomic Touch Zones:** Invisible 15% width `.click-zone` overlays on left/right edges for mobile thumbs, preserving a 70% "safe-scrolling highway".
* **The Unified Status Bar Centering:** Strict Flexbox `flex: 1` geometry applied to Left, Center, and Right zones forces `Prev/Next` buttons to absolute screen center.
* **Tag Lane "Sticky" Shielding:** Detached swimlanes from `overflow: hidden` wrappers and explicitly anchored bounds to `bottom: 28px` to prevent downward bleeding into the X-Axis.
* **[REF: UI-05] Visual Hierarchy (Map Pins):** Permanent geographical anchors (VIPs) = Gold pins. Active elements = Oversized Green pins. Inactive clusters = Standard Blue pins.
* **[REF: UI-08] Multi-Context Branding:** Logo Hex-Frame Chrono-Compass scales from 300px (Splash Context) to 24px (Status Bar Context).
* **[REF: UI-13] Metadata Hierarchy:** The `📍 Place` badge utilizes a non-bold, pill-shaped aesthetic to distinguish metadata from narrative text.
* **[REF: MAP-10] Active State Elevation (Unclustering):** Active markers are programmatically detached from the `MarkerClusterGroup` and elevated to the root map layer to prevent them from being hidden inside cluster aggregates.
* **[REF: MAP-11] Cluster Metadata Aggregation:** Map clusters utilize dynamic event listeners (`clustermouseover`) to aggregate and display a list of child-marker tooltips upon user hover.
* **[REF: MAP-UI-02] Cluster Segregation Palette:** To prevent visual confusion with semantic Green and Blue pins, Leaflet `MarkerClusterGroup` CSS is explicitly overridden with a distinct warm palette (Amber, Coral, Muted Purple).
* **[REF: MAP-12] Overview Map (Minimap):** Secondary, non-interactive Leaflet instance synchronized to the primary viewport with a dynamic bounding box (`L.rectangle`).
* **[REF: UI-HUD-01] Semantic Status Dashboard:** Formats mathematical `visibleMs` from the timeline into a semantic string (e.g., "5 Years").
* **[REF: TL-LOCK-01] The Dual-Tier Zoom Lock:** Soft Lock (decouples scaling from navigation until next slide) vs Hard Lock (permanently freezes zoom scalar; navigation triggers panning strictly).
* **[REF: UI-NUM-01] Scalar Data Formatting:** Abstract screen percentages are prohibited. The engine standardizes on `<times>x` notation formatted to a maximum of one decimal place.
* **[REF: MED-21] Spatial Image Navigation:** Direct spatial awareness granted to native `<img>` elements. Bypassing invisible DOM overlays, the engine reads raw mouse coordinates upon interaction. Sub-50% horizontal coordinates scroll left; 50%+ scroll right.
* **[REF: MED-24] Flex-Gravity Text Bounding:** Rigid `flex-direction: column` structure. The metadata block utilizes `flex-shrink: 0` to claim exact typographic height at bottom. Media wrapper employs `flex-grow: 1; min-height: 0;` to consume upward bounds, ensuring text never overlays photographic subjects.
* **[REF: UI-30] Global Spatial Keyboard Listeners:** Native `keydown` intercepts on `window` bind horizontal arrow keys to chronological navigation. Input fields are strictly sandboxed (`document.activeElement`) to prevent typing inputs from triggering jumps.
* **[REF: UI-32] Smart Date Deduplication:** Rendering engine parses Date objects to eliminate redundancy. Midnight timestamps (`00:00:00`) are stripped. Multi-day events sharing identical months and years collapse duplicate strings (e.g., "15 - 18 Mar 1944") to optimize reading.
* **[REF: UI-43] Rigid Minimap Containment (Tablet Anti-Scaling):** Relying on default relative Flexbox sizing allowed the Leaflet minimap to mathematically balloon and consume up to 1/6th of high-resolution portrait tablet displays. Furthermore, Leaflet's `setMaxBounds` calculations frequently collided with extreme aspect-ratio discrepancies. The fix entirely overrides dynamic CSS, hard-locking the containment node via inline geometry to exactly `160px by 160px`. Geographic bounding relies purely on `fitBounds(..., { animate: false })` immediately following spatial ingestion to instantly snap the frame to the payload orientation.
* **[REF: UI-39] Sticky Metadata Dock:** The `ContentSlider` text pane undergoes strict architectural separation. The Event Title and deduplicated Smart Date are locked to the UI ceiling utilizing `position: sticky; top: 0`, guaranteeing the user never loses chronical anchor context during deep scrolling. Furthermore, contextual spatial metadata (Places & Tags) are physically extracted from the prose body and "Top-Docked" directly beneath the sticky header, prioritizing the establishment of physical location before narrative delivery.
* **[REF: UI-40] Golden SVG Tag Identity:** Operating system fragmentation causes catastrophic rendering failures for standard Emojis (e.g., black-and-white wireframes on Linux, missing glyphs on older Android devices). To enforce a unified brand aesthetic, semantic Tag pills abandon native Unicode in favor of a mathematically plotted inline `<svg>` path. The vector is explicitly parameterized with a golden-yellow stroke and translucent fill (`#ffc107`), universally rendering a flawless geometric identity across all browser engines.
* **[REF: UI-39] Sticky Metadata Header (Right-Docked):** The `ContentSlider` text pane pins the Event Title, Smart Date, Places, and Tags to the UI ceiling (`position: sticky`). Contextual metadata (Places & Tags) are dynamically flex-aligned to the right-hand side of the date to maximize vertical reading space for the description body.
* **[REF: UI-40] Golden SVG Tag Identity:** Tags abandon native OS Emojis in favor of a mathematically plotted inline `<svg>` path. The vector is explicitly parameterized with a golden-yellow stroke/fill (`#ffc107`), ensuring flawless geometric identity across all browser engines.
* **[REF: UI-46] Status Bar Space-Saver Menu:** To prevent UI overlapping on narrow portrait displays (mobile/tablet), secondary utility functions (Settings, Monitor) are condensed into a single `☰` menu button. Expanding the menu triggers an absolute-positioned floating popover, preserving critical horizontal space for the central navigation arrows.
* **[REF: UI-32] Smart Date Deduplication (Same-Day Delta):** Date engine strips redundant `00:00` midnights. If an event begins and ends on the exact same Day/Month/Year but at different times, it outputs a pristine time delta (`15:45 — 16:00`).
* **[REF: UI-39] Shrink-Wrapped Sticky Header:** ContentSlider rigidly pins metadata to the UI ceiling. The DOM order is explicitly forced to: **Date ➔ Places ➔ Tags ➔ Title**. Margins and padding are heavily compressed to maximize vertical real-estate for the description.
* **[REF: UI-40] Golden SVG Tag Identity:** Tags utilize a mathematically plotted inline `<svg>` path (`#ffc107`), overriding OS-level Emojis.
* **[REF: UI-46] Left-Aligned Menu Dock:** Secondary tools (Settings, Monitor) are condensed into a single `☰ Menu` button absolutely anchored to the far-left of the status bar, freeing portrait space. The aesthetic CSS gap inside the About logo was eliminated to sit perfectly flush.
* **[REF: UI-47] The 75ch Readability Clamp:** To prevent severe eye-fatigue on wide screens (without breaking vertical scrolling via CSS columns), the `.slide-desc` block is mathematically clamped to `max-width: 75ch` and horizontally centered.
* **[REF: UI-32] Smart Date Deduplication (Same-Day Delta):** The formatting engine mathematically strips midnight `00:00:00` signatures. If an event begins and ends on the exact same Day/Month/Year but at different times, the engine isolates the time signature and outputs a clean temporal delta (e.g., `15:45 — 16:00`), entirely eliminating redundant calendar string generation.
* **[REF: UI-39] Shrink-Wrapped Sticky Header (Direct-Child Architecture):** The `ContentSlider` enforces a strict geographic-first hierarchy. The DOM layout is mechanically forced to: **Date ➔ Places ➔ Tags ➔ Title**. 
    * *The Physics:* Previously, the header failed to stick because it was housed inside an intermediate flex-wrapper that stretched infinitely. The DOM was refactored to make the header a *direct child* of the `overflow-y: auto` parent. This mechanically guarantees the header physically freezes at `top: 0` while the description body scrolls underneath it.
    * *The Aesthetics:* Padding and margins are heavily compressed to absolute minimums, "shrink-wrapping" the content to maximize vertical reading real-estate.
* **[REF: UI-40] Golden SVG Tag Identity:** Semantic Tags abandon native OS Emojis in favor of a mathematically plotted inline `<svg>` path. The vector is explicitly parameterized with a golden-yellow stroke and fill (`#ffc107`), universally rendering a flawless geometric identity across all browser engines.
* **[REF: UI-46] Left-Aligned Menu Dock:** To preserve critical horizontal space for the central navigation arrows on narrow portrait screens, secondary utility tools (Settings, Monitor) are condensed into a single popover menu. The `☰ Menu` button is rigidly anchored to the absolute far-left edge of the status bar. The aesthetic CSS gap inside the `CarTiMapperLogo` component was eliminated so the version number sits perfectly flush alongside the logotype.
* **[REF: UI-47] The 75ch Readability Clamp:** To prevent severe eye-tracking fatigue on massive wide-screen desktop displays (without resorting to CSS columns, which break vertical scrolling paradigms), the narrative description block (`.slide-desc`) is mathematically clamped to `max-width: 75ch`. The block automatically centers itself, preserving the natural vertical scroll while maintaining an optimal typographical line-length for human reading.
* **[REF: UI-48] Typographical & Geometric Hierarchy:** To ensure the Event Title remains the dominant visual anchor, the Date string's `font-weight` is explicitly reduced to standard (`400`). Places and Tags are extracted into a dedicated Flexbox column strictly governed by `align-items: flex-end`. This forces the metadata badges to stack vertically flush against the right-hand container boundary, creating a clean geometric counter-weight to the left-aligned date.
* **[REF: UI-49] Typographical Outset (The Hero Overhang):** To solve the "wide-screen void" caused by fully justifying the Sticky Header metadata across ultra-wide monitors, the header contents are mathematically wrapped in a specific geometry limit. The header utilizes a `max-width: 90ch` envelope, while the narrative description utilizes a `max-width: 75ch` envelope. 
    * *The Physics:* This pulls the Date and Tags firmly toward the visual center of the screen so they remain contextually bound to the text. However, because the header is `15ch` wider than the body, it creates a subtle typographic overhang, allowing long Event Titles and Place names to comfortably expand beyond the text margins without forcing ugly, premature line breaks.
* **[REF: UI-50] Modal Recovery Engine:** Ensure standard HTML wrapper elements (`<div id="aboutModal">...</div>`) are strictly returned alongside the primary `ContentSlider` JSX, guaranteeing absolute-positioned UI overlays correctly trigger their visibility state (`display: flex`) without crashing the Virtual DOM depth-tree.
* **[REF: UI-51] Fluid Axis Rotation (The "Reading Room" Architecture):** To permanently solve the "white-space void" on massive desktop monitors without relying on unreadable CSS text columns, the entire architectural skeleton of the application was rebuilt using CSS Flexbox Order and Fluid Resizers.
    * *The Physics (< 1024px Portrait):* The layout stack is vertical. The `visual-pane` (Map & Media side-by-side) sits on top (`order: 1`). The text pane (`content-slider`) sits underneath (`order: 3`). The primary geometric resizer (`order: 2`) acts as a horizontal drag-bar, calculating mathematical deltas on the Y-Axis.
    * *The Physics (>= 1024px Desktop):* The instant the browser hits the 1024px media query, the entire geometric axis rotates 90 degrees. The Text pane is hoisted to the far-left (`order: 1`), claiming 50-60% of the screen. The Map & Media are pushed to the right sidebar (`order: 3`) and stacked vertically. The Javascript resizer strictly detects this orientation shift (`isDesktop`) and mathematically switches its calculation logic to the X-Axis. 
    * *The Aesthetics:* This establishes a professional "Dashboard" format, seamlessly transforming an empty landscape screen into a high-density intelligence layout.
* **[REF: UI-52] Layout Boundary Mathematical Reset:** When users snap browser windows or rotate mobile devices, CSS variables retaining older, incompatible split percentages (`--primary-split`) caused severe layout crushing. The global engine now binds a `useEffect` to `window.innerWidth`. The absolute instant the boundary crosses the `1024px` threshold, the app automatically zeroes both CSS structural variables back to `50%`, ensuring perfect proportional integrity.
* **[REF: UI-53] Dynamic Secondary Viewport Collapse:** To preserve critical screen real-estate, the Media pane utilizes conditional DOM presence. If `activeSlide.media.length === 0`, the engine injects a `.no-media` class into the visual pane. This absolutely hides the Media pane and Secondary Resizer via `display: none`, and seamlessly expands the Map instance to consume `100%` of the available quadrant.
* **[REF: UI-54] Bottom-Up Timeline Resizer Physics:** The horizontal Timeline uses a dedicated structural `.resizer-dyn-timeline`. Because it is anchored to the bottom of the screen, the mathematical event listener calculates its delta backward (`deltaY = currentY - startY` against `startHeight - delta`), allowing intuitive "drag up to enlarge" physics.

---

## VI. Algorithms, Analytics & Methodologies

* **1. Spatial Indexing (The R-Tree Engine):** Rather than measuring point-to-point distance (which freezes the browser), `MarkerCluster` uses `RBush` to divide the map into nested rectangular grids. It measures point-to-grid, allowing instant, `O(log n)` collision detection.
* **2. State-Based Layer Swapping & The O(1) Dictionary:** To bypass Leaflet's internal `_leaflet_id` amnesia, the engine builds a global dictionary (`markersRef`). Slide changes execute an `O(1)` instant lookup to swap layers in and out of the cluster bucket seamlessly.
* **3. The Dual-Heuristic Density Math (Timeline Auto-Zoom):** Solves the "Outer Space" problem by evaluating two vectors: `collisionZoom` (The 10px Rule for preventing overlap on the same lane) and `contextZoom` (The 150px Rule for keeping the nearest event visible). Executes `Math.max()` to adopt the safest zoom.
* **4. The URL Routing Engine:** `[REF: URL-01]` Upon data load, the engine intercepts CLI/URL parameters. If `?date=YYYY-MM-DD` is provided, it executes a mathematical distance calculation across the entire `valid` chronological array to locate the closest absolute time-node and injects it as the `activeIndex`. `?slide=X` overrides chronology to focus on a specific narrative slide. `?theme=dark` manipulates root CSS properties prior to physical DOM render.
* **5. The Visual-Center Scrolling Algorithm:** Calculates `blockPixelWidth` dynamically. Plots the geometric center (`StartPx + Width/2`). Commands the scrollbar to target `visualCenterPx - (Screen Width / 2)` to perfectly center wide text blocks.
* **6. HTML Filter Bifurcation:** Timeline Hexagons are processed through `stripHTML()` to guarantee layout breaks (`<br>`) do not physically break geometric rendering. Content Pane Titles are processed through `dangerouslySetInnerHTML` to permit manual formatting.

---

## VII. System Stability & Error Boundaries

* **[REF: BOOT-CRASH-01] Boot Safety Validation:** To prevent fatal unrecoverable blank screens, all Native JS constructors (`new URLSearchParams`) must be strictly validated against syntax typos prior to React's first render hook.
* **[REF: TL-CLAMP-01] Span Geometry Clamp:** The Timeline Scrubber must strictly enforce a minimum temporal visual width of 24 hours (`86400000` ms) to prevent `0px` layout collapse in single-event datasets.
* **[REF: TL-CLAMP-02] Infinity Geometry Clamp:** The Timeline Scrubber's automatic scaling math must enforce a strict `60000` ms floor on event time gaps. This mathematically ensures the engine never divides by zero and triggers an infinite zoom collapse when multiple events share an identical timestamp.
* **[REF: DIAG-01] Active Sensor Telemetry:** The Vibe Monitor must passively expose the application version manifest and the raw location string of the currently active dataset row to facilitate real-time coordinate validation for Leaflet `GEOMETRYCOLLECTION` geometries.
* **[REF: DIAG-02] Metadata Manifest:** The Application 'About' Modal acts as a system manifest, exposing current URL parameters, usage instructions, and the active version footprint.
* **[REF: CRASH-01] Library Polyfill Injection:** Deprecated dependencies (`wicket 1.3.8`) fail to support modern composite geometry strings. The `MapViewer` implements a native string-interception polyfill that explicitly deconstructs `GEOMETRYCOLLECTION` wrappers and passes isolated internal primitives through the parser.
* **[REF: CRASH-02] React Prop Continuity:** Cross-component navigational functions (e.g., `jumpToSlide`) must be explicitly passed via React component props to prevent fatal unrecoverable ReferenceErrors inside isolated Modals.

---

## VIII. Core Engine & State Management

* **[REF: PERF-01] Strict Mode Memory Leak Assassination:** The global keyboard spatial navigation engine (`AppOrchestrator`) was refactored to decouple history state mutation (`setSlideHistory`) from the active state update loop. By wrapping the side-effects in a mathematically pure `jumpToSlide` closure, we prevent recursive Virtual DOM re-renders, permanently eliminating catastrophic memory stack overflows during rapid arrow-key navigation.
* **[REF: PERF-06] Canvas Memory Leak Assassination (Singleton Hoisting):** To automatically wrap long narrative titles into visually pleasing 2 or 3 line breaks, the Timeline relies on a `measureTextWidth` algorithm. Because HTML canvas interactions are invisible and heavily reliant on browser rendering engines, invoking `document.createElement('canvas')` iteratively inside the active rendering loop bypasses the Javascript Garbage Collector (GC). Booting a 500-event timeline physically generated and abandoned over 40,000 isolated DOM context nodes in milliseconds, causing an Out-Of-Memory system crash. The architecture completely eradicates this failure by hoisting the mathematical tool entirely outside the React lifecycle. A singular, immutable `_globalTmCtx` singleton variable is instantiated once during script load and serves as a permanent, zero-leak mathematical reference for all global geometry calculations.
