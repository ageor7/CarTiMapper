# CarTiMapper (v6.3)

A high-performance, single-file application that seamlessly integrates interactive cartography (Leaflet) with a high-fidelity, data-dense swimlane timeline and a synchronized media carousel. Built entirely without a build step (Webpack/Node), this engine runs natively in the browser using Preact, HTM, and Vanilla JavaScript.

It is designed to consume published Google Sheets (CSVs) and dynamically orchestrate a cinematic, data-driven narrative across three responsive panes: Map, Media, and Timeline.

---

## ✨ Architectural Highlights & Features

### ⏱️ High-Fidelity Swimlane Timeline
* **Dynamic Time Resolution:** The timeline X-axis automatically scales based on your zoom level, seamlessly shifting from Centuries down to specific Minutes using a physical-pixel loop algorithm.
* **Smart Typographic Collision:** Employs a 60px viewport safe-margin and ±65px mathematical "dead zones" around the active absolute date (e.g., `15 Mar 2026 14:30`) to guarantee minor ticks never overlap the main chronological anchor, even when manually panning.
* **Auto-Zoom & Viewport Tracking:** Automatically calculates the exact mathematical cluster of surrounding events and zooms the camera to fit them perfectly. When clicking events, the timeline calculates the spatial target and synchronously anchors it to the CSS scale matrix, eliminating "fly-aways."
* **State Controls (🔒 / 🔓):** Features a built-in auto-zoom lock. Users can toggle a hard-lock to explore dense datasets at a specific manual magnification without the timeline auto-resetting on every click.
* **Gantt-Style Duration Trapezes:** Events with both a `startDate` and `endDate` project a sleek, semi-transparent span bar utilizing precise `clip-path` slopes that snap mathematically flush to the chronological axis.

### 🖼️ Synchronized Media Carousel
* **Multi-Asset Array Mapping:** Supports multiple images, videos, YouTube embeds, and generic iframes *per event*. The engine uses a robust flattening matrix to split arrays 1:1.
* **Spatial Navigation:** No more tiny buttons. The entire wrapper serves as an invisible interaction zone. Click the left 50% of the screen to swipe back, or the right 50% to swipe forward. Mouse cursors dynamically switch to horizontal arrows (`ew-resize`) on multi-image events.
* **Flex-Gravity Typography:** Employs strict flexbox logic ensuring UI perfection regardless of image aspect ratio. Captions and credits dynamically claim only the vertical height they need at the bottom of the screen. The image perfectly centers itself (`object-fit: contain`) in the remaining space above, guaranteeing text never overlays or obscures photographic details.
* **Interactive Decoupling:** Image visual boundaries are strictly decoupled from hyperlinks to preserve the swiping UI. Typography remains hyperlinked natively, and an isolated `[↗]` external link icon routes users to the raw source assets.

### 🗺️ Cinematic Geo-Engine
* **WKT & GeoJSON Architecture:** Natively intercepts and parses both standard WKT (`POINT(lon lat)`) and valid GeoJSON syntax directly from the spreadsheet.
* **Density-Aware Auto-Zoom:** The camera mathematically calculates the Haversine distance between coordinates. It glides to a tight `Zoom 11` for crowded urban clusters and gracefully pulls back for isolated regional points.
* **Cluster Deduplication:** Natively handles arrays of geographic points per event, mapping them collectively and cleanly overriding redundant coordinates.

### 📐 State-Persistent Fluid Grid
* **Custom JS Splitters:** A highly performant, 60fps JavaScript splitter engine. Shrinking or expanding any pane flawlessly pushes or pulls its neighbors.
* **Dynamic Collapse:** If a specific record contains no media, the Media Pane instantly collapses to `0%` and the Map Pane seamlessly consumes the viewport.

---

## 🚀 Usage & Configuration

Because this project uses a **Zero-Build Architecture**, there is nothing to install or compile.

1. Format a Google Sheet with standard columns (`Title`, `Start`, `End`, `Media`, `Caption`, `Credit`, `Location/WKT`, `Tags`, `Description`).
2. Publish your Google Sheet to the web as a CSV.
3. Pass the Google Sheet ID to the URL syntax:
   `https://your-domain.com/index.html?source=YOUR_GOOGLE_SHEET_ID`

### 📊 Configuring Multiple Media (The CHAR(10) Array)
To assign multiple images to a single event, you must separate them using a newline (`Alt+Enter` or `Cmd+Return` in Google Sheets). The parser will automatically sync the `Media`, `Caption`, and `Credit` arrays by their index. 
* *Example:* If you provide 3 URLs in the Media column, but only 1 Caption, the engine gracefully maps the caption to the first image and leaves the next two blank.

### 🔗 The ETL Pipeline (Deduplication)
If your raw data has multiple rows for the same event, CarTiMapper works best when pre-processed via a Google Sheets `MAKEARRAY` formula. 
By utilizing the Omni-Splitter matrix, you can roll up duplicate rows and `TEXTJOIN` their images and metadata using `CHAR(10)` as the delimiter. *Note: Ensure your `FILTER` arrays are wrapped in `IFERROR(..., "")` to prevent `#N/A` cascades on blank cells.*

---

## 🛠️ Development & Modular Patching Protocol

Despite being a monolithic `index.html` file for ultimate portability, the codebase is strictly organized into semantic, version-controlled component blocks (e.g., `AppOrchestrator`, `MediaViewer`, `TimelineScrubber`).

**If you are modifying the code:**
1. Locate the specific `// === [ COMPONENT NAME vX.X.X ] ===` anchor.
2. Apply changes strictly within those boundaries to prevent Virtual DOM regressions.
3. Update the global `MODULE_VERSIONS` manifest object at the top of the file.

### The "Vibe-Monitor" Developer HUD
Features a built-in, draggable console overlay. Instead of checking the browser console, developers can pipe `addLog` to the monitor to intercept data-parsing matrices, array flatteners, coordinate validation failures, and real-time initialization manifests across all active modules.
