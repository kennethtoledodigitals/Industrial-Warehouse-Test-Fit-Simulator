[Uploading README (2).md…]()
# 🏭 Industrial Warehouse Test-Fit Simulator (IWTFS)
### Design Simulator v4.5 — Browser-Based, Zero Installation

> A browser-based industrial warehouse test-fit tool for architects, planners, and developers. Trace any lot shape using Google Maps or import a GeoJSON file, configure buildings and ramps, and instantly generate a site layout with GFA breakdown, parking counts, a compliance summary, and DXF output — no software required.

🔗 **Live Demo:** [https://kennethtoledodigitals.github.io/Industrial-Warehouse-Test-Fit-Simulator/](https://kennethtoledodigitals.github.io/Industrial-Warehouse-Test-Fit-Simulator/)

---

## ✨ Features

### 🗺️ Google Maps Site Picker
- Click the **📍 Google Maps** tab, then **Load Google Maps**, to open an interactive satellite map
- Click map corners to trace the lot boundary; the polygon is drawn with a light transparent fill so the imagery stays visible
- **Right-click** or **↩ Undo** removes the last point; **✕ Clear** removes all points
- Search by address or place name (Google Extended Component Library place picker)
- Hit **✓ Use Coords** to push the vertices into the simulator and run it
- Requires a Google Maps API key (see [Google Maps API Key](#-google-maps-api-key))
- Alternative: paste `Lat,Lon` pairs copied from Google Maps (right-click → coordinates)

### 📂 GeoJSON Import
- Click **📂 Import GeoJSON** in the Lot Polygon section to load a `.geojson` or `.json` file
- Supports `FeatureCollection`, `Feature`, `Polygon`, and `MultiPolygon`; the **first polygon** found is used (outer ring only)
- Coordinates are read in GeoJSON order `[lon, lat]`, the closing duplicate vertex is dropped, and the simulator switches to **Lat,Lon** mode and runs immediately
- A status line confirms the filename and vertex count
- Works with exports from QGIS, ArcGIS, geojson.io, and Google Earth (via KML → GeoJSON)

### 📐 Coordinate Input Modes
- **Local X,Y (meters)**: manual entry, ideal for preset shapes
- **Lat,Lon (Google)**: auto-converted to local meters using an equirectangular approximation anchored on the first vertex (accurate for lot-sized areas)
- Preset lot shapes: Rectangle, L-Shape, Trapezoid, Irregular, Sample LatLon
- Vertices may be entered clockwise or counter-clockwise

### 🧭 Front Road & Setbacks
- Choose the **front road / main entrance side** (South, North, West, East); the polygon is displayed **north-up**
- Separate Front / Rear / Left / Right setbacks, applied **per lot edge**, so they follow irregular polygons
- Guard house can be placed on the front (main gate) or rear

### 🏗️ Building Layout Engine
- Finds the **largest inscribed rectangle** inside the setback polygon, testing orientations aligned to the lot's edges, so buildings follow angled lots
- **1–4 buildings** stacked in rows with truck yards between them
- **1–4 storeys**, configurable floor height (4–15 m)
- Building depth is scaled by bisection to the highest value that stays within **Max Plinth %**
- **BUAR** (Built-Up Area Ratio) limit, e.g. 1.5×, 2.5×
- **Office mezzanine**: configurable % of warehouse GFA per level (default 3%)

### 🚗 Ramp Types (3 Independent, Each Toggleable)
Ramps are only generated for buildings with **2 or more storeys**.

| Badge | Type | Slope | Description |
|---|---|---|---|
| `STR` | Straight Ramp | 1:12 (8.33%) or 1:10 | Slab at the building corner; configurable width ≥ 9 m and run length |
| `U-RAMP` | Semi-Circular Ramp | Curve 1:20 · Straight run 1:12 | Straight run with a semicircle at **both ends** (stadium shape); inner R ≥ 6 m |
| `CYL` | Cylindrical Helix | ≤ 1:12 along path | Compact spiral core; configurable core radius, lane width, and turns |

### 🌿 Site Layers (Outside → In)
```
Lot Boundary
  └─ Perimeter Green Strip  (default 2 m, configurable)
       └─ Fire Access Road  (default 6 m, configurable 4–9 m)
            └─ Building Green Strip  (2 m)
                 └─ Setbacks  (Front / Rear / Left / Right, per edge)
                      └─ Buildable Zone → Buildings · Truck Yards · Ramps
```
Perimeter parking stalls are laid out along every lot edge, following the polygon.

### 🅿️ Parking Calculator
Computed per the formulas below (Philippine standards, BP 957 / DPWH / RROW):

| Category | Basis |
|---|---|
| Warehouse Parking | 1 slot for the first 1,400 m² GFA; then 1 per 232.2 m² remaining |
| Office Parking | 1 per 46.4 m² office GFA |
| Handicap / Accessible | max(2, 2% of regular slots) |
| Motorcycle | 1 per 185.8 m² total GFA |
| Truck Loading / Unloading Bays | 1 per 900 m² warehouse GFA (minimum 1) |

Stall size options: 2.5 × 5.0 m (Standard) or 2.7 × 5.0 m (Wide).

### 🔧 Ancillary Areas
- **MEP / Utility Room**: default 600 m², configurable 200–2,000 m²
- **Guard House**: default 40 m², configurable 20–80 m²

### 📊 Output Tabs

| Tab | Contents |
|---|---|
| **2D Site Plan** | North-up canvas plan: lot boundary, green strip, fire-road band, perimeter parking, setback outline, buildings, truck yards, straight ramps, U-ramp, helix, MEP room, guard house, vertex labels, north arrow. Includes the **⬇ Export CAD (.dxf)** button and a live lot/GFA/FAR/plinth summary. |
| **3D Massing** | Interactive isometric view: drag to rotate, scroll to zoom. All three ramp types are modelled: straight ramp (orange wedge), semi-circular ramp (purple sloped slab with rounded ends), and cylindrical helix (blue spiral around a core), each rising one floor height. |
| **GFA** | Per-building, per-level table: footprint, warehouse GFA, office mezzanine GFA, total GFA, height |
| **Parking** | Cards for warehouse, office, handicap, and motorcycle slots, plus truck bays |
| **Compliance** | Pass/fail cards for Plinth %, FAR vs. BUAR limit, and Landscape % |
| **📄 GeoData** | Placeholder (see [Roadmap](#-roadmap)) |
| **📍 Google Maps** | Satellite site picker described above |

### 📐 CAD / DXF Export
Click **⬇ Export CAD (.dxf)** in the 2D Site Plan toolbar to download `testfit_site_plan.dxf` (DXF R12 ASCII, coordinates in meters, local X/Y).

| Layer | Content |
|---|---|
| `LOT_BOUNDARY` | Lot polygon |
| `SETBACK` | Setback polygon |
| `BUILDING_1..4` | Building footprints |
| `TRUCK_YARD` | Truck yards |
| `RAMP_STRAIGHT` | Straight ramp footprints |
| `RAMP_HELIX` | Helix ramp core and outer circles |
| `MEP` | MEP / utility room |
| `GUARD_HOUSE` | Guard house |
| `PARKING` | Perimeter parking stalls |

The U-ramp is shown in the 2D plan but is **not yet included** in the DXF export. Opens in AutoCAD, BricsCAD, DraftSight, FreeCAD, LibreCAD, and QGIS.

---

## 🚀 Quick Start

### Option 1: Use the Live Site (Recommended)
Visit the live demo link at the top. No account, installation, or login needed (the map picker needs an API key; everything else works without one).

### Option 2: Run Locally
```bash
git clone https://github.com/kennethtoledodigitals/Industrial-Warehouse-Test-Fit-Simulator.git
cd Industrial-Warehouse-Test-Fit-Simulator

# Open in your browser:
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux
```

### Option 3: Fork and Deploy Your Own
1. Fork this repo on GitHub
2. Go to **Settings → Pages**
3. Set source to `main` branch → `/ (root)` → Save
4. Your copy is live at `https://<your-username>.github.io/Industrial-Warehouse-Test-Fit-Simulator/`

---

## 🔑 Google Maps API Key

The map picker needs your own key. In `index.html`, find this line in the script and paste your key:

```js
const GMAPS_API_KEY='YOUR_API_KEY';
```

Create a key in [Google Cloud Console](https://console.cloud.google.com/apis/credentials) and enable the **Maps JavaScript API** and **Places API**. If you publish the site, restrict the key to your domain (HTTP referrer restriction). Without a key, the Load button only highlights the warning, and everything else still works.

---

## 🗺️ How to Get Lot Coordinates

### Method A: In-App Map Picker
1. Open the simulator → **📍 Google Maps** tab → **Load Google Maps**
2. Navigate to your site and click each lot corner
3. Click **✓ Use Coords**; the simulation runs automatically

### Method B: GeoJSON Import
1. Export your lot polygon from QGIS, ArcGIS, or geojson.io as `.geojson`
2. Click **📂 Import GeoJSON** in the Lot Polygon section and select the file

### Method C: Google Maps Right-Click
1. Go to [maps.google.com](https://maps.google.com) and navigate to your site
2. Right-click a lot corner → the pop-up shows coordinates (e.g. `14.580123, 121.045678`)
3. Switch the simulator to **Lat,Lon (Google)** mode and paste one pair per line

### Method D: Google Earth Pro
1. Trace the lot with the polygon tool in Google Earth Pro
2. Export as KML → convert to GeoJSON → import, or copy vertex coordinates into Lat,Lon mode

---

## 📐 Sample Inputs

**Local X,Y (meters), simple rectangle:**
```
0,0
200,0
200,120
0,120
```

**Lat,Lon, Google Maps format:**
```
14.5800,121.0450
14.5800,121.0480
14.5775,121.0480
14.5775,121.0450
```

**Irregular polygon:**
```
0,25
40,0
180,15
220,50
200,145
90,155
0,110
```

**GeoJSON import, example file:**
```json
{
  "type": "FeatureCollection",
  "features": [{
    "type": "Feature",
    "properties": { "name": "My Lot" },
    "geometry": {
      "type": "Polygon",
      "coordinates": [[
        [121.0456, 14.5801],
        [121.0462, 14.5801],
        [121.0462, 14.5795],
        [121.0456, 14.5795],
        [121.0456, 14.5801]
      ]]
    }
  }]
}
```

---

## 🔧 Technical Details

| Item | Detail |
|---|---|
| Language | HTML + CSS + Vanilla JavaScript (zero frameworks) |
| Fonts | Google Fonts: Space Mono, Barlow Condensed (CDN) |
| Maps | Google Maps JavaScript API (loaded on demand) + Extended Component Library place picker |
| 2D Rendering | HTML5 Canvas API |
| 3D Rendering | Custom isometric projection on Canvas (no WebGL) |
| Coordinate Conversion | Equirectangular approximation from the first vertex (lat/lon → local meters) |
| Layout | Oriented largest-inscribed-rectangle search + per-edge polygon setbacks |
| GeoJSON Import | Native FileReader API; no server, no upload |
| CAD Export | DXF R12 ASCII generated client-side; no server |
| File Structure | Single self-contained `index.html` (~1,550 lines) |
| Hosting | GitHub Pages (static, no backend) |
| Browser Support | Any modern browser (Chrome, Firefox, Edge, Safari) |

---

## 📁 Repository Structure

```
/
├── index.html    ← Complete simulator (single file, self-contained)
└── README.md     ← This file
```

---

## 📋 Standards Referenced

Malaysia Standards — Johor Bahru Malaysia — Industrial Buildings

---

## 🚧 Ramp Design Reference

| Type | Slope Standard | Min Width | Notes |
|---|---|---|---|
| Straight | 1:12 (8.33%) | ≥ 9 m one-way · ≥ 12 m two-way | 1 m rise per 12 m run; for a 7 m floor: 84 m run |
| U-Ramp (semi-circular) | Arms 1:12 · Curve 1:20 | Inner R ≥ 6 m · Clear W ≥ 9 m | 2-way 18 m clear width |
| Cylindrical Helix | ≤ 1:12 along path | Core R ≥ 4 m · Lane ≥ 3 m | Compact vertical core; 1-way spiral |

---

## 🛣️ Roadmap

Planned, not yet in the current version:

- **GeoData tab**: generated Python script (Shapely + GeoPandas), downloadable GeoJSON and GeoPackage, CRS `EPSG:32651` (UTM Zone 51N, Philippines)
- **Full compliance checklist**: rule-by-rule checks for setbacks, fire road, green strips, MEP/guard house minimums, handicap slots, truck bays, and ramp dimensions (currently only Plinth, FAR, and Landscape are checked)
- **Detailed parking schedule**: stall dimensions, area subtotals, and aisle allowance
- **DXF improvements**: U-ramp geometry, dimension lines, title block, and a buildable-zone layer
- **Auto-detect projected vs. geographic coordinates** on GeoJSON import

---

## 🔄 Changelog

| Version | Changes |
|---|---|
| **v4.5** | 3D massing now shows all three ramp types; semi-circular ramp redesigned with a semicircle at both ends (footprint = L×2R + πR²). Oriented largest-inscribed-rectangle layout that follows angled lots; front-road selection with north-up plan; per-edge setbacks; polygon-edge perimeter parking. **Fixes:** rotated-frame coordinate mapping that shrank the buildable zone (default lot went from ~10% to ~49% plinth); setbacks applied to the wrong edges on clockwise polygons; duplicate `minL` input id; 2D plan drawn upside-down; U-ramp and helix now drawn in the 2D plan. Rebuilt the GeoJSON import, DXF export, and Google Maps picker functions. |
| **v4.2** | GeoJSON file import; CAD/DXF export; fixed Google Maps polygon overlay blocking satellite imagery |
| **v4.1** | Replaced Mapbox with Google Maps; in-app site picker with click-to-place vertices, right-click undo, address search, polygon preview |
| **v4.0** | Fire access road (6 m) with parking; perimeter green strip; 2 m building green strip; MEP area; guard house; U-ramp semi-circular shape |
| **v3.1** | Lat/Lon coordinate input with auto-conversion; building boundary clamping to lot polygon |
| **v3.0** | 3 independent ramp types (Straight, Circular, Cylindrical); GeoDataFrame export; BUAR limit; 4-building support; GeoData tab |
| **v2.0** | Full parking calculator; BUAR; office mezzanine GFA; compliance engine with pass/fail cards |
| **v1.0** | Initial release: 2D/3D massing, GFA breakdown, setbacks, preset lot shapes |

---

## 👤 Author

**Kenneth Toledo**
- GitHub: [@kennethtoledodigitals](https://github.com/kennethtoledodigitals)
- Live Project: [Industrial Warehouse Test-Fit Simulator](https://kennethtoledodigitals.github.io/Industrial-Warehouse-Test-Fit-Simulator/)

---

## 📄 License

This project is open for personal and educational use. For commercial use or redistribution, please contact the author.
