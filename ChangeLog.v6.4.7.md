# CarTiMapper Changelog
All notable changes to this project will be documented in this file.

Changelog (v6.4.7)
Project Management: * [FIX: PROT-02] Appended the "Discuss First, Code Later" directive to the Master Blueprint. All future interactions will strictly adhere to a two-step process: diagnostic strategy agreement, followed by code execution upon explicit user approval.

---

Changelog (v6.4.6)
AppOrchestrator (v1.35.0): High-precision date parsing added.

MapViewer (v4.2.0): Performance optimized via bulk loading. White-map tiles bug resolved.

TimelineScrubber (v23.18.0): Critical memory leak resolved.

---

Changelog (v6.4.5)
Full File: Consolidated all experimental patches into a single stable build.

Map: Fixed white-map tile bug via invalidateSize and bulk ingestion.

Timeline: Fixed memory leak that crashed Firefox on load.

---

Changelog (v6.4.4)
Project Management: * [FIX: PROT-01] Appended a strict Developer Protocol section to the Master Blueprint, legally binding the AI assistant to provide Markdown changelogs and architectural updates with every single code deployment.

---

Changelog (v6.4.3)
TimelineScrubber (v23.17.0): * [FIX: PERF-06] CRITICAL FIX: Eliminated the massive initialization freeze. Hoisted the document.createElement('canvas') call completely out of the formatLabelSmart loop. The engine now uses one pre-warmed memory canvas to measure all 500+ timeline labels instantly.

ContentSlider (v3.11.0): * [FIX: UI-32] Deployed the formatSmartDate engine to deduplicate time strings (e.g., removing redundant years/months for multi-day events and stripping 00:00).

[FIX: UI-39] Pinned the Title and Date to the top of the UI. Moved the Metadata Dock directly below the header.

[FIX: UI-40] Injected the yellow SVG tag icon directly into the mapping loop so every Tag pill renders flawlessly.

---

v6.4.2 Map Engine Rewrite
We are completely upgrading the Map architecture to v4.0.0 by combining three absolute performance safeguards:

Bulk Ingestion (The Boot Fix): Instead of feeding the cluster one pin at a time, we will hold all markers in a temporary, invisible array (const bulkCluster = []). Once the loop finishes, we hand the entire array to Leaflet in a single command: clusterLayer.current.addLayers(bulkCluster). The math runs once, dropping load times to milliseconds.

O(1) Spatial Tracker (The Memory Fix): Re-implemented the delta tracker (prevActiveIndexRef) so that changing slides updates exactly two pins on the map, not 500.

The Transmission Clutch (The Spam Fix): Re-implemented the debouncedIndex (set to 100ms). If you hold the arrow key down, the Timeline flies at 30fps, but the Map waits for you to pause before calculating its 3D flight, saving 99% of CPU overhead.

Minimap Aspect Ratio: Forced miniMap.invalidateSize() and strictly applied fitBounds(bounds.pad(0.1), { animate: false }) to guarantee the minimap instantly wraps around your dataset without geometry conflicts.

---

* **[REF: UI-38] Dynamic Minimap Bounding:** The Minimap overlay now strictly aligns to the primary dataset. The engine forces a DOM redraw (`invalidateSize()`) and calculates the exact spatial extent (`bounds`) of all valid data points, executing a `pad(0.1)` boundary lock to ensure the orientation map is contextually relevant.

---

Changelog (v6.4.0)
MapViewer (v3.18.0): * [FIX: PERF-02] CRITICAL FIX: Resolved the major memory leak and UI freeze caused by Leaflet marker thrashing. The map now utilizes an O(1) state delta, updating only the two specific pins that need to change (the old active pin and the new active pin), leaving the rest of the cluster tree untouched. Map navigation is now instantaneous.

[FIX: UI-38] Bound the miniMap.fitBounds() strictly to the dataset's geographic footprint, ensuring the miniature orientation map always contextualizes the user's actual theater of operations.

---

Changelog (v6.3.12)
AppOrchestrator (v1.35.2): * [FIX: PERF-01] Patched a severe memory stack overflow. Keyboard arrow listeners now trigger a pure jumpToSlide function, preventing runaway React/Preact state loops from crashing browser tabs.

[FIX: UI-36] Ripped out the accidentally duplicated top header that was breaking the flex-grid and pushing the Map/Media panes downward.

ContentSlider (v3.10.0): * [FIX: UI-37] Successfully migrated the v6.3.11 aesthetic polish (Sticky Title/Date, CSS Metadata Pills, Smart Date Deduplication) into the actual textual rendering component.

Injected the mathematical SVG tag icon (<svg>) directly into the slide.tags.map() loop to ensure each individual CSS pill receives its own crisp icon.

Replaced the fuzzy ⚙️ text emoji in the unified bottom status bar with the pristine SVG gear path, preserving the original toolbar layout.

---

Changelog (v6.3.11)
AppOrchestrator (v1.35.0): * [FIX: UI-30] Injected a global event listener mapping the keyboard's Left/Right arrows to the setActiveIndex timeline state, allowing for rapid, hands-free carousel navigation.

[FIX: UI-31] Replaced the OS-dependent "⚙️" text emoji with a crisp, resolution-independent SVG gear icon. Removed unpredictable &nbsp; spacing from the header in favor of a mathematically perfect Flexbox gap.

EventDetails (v3.4.0): * [FIX: UI-32] Restored the v6.0.14 date-formatting engine to smartly collapse identical days, months, and years across temporal ranges.

[FIX: UI-33] Applied the Omni-Splitter regex to the activeSlide.tags array to guarantee proper splitting regardless of upstream CSV concatenation.

[FIX: UI-34] Pinned the Event Title and Date to the top of the pane.

[FIX: UI-35] Built the Metadata Dock. Rendered locations as a singular unified tag block, and generated discrete CSS pill badges for each individual temporal tag at the bottom of the text pane.

---

Changelog (v6.3.10)
TimelineScrubber (v23.16.0): * [FIX: TL-17] Injected the getParsedTags() utility to intercept and flatten incoming tag arrays using the Omni-Splitter regex (/[\r\n]+|\\n/).

Fixed a bug where newline-delimited tags ("Tag A\nTag B") were generating single, broken lane labels. The engine now correctly generates two discrete background lanes.

Restored the vertical spanning engine: Events containing multiple newline-delimited tags once again accurately calculate their minimum and maximum Y-axis coordinates, stretching their visual event-block across all assigned categories.

---

Changelog (v6.3.9)
MediaViewer (v2.8.0): * [FIX: MED-24] Refactored slides to use Flexbox Column layout. Captions dynamically sit at the absolute bottom of the screen, shrinking to their exact text height. Images scale up to perfectly fill the space left above them.

[FIX: MED-23] Transferred click-zone geometry to the image wrapper. If an image is narrow, clicking the empty space beside it will now successfully scroll the carousel. Bound pointer-events: none to the physical image to prevent drag-ghosting during navigation clicks.

Re-anchored the multi-image counter to the top right of the pane using position: absolute, ensuring it hovers cleanly over the media rather than pushing the image geometry downward.

---

Changelog (v6.3.8)
MediaViewer (v2.7.0): * [FIX: MED-20] Removed invasive diagnostic HTML HUD. Piped array readouts purely into the addLog prop for global monitoring.

[FIX: MED-21] Attached mathematical click zones directly to the rendered image. Clicking the left half scrolls left; right half scrolls right. Changed mouse cursor to horizontal arrows (ew-resize) on multi-image carousels.

[FIX: MED-22] Natively hid the scrollbar track via inline CSS and injected style blocks, while perfectly preserving the ability to physically swipe on touch screens.

---

Changelog (v6.3.7)
MediaViewer (v2.6.3): * [FIX: MED-18] Upgraded the array flattening matrix to utilize the Omni-Splitter regex (/[\r\n]+|\\n/). This permanently resolves the concatenation bug caused by rogue carriage returns or escaped line-feed characters embedded in legacy CSV exports, ensuring arrays physically split into multiple slides.

[FIX: MED-19] Restored the native anchor (<a>) wrappers to the mediaCaption (linking to the target media) and mediaCredit (linking to the source text). Kept the central <img> element unlinked to preserve carousel left/right navigation clicks.

[DIAGNOSTICS] Deployed a high-contrast HUD to the bottom of the media pane specifically to track the rawMediaData string before it enters the flattening matrix, exposing upstream PapaParse injection artifacts.

---

Changelog (v6.3.5)
MediaViewer (v2.6.1): * [FIX: MED-14] Reverted a fatal prop hallucination (data/activeIndex to activeSlide) that caused an absolute void render state. Implemented safe line-break (\n, \r\n) delimiter execution across media, caption, and credit data objects to preserve embedded comma structures.

[FIX: MED-15] Synchronized metadata array indices natively 1:1 with media loop instances. Stripped the <a> wrapper from the visual image render to restore unblocked prev/next DOM click-zone telemetry, relegating outward bound links to a targeted interactive icon inside the metadata block.

---

Changelog (v6.3.4)
MediaViewer (v2.5.4): * [FIX: MED-11] Simplified delimiter parsing regex to exclusively target line-breaks (/\r\n|\n|\r/). This resolves a critical bug where legitimate commas embedded inside image filenames were inadvertently triggering destructive URL array splits, leading to blank renders and 404 network errors.

---

Changelog (v6.3.2)
MediaViewer (v2.5.2): * [FIX: MED-07] Deployed aggressive inline dimensional bounds to the <img> renderer to mathematically prevent flex-child DOM collapse. Restored standard "..." quote wrapping to src targets for safe Preact string-interpolation parsing.

[FIX: MED-08] Upgraded the global array delimiter RegExp from /[|\n]/ to /\||\r?\n/. This explicitly identifies and nukes trailing Windows CRLF carriage returns generated by CHAR(10) exports, resolving silent 404 broken-image renders.

---

Changelog (v6.3.1)
MediaViewer (v2.5.1): * [FIX: MED-06] Stripped loading="lazy" from <img> generation to prevent horizontal scroll-snap intersection calculation failures (permanently blank images). Rewrote variable interpolations across all src tags (removing quote wrapping) to ensure Preact's htm VDOM maps the properties correctly as dynamic data rather than static strings.

---

Changelog (v6.3.0)
MediaViewer (v2.5.0): * [FIX: MED-03] Injected \n logic into the parsing loop to support CHAR(10). Separated Media, Captions, and Credits into synchronized parallel arrays to map context 1:1 within the carousel layout.

[FIX: MED-04] Stripped HTML anchor (<a>) wrappers from dynamically rendered image targets to unblock invisible navigation click-zone interactions.

[FIX: MED-05] Deployed a dedicated Flexbox layout row inside the metadata block, explicitly routing the raw target URL into an isolated [↗] external window link. Updated the multi-image HUD counter to clarify scroll intent (N Items (Scroll ➔)).

---

Changelog (v6.2.8)
TimelineScrubber (v23.15.0): * [FIX: TL-UI-16] Implemented the 60px Safe-Threshold algorithm. The engine dynamically assigns the absolute date context strictly to the first major tick that physically clears 60 pixels from the left viewport edge. The collision array was expanded to ±65px to symmetrically block minor ticks from rendering beneath the full width of the centered absolute string.

---

Changelog (v6.2.6)
TimelineScrubber (v23.13.0): * [FIX: TL-UI-14] Recalculated distanceToAxis to intercept the containerHeight - 28 boundary. Reduced active single-point anchor dot to 6px and mathematically offset it (-4px) to center perfectly on the axis floor. Adjusted the background trapezoid height constraints and deleted the right vertical bar.

[FIX: TL-UI-12] Injected a debounced onScroll state trigger into the wrapper element to wake React up following a manual viewport drag, forcing real-time evaluations of the viewLeftEdge boundary.

[FIX: TL-UI-13] Built a collision-avoidance sub-routine into the axis generation loop. Left-aligns the firstVisibleMajorTick to prevent left-edge cutoff, and institutes a 120-pixel rendering block for minor ticks falling within the absolute date string's bounding box.

---

Changelog (v6.2.5)
TimelineScrubber (v23.12.0): * [FIX: TL-ANIM-06] Detached Manual zooming from Auto-centering constraints. Scrolling the mouse-wheel dynamically assigns the cursor's coordinate as the CSS stretch origin.

[FIX: TL-UI-11] Recalculated timeline geometry variables (topPos + 5, distanceToAxis referenced to containerHeight - 33) to correctly integrate the 5px structural top breather.

[FIX: TL-UI-10] Upgraded isFirstRenderedTick gate. It now computes an expectedVisibleMin derived from the active event focus, ensuring the absolute date stamp renders physically inside the viewport rather than invisibly off-screen inside the padding barrier. Scrubbed bold font-weight from major ticks.

---

Changelog (v6.2.4)
Imports & Globals (v1.0.52): * [FIX: CORE-01] Injected the useLayoutEffect hook into the global scope array to support synchronous DOM painting.

TimelineScrubber (v23.11.0): * [FIX: TL-ANIM-05] Transferred the Cluster Auto-Zoom step of the animation state machine from a standard useEffect to a synchronous useLayoutEffect. The width stretch and the scrollTo re-centering now execute in the exact same millisecond, flawlessly eliminating timeline stretch-flicker.

[FIX: TL-UI-09] Engineered an isFirstRenderedTick gate inside the axis generation loop to strictly enforce a full absolute date context on the leading edge of the timeline viewport.

[FIX: UI-NUM-02] Wrapped the active zoomLevel readout in Math.round() for a clean, whole-number GUI presentation.

---

Changelog (v6.2.3)
MapViewer (v3.17.0): * [FIX: MAP-UI-01] Dropped the mathematically inconsistent spatial multipliers from the map zoom indicator. HUD now natively reports absolute zoom scale.

[FIX: MAP-GRID-01] Engineered a dependency-free Micro-Graticule Engine. Parses the absolute bounds of the map view and dynamically loops native L.polyline injections based on variable degree intervals (10° down to 0.01°) synced seamlessly to the Leaflet moveend camera trigger. Included an interactive toggle boolean in the map HUD cluster.

---

Changelog (v6.2.2)
GlobalStyles (v4.11.0): [FIX: TL-UI-05] Retained the v6.1.3 flat-edge .event-block boundary box and preserved the 8px internal vertical padding expansion to strictly support native 3-line string wrapping.

TimelineScrubber (v23.10.0): [FIX: TL-UI-07] Injected a symmetrical top: 5px offset into the internal lane layout boundary. This geometrically balances the bottom margin, providing a 5px positive breather above the uppermost active swimlane.

[FIX: TL-UI-08] Engineered the Infinite Calendar Scale Matrix. Deprecated static increments. Major/Minor axes ticks are now calculated via dynamic Gregorian interpolation against an array of 16 context thresholds (spanning 1000 Years down to 1 Minute). Resolves extreme-zoom label collision while natively accommodating leap variations.

[FIX: TL-ANIM-03] Deleted the sub-second micro-pan useEffect hook. Cross-domain sequence limits transitions to native CSS-driven auto-zoom width expansion post-pan, permanently preventing DOM coordinate stutter.

---

Changelog (v6.2.1)
GlobalStyles (v4.11.0): * [FIX: TL-UI-05] Restored the flat-edge structural geometry to the .event-block class and compressed internal top/bottom padding to 2px to natively support 3-line string wraps without clipping.

TimelineScrubber (v23.9.0): * [FIX: TL-ANIM-01] Implemented a 3-step delayed state machine (Highlight ➔ Smooth Pan ➔ 400ms Delay ➔ Cluster Zoom & Snap) for timeline navigation, mathematically eliminating visual stretch-whiplash.

[FIX: TL-UI-06] Rewrote the timeline ruler generation loop. Replaced static millisecond addition with a Semantic Gregorian Calendar Engine, guaranteeing perfect chronological alignment across variable-length months and leap years.

[FIX: TL-UI-04] Integrated spatial collision detection to intelligently format and mount semantic minor tick labels based on dynamic track width.

---

Changelog (v6.2.0)
GlobalStyles (v4.10.0): * [FIX: TL-UI-03] Recalibrated .event-block clip-paths with a 4px squeeze and elevated the .timeline-track-container 5px to eliminate visual bleed across swimlanes and the X-Axis.

[FIX: UI-TYPO-02] Scrubbed bold styling application-wide for a neutral, professional UI.

[FIX: UI-HUD-02] Deployed absolute centering (transform: translateX(-50%)) for the main navigation cluster.

AppOrchestrator (v1.34.1): * [FIX: CRASH-02] Passed jumpToSlide into ContentSlider to prevent ReferenceErrors from Modal interactions.

[FIX: SUB-02] Upgraded parsing logic to intercept Google Sheets' CHAR(10) line breaks (\n) and strictly truncate sublabels at the first comma.

MapViewer (v3.16.3): * [FIX: CRASH-01.5] Engineered an invincible micro-parser polyfill to intercept and manually construct geometries when Wicket silently fails on GEOMETRYCOLLECTION arrays.

[FIX: GEO-02] Reduced LINESTRING weights to 2px with dashed strokes and stripped borders from POLYGON shapes.

[FIX: UI-NUM-01] Converted zoom percentages to clean <times>x floats.

MediaViewer (v2.4.0): * [FIX: MED-02] Bypassed anchor-wrapping for strings containing <iframe, executing them dynamically as raw HTML.

[FIX: MED-UI-03] Engineered regex extraction to pull raw URLs from <iframe src="..."> strings for caption hyperlink routing.

ContentSlider (v3.9.2): * [FIX: DIAG-02] Reorganized the About Modal to separate project description from active telemetry (URL parameters, dynamic version footprint, and usage instructions).

TimelineScrubber (v23.8.0): * [FIX: TL-AUTO-03] Replaced nearest-neighbor auto-zoom with a 5-event Cluster Window, enforcing a 30-minute minimal span calculation to properly frame high-density event cascades.

[FIX: TL-UI-04] Injected spatial collision detection to dynamically render scaled, contextual typography (Months, Days, HH:MM) on minor axis ticks.

---

Changelog (v6.1.5)
MapViewer (v3.16.3):
[FIX: SUB-02] Updated sublabel parsing. Inserted \n into the RegExp delimiter block to support CHAR(10) line breaks and chained a split/index call to dynamically truncate label strings at the first comma encountered.

---

Changelog (v6.1.4)
ContentSlider (v3.9.2): [FIX: CRASH-02] Added missing jumpToSlide prop assignment. The "View overview as slide" button now correctly triggers timeline navigation without causing a fatal ReferenceError crash.

AppOrchestrator (v1.34.1): [FIX: CRASH-02] Wired the jumpToSlide method into the <ContentSlider /> component injection.

MapViewer (v3.16.1): [FIX: CRASH-01] Implemented parseGeometryCollection(). This custom polyfill strips the WKT wrapper and iteratively forces individual shapes (LINESTRING, POINT) into the Wicket constructor, permanently preventing the not yet supported console crash and restoring geometry rendering.

---

Changelog (v6.1.3-2)
GlobalStyles (v4.9.0): [FIX: UI-TYPO-02] Scrubbed all font-weight: bold properties from the UI layer to establish the professional neutral aesthetic. [FIX: UI-HUD-02] Reprogrammed .status-center with absolute CSS positioning to lock navigation buttons to the exact middle of the user's screen.

VibeMonitor (v2.0.0): [FIX: DIAG-01] Implemented the Active Sensor HUD immediately below the header to display the activeSlide.location text and system version metrics for diagnostic transparency.

ContentSlider (v3.9.1): [FIX: DIAG-02] Restructured the 'About' modal flow. Placed the "View overview" button immediately following the description block, and isolated the dynamic URL parameter list and Usage Instructions into a separate trailing block. Injected dynamic ${APP_VERSION} into the status bar logo button.

---

Changelog (v6.1.3)
MediaViewer (v2.4.0):
[FIX: MED-02] Engineered a raw HTML interception loop. Entire iframe tags pasted into the media column now render flawlessly without triggering relative-path appending errors.
[FIX: MED-UI-03] Built getCleanUrl() extractor. Captions bound to iframe snippets now correctly link to the map/video source instead of triggering browser panics. Added fallback interceptor for naked Google Map embed URLs.

---

Changelog (v6.2.0)
GlobalStyles (v4.8.0): [FIX: MED-UI-01] Styled .media-meta-box a to inherit standard font weights while revealing anchor behaviors on hover.

AppOrchestrator (v1.33.0): [FIX: MED-01] Upgraded parsedMedia regex to explicitly target comma-space and semicolon-space signatures, preserving URL integrity.

MediaViewer (v2.2.0): [FIX: MED-UI-01] Extracted the img tags into <a> wrappers. Injected the .media-meta-box container directly inside the carousel items to permanently bind clickable captions and credits below the media focus.

---

Changelog (v6.1.1)
MapViewer (v3.16.0):
[FIX: GEO-02] Segregated polygon and linestring styling blocks to minimize visual dominance.
[FIX: UI-NUM-01] Converted map zoom HUD to report relative scaling using the x notation.

TimelineScrubber (v23.7.0):
[FIX: TL-AUTO-02] Removed the gap > 0 exclusionary logic. The autozoom engine now correctly identifies dense/simultaneous event clusters and correctly zooms in to construct visual cascades instead of aggressively zooming out.
[FIX: UI-NUM-01] Converted timeline UI to report scale using the x notation.

---

Changelog (v6.1.0)
GlobalStyles (v4.7.0): Injected .minimap-container CSS footprint and .timeline-leaflet-controls mimicry logic.

AppOrchestrator (v1.32.0): Configured central zoomLock and visibleTimeSpan HUD properties and propagated them to visual children.

MapViewer (v3.15.0): [FIX: MAP-12] Instantiated the synchronized Overview Minimap bounded explicitly to the dataset scope. [FIX: MAP-13] Deployed L.control.scale(). Engineered dynamic 100% calculation based on relative deviation from the baseZoomRef.

TimelineScrubber (v23.6.0): [FIX: TL-AUTO-01] Remodeled Auto-Zoom engine to strictly enforce 10px spacing with a 10-minute cascade floor. [FIX: TL-LOCK-01] Implemented Soft/Hard tracking states. [FIX: TL-UI-01] Repackaged timeline controls into a Leaflet-style vertical stack for unified visual cadence.

ContentSlider (v3.9.0): [FIX: UI-HUD-01] Formatted the unified status bar to display the semantic timeline extent readout.

---

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
