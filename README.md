# CarTiMapper (v5.0.0)

A high-performance, single-file application that seamlessly integrates interactive cartography (Leaflet) with a high-fidelity, KnightLab-style swimlane timeline. Built entirely without a build step (Webpack/Node), this engine runs natively in the browser using Preact, HTM, and pure Vanilla JavaScript.

It is designed to consume published Google Sheets (CSVs) and dynamically orchestrate a cinematic, data-driven narrative across three responsive panes: Map, Media, and Timeline.

## ✨ v5.0.0 Architectural Highlights

### 🗺️ Cinematic Geo-Engine
* **Density-Aware Auto-Zoom:** The camera mathematically calculates the Haversine distance between coordinates. It glides to a tight `Zoom 11` for crowded urban clusters and gracefully pulls back to `Zoom 6` for isolated regional points.
* **Focus-Locked Physics:** Pan and zoom movements are mathematically locked to a 2.5-second cinematic transition curve.
* **Custom SVG Cartography:** Ditched standard image markers for highly customizable, CSS-driven HTML pins.
* **GeoJSON Trajectory Mapping:** Natively parses LineStrings and mathematically computes standard Euclidean angles (`Math.atan2`) to draw directional SVG arrows on the terminal vertices.

### ⏱️ High-Fidelity Swimlane Timeline
* **Dynamic Time Resolution:** The timeline X-axis automatically scales based on your zoom level, seamlessly shifting from Centuries and Decades down to specific Days and Minutes (e.g., `15 Oct, 14:30`) utilizing a physical-pixel loop algorithm.
* **KnightLab-Style Swimlanes:** Extracts tags from your dataset and automatically sorts them into distinct horizontal tracks. Events spanning the same tag automatically arrange themselves to avoid collisions.
* **Hexagon CSS-Clip Blocks:** Event labels utilize a custom CSS `clip-path` polygon that anchors perfectly to the swimlane height while allowing the top and bottom diagonals to expand infinitely for elegant text word-wrapping.
* **Gantt-Style Duration Bars:** Events with both a `startDate` and `endDate` project a semi-transparent span bar across their swimlane to physically represent time elapsed.
* **Focus-Lock Scrolling:** When zooming in or out, the timeline dynamically recalculates the exact pixel midpoint of the active event, forcing the scrollbar to keep it dead-center.

### 📐 State-Persistent Fluid Grid
* **Custom JS Splitters:** Abandoned native CSS resizing for a highly performant, 60fps JavaScript splitter engine. Shrinking or expanding any pane (Map, Media, or Content) flawlessly pushes or pulls its neighbors.
* **CSS Variable Persistence:** Layout state is stored in root CSS variables, preventing expensive React re-renders during drag events.
* **Dynamic Media Collapse:** If a specific record contains no media, the Media Pane instantly collapses to `0%` and the Map Pane seamlessly consumes `100%` of the viewport.

### 🛠️ The "Vibe-Monitor" Developer Overlay
Features a built-in, draggable heads-up display (HUD) that intercepts and logs data-parsing errors, coordinate validation failures, and the real-time initialization manifest of all internal component modules.

---

## 🚀 Quick Start / Usage

Because this project uses a **Zero-Build Architecture**, there is nothing to install, compile, or bundle. 

1. Host the `index.html` file on any basic web server (GitHub Pages, Vercel, Netlify).
2. Format a Google Sheet with standard TimeMapper columns (`Title`, `Start`, `End`, `Location`, `Media`, `Tags`, `Description`).
3. Publish your Google Sheet to the web as a CSV.
4. Pass the Google Sheet ID to the URL:
   `https://your-domain.com/index.html?source=YOUR_GOOGLE_SHEET_ID`

## 🏗️ Development & Modular Patching Protocol

Despite being a monolithic `index.html` file for ultimate portability, the codebase is strictly organized into semantic, version-controlled component blocks (e.g., `AppOrchestrator`, `MapViewer`, `TimelineScrubber`). 

If you are modifying the code:
1. Locate the specific `// === [ COMPONENT NAME vX.X.X ] ===` anchor.
2. Apply changes strictly within those boundaries to prevent Virtual DOM regressions.
3. Update the global `MODULE_VERSIONS` manifest object at the top of the file so the Vibe-Monitor correctly reports the active component architecture on boot.
