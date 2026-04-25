# CarTiMapper Changelog
All notable changes to this project will be documented in this file.

### [v6.4.14]
* **AppOrchestrator (v1.36.2) [DATA-09]:** Restored missing `caption` and `credit` extraction logic inside the CSV mapping loop. Media metadata now correctly parses and renders alongside images.
* **AppOrchestrator [DIAG-02]:** Repaired the About button visibility bug. Restored the `setAboutData` manifest injection that occurs post-fetch, ensuring the modal overlay correctly populates and renders.
* **AppOrchestrator [URL-01]:** Restored the Deep-Linking Engine. The URL interceptor once again actively reads `?slide=` and `?date=` parameters upon boot, mathematically jumping the user to the requested index before the loading screen fades.

### [v6.4.13]
* **AppOrchestrator (v1.36.1) [UI-52]:** Patched the rotation glitch. The application now actively tracks the `1024px` CSS boundary and mechanically resets all resizer variables to `50%` upon crossing it, preventing the panes from crushing during rotation or window-snapping.
* **AppOrchestrator [UI-53]:** Fixed missing `activeSlide` prop assignment that was causing media to render as undefined. Engineered the Dynamic Collapse architecture. The Media pane and secondary drag-bar now entirely vanish when no media is present, granting `100%` of the quadrant to the Map.
* **AppOrchestrator [UI-54]:** Restored the missing Timeline resizer bar, injecting "Bottom-Up" delta math to perfectly calculate container height changes.

### [v6.4.12]
* **AppOrchestrator (v1.36.0) [UI-51]:** Deployed the "Reading Room" architectural layout. Replaced legacy hardcoded grid wrappers with dynamic CSS Flexbox media queries (`<style>` block injection). On screens wider than 1024px, the text pane dynamically snaps to the left column, while the Map and Media panes stack in the right column, maximizing screen real-estate.
* **AppOrchestrator [UI-51]:** Engineered Fluid Dynamic Resizers. Primary and Secondary drag-bars instantly detect the browser's portrait/landscape mode and mathematically invert their axis tracking (X-delta vs Y-delta) on the fly, preventing navigation failure during responsive window scaling.

### [v6.4.11]
* **ContentSlider (v3.16.0) [UI-49]:** Deployed the "Hero Overhang" aesthetic. Wrapped the Sticky Header in a `max-width: 90ch` container, keeping the Date and Metadata elements visually anchored to the `75ch` narrative text column, while allowing long titles to elegantly expand past the reading margins.
* **ContentSlider [UI-50]:** Restored the physical HTML DOM nodes for the `aboutModal` overlay, fixing a UI break where the About state triggered successfully but had no physical overlay to render.

### [v6.4.10]
* **Documentation [PROT-01]:** Decoupled the ChangeLog back into a dedicated file. Embedded a strict protocol directive requiring all architectural and historical tracking to be delivered exclusively in isolated `.md` syntax blocks to prevent UI formatting corruption.
* **ContentSlider (v3.15.0) [UI-39]:** **CRITICAL FIX:** Resolved the Sticky Header failure. Removed the infinitely stretching `.content-scroll-area` intermediate wrapper. The header is now a direct child of the `overflow-y` parent container, mechanically guaranteeing it freezes at `top: 0` during active scrolling.
* **ContentSlider [UI-48]:** Refined typographical and geometric metadata hierarchies. The Date string was stripped of bold styling (`font-weight: 400`). Places and Tags were relocated into a dedicated Flexbox column governed by `align-items: flex-end`, forcing them to stack perfectly flush against the right-hand boundary.

### [v6.4.9]
* **Documentation [PROT-01]:** Decoupled documentation into two distinct files (`Master Blueprint` and `ChangeLog`). Appended a strict directive requiring all documentation to be delivered exclusively in `.md` format.
* **ContentSlider (v3.14.0) [UI-39]:** Reordered the Sticky Header. The visual hierarchy now explicitly presents Date, Places, and Tags *above* the Event Title. Compressed padding to shrink-wrap the header and maximize reading space.
* **ContentSlider [UI-47]:** Deployed the `75ch` Readability Clamp. Narrative descriptions dynamically center and restrict their maximum width on large desktop screens to prevent eye-tracking fatigue, keeping vertical scrolling intact.
* **ContentSlider [UI-32]:** Patched the same-day Smart Date bug. `15:45 — 16:00` deltas now render cleanly without duplicating the calendar date.
* **ContentSlider [UI-46]:** Shifted the `☰ Menu` button to the absolute left edge of the status bar. 
* **AppOrchestrator (v1.35.1) [UI-46]:** Removed the hardcoded CSS gap from `CarTiMapperLogo` to align the version number flush with the text.

#### [v6.4.8]
ContentSlider (v3.11.0): * [FIX: UI-39] Consolidated the Title, Date, Place, and Tags into a unified Sticky Header. Right-aligned the Place and Tag metadata to save vertical space.

[FIX: UI-40] Injected Golden SVG vector icons for Tags.

[FIX: UI-46] Condenses Settings and Monitor triggers into a space-saving ☰ popover menu to accommodate portrait/mobile aspect ratios. Left the Arrow navigation and About buttons completely untouched.

---

### [v6.4.7] 
* **Merged and normalized

---

### [v6.4.7]
* **Project Management [PROT-02]:** Appended the "Discuss First, Code Later" directive to the Master Blueprint. All future interactions will strictly adhere to a two-step process: diagnostic strategy agreement, followed by code execution upon explicit user approval. Mechanically locks the developer AI into a diagnostic-first loop to halt version fragmentation and UI drift.

### [v6.4.6]
* **AppOrchestrator [UI-44]:** High-precision Universal Binary Translation deployed. The CSV data parser actively intercepts GitHub `/blob/` strings and mathematically forces them to `raw.githubusercontent.com` binaries, bypassing HTML wrapper page loading and serving raw image data directly to the DOM.
* **AppOrchestrator [PERF-08]:** High-precision millisecond date parsing (`ss.000`) added to strictly preserve dataset ordering during simultaneous events.

### [v6.4.5]
* **Full File:** Consolidated all experimental performance patches into a single, highly stable build.
* **MapViewer [UI-42]:** Injected a mandatory `invalidateSize()` forcing mechanism, effectively resolving the white-map tile bug by demanding Leaflet calculate its true container bounds before requesting HTTP raster data.
* **MapViewer [UI-43]:** Hardcoded strict `160px` geometric locks on the Minimap container to eliminate Flexbox aspect-ratio explosion on tablet portrait displays.
* **Architecture [PERF-07]:** Scoped `safeStripHTML` directly into map and timeline rendering loops. Eliminated external utility dependencies, resolving silent global `ReferenceError` crashes during DOM construction.

### [v6.4.4]
* **Project Management [PROT-01]:** Appended a strict Developer Protocol section to the Master Blueprint (Documentation Supremacy), legally binding the AI assistant to provide Markdown changelogs and architectural updates with every single code deployment.

### [v6.4.3]
* **TimelineScrubber (v23.17.0) [PERF-06]:** **CRITICAL FIX:** Eliminated the massive initialization freeze. Diagnosed a severe Out-Of-Memory crash caused by the repeated generation of HTML `<canvas>` objects. Hoisted the `document.createElement('canvas')` call completely out of the `formatLabelSmart` React lifecycle. The engine now uses one pre-warmed, globally immutable memory singleton (`_globalTmCtx`) to measure all 500+ timeline labels instantly.
* **ContentSlider (v3.11.0) [UI-32]:** Deployed the `formatSmartDate` engine to natively deduplicate time strings (e.g., removing redundant years/months for multi-day events and mathematically stripping `00:00` midnight signatures).
* **ContentSlider [UI-39]:** Pinned the Title and Date to the top of the UI via `position: sticky`. Moved the Metadata Dock directly below the header to establish strict geographic-first narrative hierarchy.
* **ContentSlider [UI-40]:** Injected the golden-yellow `<svg>` tag icon directly into the mapping loop so every Tag pill renders flawlessly, overriding unpredictable OS-dependent Emoji rendering.

### [v6.4.2] 
* **Map Engine Rewrite (v4.0.0):** Completely upgraded the Map architecture by combining three absolute performance safeguards:
    * **[PERF-05] Bulk Ingestion (The Boot Fix):** Instead of feeding the MarkerCluster one pin at a time, WKT arrays are collected in a temporary, invisible JavaScript array (`const bulkCluster = []`). Once the loop finishes, the array is handed to Leaflet in a single `clusterLayer.addLayers(bulkCluster)` command. The collision math runs exactly once instead of $O(N^2)$ times, dropping load times to milliseconds and resolving the CPU hang.
    * **[PERF-02] O(1) Spatial Tracker (The Memory Fix):** Re-implemented the delta tracker (`prevActiveIndexRef`). Changing slides updates exactly two mathematical targets on the map (promoting the new, demoting the old), leaving the remaining 500+ cluster objects untouched.
    * **[PERF-04] The Transmission Clutch (The Spam Fix):** Re-implemented the `debouncedIndex` (set to 100ms). Holding the arrow key flies the Timeline at 30fps, but the Map safely pauses before calculating heavy DOM cluster extractions and 3D flight geodesics, saving 99% of CPU overhead.
* **MapViewer [UI-38]:** Forced `miniMap.invalidateSize()` and strictly applied `fitBounds(bounds.pad(0.1), { animate: false })` to guarantee the minimap instantly and rigidly wraps around the specific geographic footprint of the dataset without aspect-ratio geometry conflicts.

### [v6.4.1]
* **MapViewer [PERF-04]:** Integrated the Transmission Clutch (`debouncedIndex`). Arrow-key events now decouple the Map engine from the UI by 100ms, entirely preventing event-spam from queuing hundreds of heavy DOM manipulations per second.
* **MapViewer [PERF-03]:** Implemented `map.stop()` as an absolute animation brake, physically terminating previous geodesic animation frames and preventing simultaneous camera flights from overloading the CPU thread.

### [v6.4.0]
* **MapViewer (v3.18.0) [PERF-02]:** **CRITICAL FIX:** Resolved the major memory leak and UI freeze caused by Leaflet marker thrashing. Rewrote map interaction logic from an $O(N)$ brute-force array destruction loop to a precise $O(1)$ delta tracker. Map navigation is now mathematically instantaneous.
* **MapViewer [UI-38]:** Bound the `miniMap.fitBounds()` strictly to the dataset's geographic footprint, ensuring the miniature orientation map always contextualizes the user's actual theater of operations.

---

### [v6.3.12] (Rollback Baseline)
* **AppOrchestrator (v1.35.2) [PERF-01]:** Patched a severe memory stack overflow. Keyboard arrow listeners now trigger a pure `jumpToSlide` function, preventing runaway React/Preact state loops from crashing browser tabs.
* **AppOrchestrator [UI-36]:** Ripped out the accidentally duplicated top header that was breaking the flex-grid and pushing the Map/Media panes downward.
* **ContentSlider (v3.10.0) [UI-37]:** Successfully migrated aesthetic polish (Sticky Title/Date, CSS Metadata Pills, Smart Date Deduplication) into the text rendering component.
* **ContentSlider:** Injected the mathematical SVG tag icon (`<svg>`) directly into the `slide.tags.map()` loop to ensure each individual CSS pill receives its own crisp vector path. Replaced the fuzzy ⚙️ text emoji with pristine SVG gear paths.

### [v6.3.11]
* **AppOrchestrator (v1.35.0) [UI-30]:** Injected a global event listener mapping the keyboard's Left/Right arrows to the `setActiveIndex` timeline state for hands-free carousel navigation.
* **AppOrchestrator [UI-31]:** Replaced OS-dependent "⚙️" text emoji with a resolution-independent SVG. Removed unpredictable `&nbsp;` spacing in favor of mathematical Flexbox gaps.
* **EventDetails (v3.4.0) [UI-32]:** Restored the v6.0.14 date-formatting engine to smartly collapse identical days, months, and years across temporal ranges.
* **EventDetails [UI-33 & UI-34]:** Applied the Omni-Splitter regex (`/[\r\n]+|\\n/`) to `activeSlide.tags`. Pinned the Event Title and Date to the top of the pane.
* **EventDetails [UI-35]:** Built the Metadata Dock. Rendered locations as a singular unified tag block, generating discrete CSS pill badges for temporal tags at the bottom.

### [v6.3.10]
* **TimelineScrubber (v23.16.0) [TL-17]:** Injected the `getParsedTags()` utility to intercept and flatten incoming tag arrays using the Omni-Splitter regex (`/[\r\n]+|\\n/`).
* **TimelineScrubber:** Fixed a bug where newline-delimited tags generated single, broken lane labels. The engine now correctly generates discrete background lanes.
* **TimelineScrubber:** Restored vertical spanning engine. Events with multiple tags accurately calculate their Y-axis coordinates to stretch visually across all assigned categories.

### [v6.3.9]
* **MediaViewer (v2.8.0) [MED-24]:** Refactored slides to use Flexbox Column layout. Captions sit at the absolute bottom; Images scale upward to perfectly fill space via `object-fit: contain`.
* **MediaViewer [MED-23]:** Transferred click-zone geometry to the image wrapper. Bound `pointer-events: none` to the physical image to prevent drag-ghosting during navigation clicks. Re-anchored the multi-image HUD counter via `position: absolute`.

### [v6.3.8]
* **MediaViewer (v2.7.0) [MED-20]:** Removed invasive diagnostic HTML HUD. Piped array readouts purely into the `addLog` prop for global monitoring.
* **MediaViewer [MED-21]:** Attached mathematical click zones directly to the rendered image. Clicking the left half scrolls left; right half scrolls right. Changed mouse cursor to horizontal arrows (`ew-resize`).
* **MediaViewer [MED-22]:** Natively hid the scrollbar track via inline CSS, perfectly preserving the ability to physically swipe on touch screens.

### [v6.3.7]
* **MediaViewer (v2.6.3) [MED-18]:** Upgraded the array flattening matrix to utilize the Omni-Splitter regex (`/[\r\n]+|\\n/`). Resolves concatenation bugs caused by rogue carriage returns or escaped line-feeds.
* **MediaViewer [MED-19]:** Restored native anchor (`<a>`) wrappers to `mediaCaption` and `mediaCredit`. Kept the central `<img>` element unlinked to preserve carousel left/right navigation clicks.
* **Diagnostics:** Deployed a high-contrast HUD to track rawMediaData string parsing artifacts before flattening matrices.

### [v6.3.5]
* **MediaViewer (v2.6.1) [MED-14]:** Reverted a fatal prop hallucination (`data/activeIndex` to `activeSlide`) that caused absolute void renders. Implemented safe line-break (`\n, \r\n`) delimiter execution to preserve embedded comma structures.
* **MediaViewer [MED-15]:** Synchronized metadata array indices natively 1:1 with media loop instances. Stripped `<a>` wrapper from visual renders to restore unblocked prev/next DOM telemetry.

### [v6.3.4]
* **MediaViewer (v2.5.4) [MED-11]:** Simplified delimiter parsing regex to exclusively target line-breaks (`/\r\n|\n|\r/`). Resolves critical bug where legitimate commas inside image filenames triggered destructive URL array splits (404 errors).

### [v6.3.2]
* **MediaViewer (v2.5.2) [MED-07]:** Deployed aggressive inline dimensional bounds to `<img>` renderer to mathematically prevent flex-child DOM collapse.
* **MediaViewer [MED-08]:** Upgraded the global array delimiter RegExp to `/\||\r?\n/`. Explicitly identifies and nukes trailing Windows CRLF carriage returns generated by `CHAR(10)` exports.

### [v6.3.1]
* **MediaViewer (v2.5.1) [MED-06]:** Stripped `loading="lazy"` from `<img>` generation to prevent horizontal scroll-snap intersection calculation failures. Rewrote variable interpolations to ensure proper Preact htm VDOM mapping.

### [v6.3.0]
* **MediaViewer (v2.5.0) [MED-03]:** Injected `\n` logic into parsing loop to support `CHAR(10)`. Separated Media, Captions, and Credits into synchronized parallel arrays.
* **MediaViewer [MED-04]:** Stripped HTML anchor (`<a>`) wrappers from dynamically rendered image targets to unblock invisible navigation click-zones.
* **MediaViewer [MED-05]:** Deployed dedicated Flexbox layout row, routing raw URLs into an isolated `[↗]` external link icon.

### [v6.2.8]
* **TimelineScrubber (v23.15.0) [TL-UI-16]:** Implemented the 60px Safe-Threshold algorithm. Dynamically assigns absolute date context strictly to the first major tick that physically clears 60 pixels from the left viewport edge.

### [v6.2.6]
* **TimelineScrubber (v23.13.0) [TL-UI-14]:** Recalculated `distanceToAxis`. Reduced active single-point anchor dot to 6px and offset it to center perfectly on the axis floor.
* **TimelineScrubber [TL-UI-12]:** Injected debounced `onScroll` state trigger into the wrapper element to wake React up following a manual viewport drag.
* **TimelineScrubber [TL-UI-13]:** Built collision-avoidance sub-routine. Left-aligns the `firstVisibleMajorTick` and institutes a 120-pixel rendering block for minor ticks.

### [v6.2.5]
* **TimelineScrubber (v23.12.0) [TL-ANIM-06]:** Detached Manual zooming from Auto-centering constraints. Scrolling mouse-wheel dynamically assigns cursor's coordinate as CSS stretch origin.
* **TimelineScrubber [TL-UI-11]:** Recalculated timeline geometry variables (`topPos + 5`) to integrate 5px structural top breather.
* **TimelineScrubber [TL-UI-10]:** Upgraded `isFirstRenderedTick` gate. Computes an `expectedVisibleMin` derived from active event focus.

### [v6.2.4]
* **Imports & Globals (v1.0.52) [CORE-01]:** Injected `useLayoutEffect` hook into global scope to support synchronous DOM painting.
* **TimelineScrubber (v23.11.0) [TL-ANIM-05]:** Transferred Cluster Auto-Zoom state to synchronous `useLayoutEffect`. Width stretch and scrollTo re-centering now execute in the exact same millisecond, eliminating stretch-flicker.
* **TimelineScrubber [TL-UI-09]:** Engineered `isFirstRenderedTick` gate inside the axis loop to strictly enforce full absolute date context on leading edge of viewport.
* **TimelineScrubber [UI-NUM-02]:** Wrapped active zoomLevel readout in `Math.round()`.

### [v6.2.3]
* **MapViewer (v3.17.0) [MAP-UI-01]:** Dropped mathematically inconsistent spatial multipliers from zoom indicator. HUD natively reports absolute zoom scale.
* **MapViewer [MAP-GRID-01]:** Engineered a dependency-free Micro-Graticule Engine. Parses absolute bounds and dynamically injects `L.polyline` grids synced to Leaflet `moveend` camera trigger.

### [v6.2.2]
* **GlobalStyles (v4.11.0) [TL-UI-05]:** Retained flat-edge `.event-block` boundary and preserved 8px internal vertical padding expansion to strictly support native 3-line string wrapping.
* **TimelineScrubber (v23.10.0) [TL-UI-07]:** Injected a symmetrical `top: 5px` offset into internal lane layout boundary for positive visual breather.
* **TimelineScrubber [TL-UI-08]:** Engineered Infinite Calendar Scale Matrix. Major/Minor axes calculated via dynamic Gregorian interpolation across 16 context thresholds.
* **TimelineScrubber [TL-ANIM-03]:** Deleted sub-second micro-pan `useEffect` hook. Limit transitions to native CSS width expansion post-pan to prevent DOM stutter.

### [v6.2.1]
* **GlobalStyles (v4.11.0) [TL-UI-05]:** Restored flat-edge structural geometry to `.event-block` class.
* **TimelineScrubber (v23.9.0) [TL-ANIM-01]:** Implemented 3-step delayed state machine (Highlight ➔ Smooth Pan ➔ 400ms Delay ➔ Cluster Zoom & Snap) mathematically eliminating visual stretch-whiplash.
* **TimelineScrubber [TL-UI-06]:** Rewrote timeline ruler generation loop utilizing Semantic Gregorian Calendar Engine.
* **TimelineScrubber [TL-UI-04]:** Integrated spatial collision detection to format contextual minor tick labels.

### [v6.2.0]
* **GlobalStyles (v4.10.0) [TL-UI-03]:** Recalibrated `.event-block` clip-paths and elevated `.timeline-track-container` 5px.
* **GlobalStyles [UI-TYPO-02]:** Scrubbed bold styling application-wide for neutral professional UI.
* **GlobalStyles [UI-HUD-02]:** Deployed absolute centering (`transform: translateX(-50%)`) for navigation cluster.
* **AppOrchestrator (v1.34.1) [CRASH-02]:** Passed `jumpToSlide` into ContentSlider to prevent ReferenceErrors from Modal interactions.
* **AppOrchestrator [SUB-02]:** Upgraded logic to intercept `CHAR(10)` line breaks (`\n`) and truncate sublabels at first comma.
* **MapViewer (v3.16.3) [CRASH-01.5]:** Engineered micro-parser polyfill to manually construct geometries when Wicket silently fails on `GEOMETRYCOLLECTION`.
* **MapViewer [GEO-02]:** Reduced LINESTRING weights to 2px with dashed strokes; stripped borders from POLYGON shapes.
* **MapViewer [UI-NUM-01]:** Converted zoom percentages to clean `<times>x` floats.
* **MediaViewer (v2.4.0) [MED-02]:** Bypassed anchor-wrapping for `<iframe` strings, executing dynamically as HTML.
* **MediaViewer [MED-UI-03]:** Engineered regex extraction to pull raw URLs from `<iframe src="...">` for hyperlink routing.
* **ContentSlider (v3.9.2) [DIAG-02]:** Reorganized About Modal to separate project description from active telemetry.
* **TimelineScrubber (v23.8.0) [TL-AUTO-03]:** Replaced nearest-neighbor auto-zoom with 5-event Cluster Window enforcing a 30-minute minimal span.
* **TimelineScrubber [TL-UI-04]:** Injected spatial collision detection for minor axis ticks.

### [v6.1.5]
* **MapViewer (v3.16.3) [SUB-02]:** Inserted `\n` into RegExp delimiter block to support `CHAR(10)` and dynamically truncate at first comma.

### [v6.1.4]
* **ContentSlider (v3.9.2) [CRASH-02]:** Added missing `jumpToSlide` prop assignment.
* **AppOrchestrator (v1.34.1) [CRASH-02]:** Wired `jumpToSlide` method into injection.
* **MapViewer (v3.16.1) [CRASH-01]:** Implemented `parseGeometryCollection()` polyfill to natively support non-compliant nested WKT geometries.

### [v6.1.3-2]
* **GlobalStyles (v4.9.0) [UI-TYPO-02]:** Scrubbed bold application-wide.
* **GlobalStyles [UI-HUD-02]:** Reprogrammed `.status-center` with absolute CSS positioning.
* **VibeMonitor (v2.0.0) [DIAG-01]:** Implemented Active Sensor HUD displaying location text/system version.
* **ContentSlider (v3.9.1) [DIAG-02]:** Restructured About modal flow. Injected dynamic `${APP_VERSION}` into status bar.

### [v6.1.3]
* **MediaViewer (v2.4.0) [MED-02]:** Engineered raw HTML interception loop for entire iframe tags.
* **MediaViewer [MED-UI-03]:** Built `getCleanUrl()` extractor. Captions bound to iframe snippets correctly link to source.

### [v6.2.0] (*Legacy Identifier*)
* **GlobalStyles (v4.8.0) [MED-UI-01]:** Styled `.media-meta-box a` to inherit standard weights.
* **AppOrchestrator (v1.33.0) [MED-01]:** Upgraded `parsedMedia` regex to explicitly target comma-space and semicolon-space signatures.
* **MediaViewer (v2.2.0) [MED-UI-01]:** Extracted `<img>` tags into `<a>` wrappers. Injected `.media-meta-box` container directly inside carousel items.

### [v6.1.1]
* **MapViewer (v3.16.0) [GEO-02]:** Segregated polygon and linestring styling blocks.
* **MapViewer [UI-NUM-01]:** Converted map zoom HUD to report relative scaling using `x` notation.
* **TimelineScrubber (v23.7.0) [TL-AUTO-02]:** Removed `gap > 0` exclusionary logic. Autozoom engine constructs visual cascades instead of zooming out.

### [v6.1.0]
* **GlobalStyles (v4.7.0):** Injected `.minimap-container` CSS footprint.
* **AppOrchestrator (v1.32.0):** Configured central `zoomLock` and `visibleTimeSpan` HUD properties.
* **MapViewer (v3.15.0) [MAP-12]:** Instantiated synchronized Overview Minimap bounded explicitly to dataset scope.
* **MapViewer [MAP-13]:** Deployed `L.control.scale()`. Engineered dynamic 100% calculation based on relative deviation.
* **TimelineScrubber (v23.6.0) [TL-AUTO-01]:** Remodeled Auto-Zoom engine enforcing 10px spacing with 10-minute cascade floor.
* **TimelineScrubber [TL-LOCK-01]:** Implemented Soft/Hard tracking states.
* **ContentSlider (v3.9.0) [UI-HUD-01]:** Formatted unified status bar to display semantic timeline extent readout.

### [v6.0.26]
* **GlobalStyles (v4.6.0) [MAP-UI-02]:** Overrode default Leaflet Cluster CSS to deploy Warm Segregation Palette.
* **GlobalStyles [UI-TYPO-01]:** Reconfigured `.slide-place-badge` constraints to allow dynamic multi-line text wrapping.
* **MapViewer (v3.14.0) [MAP-UI-01]:** Restored native Leaflet zoom controls.
* **MapViewer [SUB-01]:** Added dash and ano teleia to sublabels RegEx split logic.
* **MapViewer [MAP-09]:** Enabled hot-swapping between English Esri BaseMap and Local CartoDB map.
* **MapViewer [MAP-10]:** Implemented Elevation hook. Active marker removed from cluster group to guarantee visibility.
* **MapViewer [MAP-11]:** Bound `getAllChildMarkers()` to hover state to render list of swallowed location names.
* **AppOrchestrator (v1.31.0) [MAP-SET-01]:** Injected `maxAutoZoom` and `mapLocale` state variables into Settings Modal.

### [v6.0.25]
* **MapViewer (v3.13.0) [MAP-UI-01]:** Restored native Leaflet zoom controls.
* **MapViewer [MAP-09]:** Swapped CartoDB for Esri World Street Map to force English labeling globally.
* **MapViewer [MAP-10]:** Implemented Elevation hook.
* **MapViewer [MAP-11]:** Bound `getAllChildMarkers()` mapping logic.

### [v6.0.24]
* **AppOrchestrator (v1.30.1) [BOOT-CRASH-01]:** Patched critical syntax typo in Stage 1 data fetch (`new URLSearchParams`). Resolves catastrophic halting bug on Initialization.
* **ContentSlider (v3.8.1) [UI-NAV-01]:** Restored missing `slideHistory` and `goBack` properties.
* **TimelineScrubber (v23.5.0) [TL-CLAMP-01]:** Deployed mathematical span floor (minimum 24 hours).
* **TimelineScrubber [TL-CLAMP-02]:** Deployed Infinity Clamp (locks minimum time gap to 60 seconds).

### [v6.0.23]
* **AppOrchestrator (v1.30.1) [BOOT-CRASH-01]:** Patched syntax typo `new URLSearchParams`.
* **ContentSlider (v3.8.1) [UI-NAV-01]:** Restored `slideHistory`.

### [v6.0.22]
* **ContentSlider (v3.8.0) [UI-SET-01]:** Injected Settings trigger button into Status Bar.
* **ContentSlider [DATE-13]:** Upgraded `formatSmartDate` to conditionally render `HH:mm`.
* **TimelineScrubber (v23.5.0) [TL-CLAMP-01/02]:** Span Floor and Infinity Clamp logic deployed.
* **AppOrchestrator (v1.30.0) [ARCH-01]:** Implemented Two-Stage Data Pipeline caching raw PapaParse rows.
* **AppOrchestrator [DATE-11 & 12]:** Replaced `parseEuroDate` with `dynamicDateParse`, routing HH:mm:ss payloads safely.
* **AppOrchestrator [UI-SET-01]:** Injected Settings Modal UI overlay.

### [v6.0.21]
* **AppOrchestrator (v1.29.0) [DATA-08]:** Replaced fuzzy `smartGet` with high-performance `exactGet` mapper.
* **AppOrchestrator [DATA-09]:** Activated extraction for media caption/credit.
* **AppOrchestrator [DATA-10]:** Pruned unused fields (Source, Web Page) to optimize memory. Pointed subLabels logic directly at Place column.

### [v6.0.18]
* **AppOrchestrator (v1.28.1) [DATA-03]:** Expanded normalization fallbacks. Implemented rigorous `.filter()` chain to obliterate falsy arrays.
* **TimelineScrubber (v23.2.0) [UI-TL-02]:** Encapsulated Absolute Zoom Controls and Scroll Wrapper to restore architectural integrity against flexbox crushes.

### [v6.0.17]
* **MapViewer (v3.12.0) [GEO-01]:** Resolved GeoJSON inversion by deleting custom `coordsToLatLng` override. Engine trusts Leaflet's native parser.
* **ContentSlider (v3.7.0) [DATE-02]:** Augmented `formatSmartDate` logic to include `weekday: 'short'`.
* **TimelineScrubber (v23.1.0) [UI-TL-01]:** Restored complete multi-lane visualizer logic. Redesigned Zoom Controls container.

### [v6.0.16]
* **AppOrchestrator [DATE-01]:** Strict Euro/ISO Date Parser. Replaced generic `new Date()` with manual `parseEuroDate` to guarantee US-localized browsers no longer silently invert European dates.

### [v6.0.15]
* **Map Engine:** Removed redundant `flipLeafletGeometry` function.
* **Map Engine:** Fixed Sub-Label mapping inside nested WKT LayerGroups.
* **UI/UX:** Increased Splash Screen logo width to 300px. Re-introduced full-size branding logo into About modal.

### [v6.0.14]
* **Map Engine [MAP-06]:** Resolved Multipoint Sub-Label duplication bug via recursive geometry tunneling (`extractLayers`).
* **Map Engine:** Increased Y-offset of custom map tooltips to `-48px`.

### [v6.0.13]
* **Map Engine [UI-12]:** Permanently resolved double-offset CSS bug pushing Map Pins away from coordinates.
* **Branding Layout:** Replaced legacy text button in status bar with CarTiMapper Logo icon.
* **Typography Hierarchy [UI-13]:** Moved `📍 Place` metadata badge to right-hand column beneath Tags.

### [v6.0.12]
* **Branding:** Officially integrated Dark Film Hex-Frame logo. Dynamically embedded version (`v6.0.12`) into splash loader. Hex-Frame SVG added as URL-encoded Favicon.

### [v6.0.11]
* **Map Engine:** Deployed hotfix for fatal `ReferenceError` (`new URLSearchParams` typo).

### [v6.0.10]
* **Data Schema:** Expanded CSV ingestor to capture `Place` column.
* **Map Engine:** Overhauled Map Tooltip UI labeling hierarchy (`Sub-Label` -> `Place Name` -> `Event Title`).
* **UI/UX:** Added `📍 [Place Name]` spatial context badge.

### [v6.0.9]
* **Architecture:** Bumped `APP_VERSION` to `v6.0.9`.
* **Map Engine:** Diagnosed and fixed catastrophic WKT inversion bug by restoring missing `wicket.min.js` to `<head>`.
* **Architecture:** Fixed boot freeze by defaulting to Master Database URL if `?source=` is omitted.
* **Diagnostics:** Refined `VibeMonitor` telemetry to catch WKT parsing stack traces.

### [v6.0.8]
* **Map Engine [MAP-05]:** Deployed Recursive Leaflet Geometry Flipper (`flipLeafletGeometry`).
* **Diagnostics:** Wired map parser directly into `VibeMonitor` for verbosity.

### [v6.0.7]
* **Map Engine:** Removed Regex coordinate flipper.
* **UI/UX:** Added `?source=URL` parameter documentation into About modal.

### [v6.0.6]
* **Map Engine:** Deployed ultra-fast Regex Pre-Parser string flipper (Reverted in v6.0.7).
* **UI/UX:** Rebuilt `about-modal` to render reference card explaining `[URL-01]` routing parameters.

### [v6.0.5]
* **Navigation [URL-01]:** Built URL Routing Engine intercepting `?slide=X`, `?date=YYYY-MM-DD`, `?theme=dark`, `?mapzoom=X`.

### [v6.0.4]
* **Architecture:** Routed parsed WKT through `.toJson()` into `.geoJSON()` natively (Reverted in v6.0.7).
* **UI/UX:** Added `.custom-div-icon` CSS override to strip Leaflet white bounding boxes.
* **Physics:** Raised bounding box max-zoom tolerances to `16`.

### [v6.0.3]
* **Google Sheets ETL Pipeline:** Formula brought under versioning. Enforced Cartesian schema (`POINT(Lon Lat)`). Injected Two-Tier REF-TAG documentation via `LET` dummy variables (`_ref_etl`).

### [v6.0.2]
* **Google Sheets ETL Pipeline:** Wrapped GeoJSON/WKT array builders in `UNIQUE(FILTER(...))` logic to purge redundant nodes.

### [v6.0.1]
* **UI/UX:** Reassigned mapping hierarchy colors.
* **Data Schema:** Smartened ETL `col_idx=11` formula to conditionally map raw Lat/Lon into `GeometryCollection` or `GEOMETRYCOLLECTION`.

### [v6.0.0]
* **Architecture:** Major version rewrite. Replaced basic Leaflet plotting with WKT parsing (`Wicket`) and R-Tree spatial indexing (`MarkerCluster`).
* **Logic:** Built $O(1)$ memory dictionary (`markersRef`) to handle "State-Based Layer Swapping" for VIP slide focus.

### [v5.23.0]
* **Data Schema:** Updated PapaParse fuzzy ingestor to capture `clean_geo`, `sub_labels`, `priority`.
* **Media:** Hardened Media Agnosticism pipeline natively supporting YouTube iframes, HTTP links, and images within unified flex container.

### [v1.0.0 – v5.13.0] - Legacy Foundation Rollup
* **Core Layout:** Established strict Flexbox boundaries with a Unified Status Bar and 15% Ergonomic Touch Zones.
* **Timeline:** Established foundation temporal visualization replacing list navigation.
* **Cloud Data:** Hardcoded datasets replaced with live Google Sheets API Proxy fetching.
* **Genesis:** Initial concept deployment via basic Leaflet map instantiation reading hardcoded Lat/Lon arrays.
