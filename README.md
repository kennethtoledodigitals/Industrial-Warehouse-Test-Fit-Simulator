[README (1).md](https://github.com/user-attachments/files/28375136/README.1.md)
# 🏭 Industrial Warehouse Test-Fit Simulator
### TESTFIT // WAREHOUSE — Design Simulator v1.0

A browser-based simulator for architects, planners, and developers to quickly test-fit industrial warehouse layouts on any lot shape — no software installation required.

🔗 **Live Demo:** [https://kennethtoledodigitals.github.io/Industrial-Warehouse-Test-Fit-Simulator/](https://kennethtoledodigitals.github.io/Industrial-Warehouse-Test-Fit-Simulator/)

---

## 📐 What It Does

The Industrial Warehouse Test-Fit Simulator lets you define a lot polygon, configure building parameters, and instantly generate a compliant warehouse layout — complete with 2D site plan, 3D massing, GFA breakdown, and zoning compliance report.

It's designed for early-stage feasibility studies where speed matters more than construction documents.

---

## ✨ Features

- **Custom Lot Polygon** — Input any lot shape by entering X,Y coordinates (meters), or choose from presets: Rectangle, L-Shape, Trapezoid, or Irregular
- **Setback Controls** — Define front, rear, left, and right setbacks in meters
- **Building Configuration** — Choose 1 or 2 buildings, 1–3 storeys, floor height, yard width, ramp length, and ramp width
- **Zoning Rules** — Set maximum plinth coverage (%) and minimum landscape area (%)
- **2D Site Plan View** — Visual plan showing lot boundary, setback zone, buildings, yard, ramp, and landscape areas
- **3D Massing View** — Interactive isometric view with drag-to-rotate and scroll-to-zoom
- **GFA Breakdown** — Gross Floor Area summary per building and total
- **Compliance Report** — Instant check against your zoning inputs

---

## 🚀 How to Use

1. **Open the simulator** at the live link above — no account or installation needed
2. **Enter your lot polygon** coordinates (one vertex per line: `X,Y` in meters), or select a preset shape
3. **Set your setbacks** for all four sides
4. **Configure the building** — number of buildings, storeys, floor height, yard, and ramp dimensions
5. **Input zoning rules** — max plinth % and min landscape %
6. **Click ▶ RUN SIMULATION**
7. Explore the **2D Site Plan**, **3D Massing**, **GFA Breakdown**, and **Compliance** tabs

---

## 🖥️ Technical Details

- Built with **HTML, CSS, and JavaScript** — runs entirely in the browser
- No backend, no database, no login required
- Hosted on **GitHub Pages**
- Compatible with modern browsers (Chrome, Firefox, Edge, Safari)

---

## 📁 Repository Structure

```
/
└── index.html       # Main simulator (self-contained)
└── README.md        # This file
```

---

## 🗺️ How to Get Lot Coordinates from Google Maps

You can trace your lot polygon directly from Google Maps and convert the corner points into X,Y coordinates for the simulator.

### Step-by-step:

1. **Go to [Google Maps](https://maps.google.com)** and find your lot
2. **Right-click on a corner point** of your lot → click **"Measure distance"**
3. **Click each corner** of the lot boundary to trace the polygon — Google Maps will show the segment distances (in km or m)
4. **Note down the distances** between each vertex as your polygon edges
5. **Convert to X,Y meters** — pick one corner as your origin `0,0`, then use the distances and direction to calculate each subsequent vertex

### Example (from aerial measurement):

The image below shows a sample irregular lot traced on Google Maps with segment distances labeled:

> Segment distances visible: **2.29 km → 2.00 km → 0.50 km → 1.50 km → 1.00 km**

Using the bottom vertex as origin `0,0`, you'd work clockwise to derive approximate X,Y coordinates in meters for each corner, then paste them into the simulator's **Lot Polygon** field like this:

```
0,0
-1000,500
-500,2000
500,2200
1500,1800
1200,800
```

> 💡 **Tip:** For precise coordinates, you can also use **[Google Earth Pro](https://earth.google.com)** (free) — right-click any point and select *"What's here?"* to get exact lat/long, then use a coordinate converter to get local X,Y in meters.

---

## 👤 Author

**Kenneth Toledo**
- GitHub: [@kennethtoledodigitals](https://github.com/kennethtoledodigitals)

---

## 📄 License

This project is open for viewing and personal use. For commercial use or redistribution, please contact the author.
