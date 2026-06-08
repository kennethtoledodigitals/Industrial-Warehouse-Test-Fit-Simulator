[README (3).md](https://github.com/user-attachments/files/28699753/README.3.md)
# 🏭 Industrial Warehouse Test-Fit Simulator (IWTFS)
### Design Simulator v4.2 — Browser-Based, Zero Installation

> A fully browser-based industrial warehouse test-fit tool for architects, planners, and developers. Trace any lot shape using Google Maps or import a GeoJSON file, configure buildings and ramps, and instantly generate a compliant site layout with GFA breakdown, parking schedule, GeoDataFrame export, and CAD/DXF output — no software required.

🔗 **Live Demo:** [https://kennethtoledodigitals.github.io/Industrial-Warehouse-Test-Fit-Simulator/](https://kennethtoledodigitals.github.io/Industrial-Warehouse-Test-Fit-Simulator/)

---

## ✨ Features

### 🗺️ Google Maps Site Picker
- Click the **📍 Google Maps** tab to open an interactive satellite map
- Click on map corners to trace your lot boundary — numbered vertices placed automatically
- Undo last point, clear all, or search by address
- Hit **✓ Use Coords** to push coordinates directly into the simulator
- Polygon overlay uses a transparent fill so the satellite imagery stays fully visible underneath
- Alternative: paste `Lat,Lon` pairs copied from Google Maps right-click → "What's here?"

### 📂 GeoJSON Import *(new in v4.2)*
- Click **📂 Import GeoJSON** in the Lot Polygon section to load any `.geojson` or `.json` file
- Supports `FeatureCollection`, `Feature`, `Polygon`, and `MultiPolygon` geometry types
- Auto-detects coordinate system — geographic `[lon, lat]` (GeoJSON spec) or projected meters
- Automatically sets coordinate mode (`Lat,Lon` or `X,Y`) and runs the simulation immediately
- Status message confirms filename, vertex count, and detected mode
- Compatible with exports from QGIS, ArcGIS, geojson.io, Google Earth, and any GIS tool

### 📐 Coordinate Input Modes
- **Local X,Y (meters)** — manual entry, ideal for preset shapes
- **Lat,Lon (Google Maps)** — auto-converted to local metric using Haversine projection
- **GeoJSON file import** — drag-and-drop via the Import button
- Preset lot shapes: Rectangle, L-Shape, Trapezoid, Irregular, Sample LatLon

### 🏗️ Building Layout Engine
- **1–4 buildings** stacked with shared truck yards sandwiched between pairs
- **1–4 storeys**, configurable floor height (4–15m)
- Buildings always constrained inside the buildable zone — never exceeds the lot boundary
- **BUAR** (Built-Up Area Ratio) configurable multiplier (e.g. 1.5×, 2.5×)
- **Office Mezzanine** — 3% of warehouse GFA per level per building (configurable)

### 🚗 Ramp Types (3 Independent, Each Individually Toggleable)

| Badge | Type | Slope | Description |
|---|---|---|---|
| `STR` | Straight Ramp | 1:12 (8.33%) | Hatched slab; configurable width ≥9m and run length |
| `U-RAMP` | Semi-Circular U-Ramp | Curve 1:20 · Straight arms 1:12 | Two straight arms + semi-circular head (matches reference design); inner R ≥ 6m |
| `CYL` | Cylindrical Helix | ≤ 1:12 along path | Compact spiral core; configurable radius, lane width, and number of turns |

### 🌿 Site Layers (Outside → In)
```
Lot Boundary
  └─ Perimeter Green Strip  (≥ 2m, configurable)
       └─ Fire Access Road  (≥ 6m wide — parking on both sides)
            └─ Building Green Strip  (2m around each building)
                 └─ Setbacks  (Front / Rear / Left / Right)
                      └─ Buildable Zone → Buildings · Truck Yards · Ramps
```

### 🅿️ Parking Calculator
Automatically computed per Philippine standards (BP 957 / DPWH / RROW):

| Category | Basis |
|---|---|
| Warehouse Parking | 1 slot per first 1,400 m² GFA; then 1 per 232.2 m² remaining |
| Office Parking | 1 per 46.4 m² office GFA |
| Handicap / Accessible | max(2, 2% of total regular slots) |
| Motorcycle | 1 per 185.8 m² total GFA |
| Truck Loading / Unloading Bays | 1 per 900 m² warehouse GFA (≈ 1 per 10,000 sqft) |

Stall size options: 2.5 × 5.0 m (Standard) or 2.7 × 5.0 m (Wide)

### 🔧 Ancillary Areas
- **MEP / Utility Room** — minimum 600 m², configurable up to 2,000 m²
- **Guard House** — 40 m² at main entrance/exit gate (configurable)

### 📊 Output Tabs

| Tab | Contents |
|---|---|
| **2D Site Plan** | Canvas plan view: lot boundary, green strips, fire road, perimeter parking, buildings, truck yards, all ramp types, MEP room, guard house, scale bar, vertex labels. Includes **⬇ Export CAD (.dxf)** button. |
| **3D Massing** | Interactive isometric — drag to rotate, scroll to zoom, floor division lines rendered |
| **GFA Breakdown** | Per-building per-floor table: warehouse GFA, office mezzanine GFA, ramp footprints with formulas (W×L, π(R²out − R²in)) |
| **Parking** | Full parking schedule with stall dimensions, area subtotals, 40% aisle allowance, site notes |
| **Compliance** | Pass/fail summary cards + detailed rule-by-rule checklist |
| **📄 GeoData** | Python/GeoPandas code, GeoJSON download, coordinate table, install guide, and DXF/CAD export shortcut |
| **📍 Google Maps** | Interactive satellite site picker with click-to-place vertices and transparent polygon overlay |

### 🗄️ GeoDataFrame Export
- Complete **Python script** (Shapely + GeoPandas): lot polygon, buildable zone, buildings, yards, ramps, MEP area, and guard house — each as a separate named GeoDataFrame layer
- **GeoJSON** file — paste directly into QGIS, Mapbox, or [geojson.io](https://geojson.io)
- **GeoPackage** (`.gpkg`) — all layers in one file
- CRS: `EPSG:32651` (WGS84 / UTM Zone 51N — Philippines); easy to change for other regions
- In-browser **Copy** and **Download** buttons for both `.py` and `.geojson`

### 📐 CAD / DXF Export *(new in v4.2)*
Click **⬇ Export CAD (.dxf)** in the 2D Site Plan toolbar (or via the GeoData tab) to download a fully layered AutoCAD DXF file.

**Named layers exported:**

| Layer | Content |
|---|---|
| `LOT_BOUNDARY` | Full lot polygon outline |
| `BUILDINGS` | All building footprints |
| `TRUCK_YARDS` | Truck yard rectangles |
| `STR_RAMPS` | Straight ramp footprints with hatch lines |
| `U_RAMP` | U-ramp arms and arc geometry |
| `HELIX_RAMP` | Cylindrical helix ramp circles |
| `MEP_AREA` | MEP / utility room rectangle |
| `GUARD_HOUSE` | Guard house rectangle |
| `PERIMETER_GREEN` | Inner boundary of perimeter green strip |
| `FIRE_ROAD` | Fire access road inner boundary |
| `PARKING` | Perimeter parking row rectangles |
| `BUILDABLE_ZONE` | Dashed buildable zone rectangle |
| `DIMENSIONS` | Dimension lines with lot width and height |
| `ANNOTATIONS` | All text labels, building IDs, GFA values, and title block |

Compatible with AutoCAD, QGIS, DraftSight, FreeCAD, LibreCAD, BricsCAD, and any DXF-capable application. Coordinates are in meters (DXF `INSUNITS = 4`).

### ✅ Compliance Engine
Auto-checks every simulation against:

- Max Plinth Coverage %
- Min Landscape Area %
- BUAR (Built-Up Area Ratio) limit
- Total GFA vs. BUAR max
- Storeys within 1–4
- Setback clearance (both dimensions)
- Fire road ≥ 6 m
- Perimeter green strip ≥ 2 m
- Building green strip ≥ 2 m
- MEP area ≥ 600 m²
- Guard house ≥ 40 m²
- Handicap slots ≥ max(2, 2% of regular)
- Truck bay provision
- Straight ramp width ≥ 9 m
- U-ramp inner radius ≥ 6 m, clear width ≥ 9 m

---

## 🚀 Quick Start

### Option 1 — Use the Live Site (Recommended)
Visit the live demo link at the top. No account, no installation, no login required.

### Option 2 — Run Locally
```bash
git clone https://github.com/kennethtoledodigitals/Industrial-Warehouse-Test-Fit-Simulator.git
cd Industrial-Warehouse-Test-Fit-Simulator

# Open in your browser:
open index.html          # macOS
start index.html         # Windows
xdg-open index.html      # Linux
```

### Option 3 — Fork and Deploy Your Own
1. Fork this repo on GitHub
2. Go to **Settings → Pages**
3. Set source to `main` branch → `/ (root)` → Save
4. Your copy is live at `https://<your-username>.github.io/Industrial-Warehouse-Test-Fit-Simulator/`

---

## 🗺️ How to Get Lot Coordinates

### Method A — In-App Map Picker *(easiest)*
1. Open simulator → click **📍 Google Maps** tab
2. Click **Load Google Maps**
3. Navigate to your site → click each lot corner to place a vertex
4. Click **✓ Use Coords** — the simulation runs automatically

### Method B — GeoJSON Import *(fastest for GIS users)*
1. Export your lot polygon from QGIS, ArcGIS, or geojson.io as a `.geojson` file
2. In the simulator, click **📂 Import GeoJSON** in the Lot Polygon section
3. Select your file — coordinates are imported and the simulation runs automatically

### Method C — Google Maps Right-Click
1. Go to [maps.google.com](https://maps.google.com)
2. Navigate to your site
3. Right-click any corner of the lot → a pop-up shows coordinates (e.g. `14.580123, 121.045678`)
4. In the simulator, switch to **Lat,Lon (Google)** mode and paste one pair per line:

```
14.580100, 121.045600
14.580100, 121.046200
14.579500, 121.046200
14.579500, 121.045600
```

### Method D — Google Earth Pro *(most precise)*
1. Open Google Earth Pro (free download from Google)
2. Use the polygon tool to trace the lot boundary
3. Right-click each vertex → copy coordinates
4. Paste into the simulator in Lat,Lon mode, or export as KML → convert to GeoJSON → import directly

---

## 📐 Sample Inputs

**Local X,Y (meters) — simple rectangle:**
```
0,0
200,0
200,120
0,120
```

**Lat,Lon — Google Maps format:**
```
14.580100,121.045600
14.580100,121.046200
14.579500,121.046200
14.579500,121.045600
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

**GeoJSON import — example file structure:**
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

## 📐 CAD Export Workflow

1. Run the simulation with your lot polygon and building config
2. Click **⬇ Export CAD (.dxf)** in the **2D Site Plan** toolbar
3. Open `warehouse_testfit.dxf` in your CAD application
4. Each element is on its own named layer — toggle visibility, adjust line weights, or add your own annotations on top

**Tip:** In AutoCAD or BricsCAD, use `ZOOM → Extents` after opening to fit the drawing to screen. Units are in meters.

---

## 🐍 GeoDataFrame Export — Python Setup

After running the simulation, click the **📄 GeoData** tab to get the generated Python script. To run it:

```bash
# Install dependencies
pip install geopandas shapely pandas pyproj fiona

# Or with conda
conda install -c conda-forge geopandas shapely

# Run the exported script
python warehouse_geodata.py
```

**Output files:**
```
warehouse_site.geojson   → drag into QGIS or paste into geojson.io
warehouse_site.gpkg      → GeoPackage with all layers (lot, buildings, yards, ramps, MEP, GH)
```

**GeoDataFrame layers exported:**

| Layer | Content |
|---|---|
| `lot` | Full lot polygon with area, BUAR limit, max GFA |
| `buildable_zone` | Inset rectangle after setbacks |
| `building_1..4` | Each building with storeys, footprint, WH GFA, office GFA, total GFA |
| `yard_1..n` | Truck yards between building pairs |
| `ramp_1..n` | Straight, U-ramp, or helix footprints |
| `mep_area` | MEP / utility room rectangle |
| `guard_house` | Guard house rectangle |

---

## 🔧 Technical Details

| Item | Detail |
|---|---|
| Language | HTML + CSS + Vanilla JavaScript (zero frameworks) |
| Fonts | Google Fonts — Space Mono, Barlow Condensed (CDN) |
| Maps | Google Maps JavaScript API (loads on demand) |
| 2D Rendering | HTML5 Canvas API |
| 3D Rendering | Custom isometric projection on Canvas (no WebGL) |
| Coordinate Conversion | Haversine formula (lat/lon → local meters) |
| GeoJSON Import | Native FileReader API — no server, no upload |
| CAD Export | DXF R2000 (AC1015) generated client-side — no server |
| GeoData | Generated Python code using Shapely + GeoPandas |
| File Structure | Single self-contained `index.html` (~1,870 lines) |
| Hosting | GitHub Pages (static, no backend) |
| Browser Support | Chrome, Firefox, Edge, Safari (any modern browser) |

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
| Straight | 1:12 (8.33%) | ≥ 9m one-way · ≥ 12m two-way | 1m rise per 12m run; for 7m floor: 84m run |
| U-Ramp (semi-circular) | Arms 1:12 · Curve 1:20 | Inner R ≥ 6m · Clear W ≥ 9m | 2-way 18m clear width; matches reference site plan |
| Cylindrical Helix | ≤ 1:12 along path | Core R ≥ 4m · Lane ≥ 3m | Compact vertical core; 1-way spiral |

---

## 🔄 Changelog

| Version | Changes |
|---|---|
| **v4.2** | GeoJSON file import (auto-detects coordinate type, supports FeatureCollection/Feature/Polygon/MultiPolygon); CAD/DXF export with 14 named layers, dimension lines, and title block; fixed Google Maps polygon overlay blocking satellite imagery; map default changed to hybrid mode |
| **v4.1** | Replaced Mapbox with Google Maps; in-app site picker with click-to-place vertices, right-click undo, address search, polygon preview |
| **v4.0** | Fire access road (6m) with parking on both sides; perimeter green strip; 2m building green strip; MEP area (≥600m²); guard house (40m²); U-ramp semi-circular shape matching reference plan |
| **v3.1** | Lat/Lon coordinate input with Haversine auto-conversion; building boundary clamping to lot polygon |
| **v3.0** | 3 independent ramp types (Straight, Circular, Cylindrical); GeoDataFrame export (Python + GeoJSON); BUAR limit; 4-building support; GeoData tab |
| **v2.0** | Full parking calculator; BUAR; office mezzanine GFA; compliance engine with pass/fail cards |
| **v1.0** | Initial release — 2D/3D massing, GFA breakdown, setbacks, preset lot shapes |

---

## 👤 Author

**Kenneth Toledo**
- GitHub: [@kennethtoledodigitals](https://github.com/kennethtoledodigitals)
- Live Project: [Industrial Warehouse Test-Fit Simulator](https://kennethtoledodigitals.github.io/Industrial-Warehouse-Test-Fit-Simulator/)

---

## 📄 License

This project is open for personal and educational use. For commercial use or redistribution, please contact the author.
