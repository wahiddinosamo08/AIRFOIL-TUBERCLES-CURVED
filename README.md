# Helical Turbine Blade with Leading-Edge Tubercles

> Bio-inspired VAWT blade design — NACA 0018 | 60° helix angle | Parametric SolidWorks equations | Interactive 3D visualiser

---

## Overview

This repository contains the complete parametric design of a **helical vertical-axis turbine (VAWT) blade** with **bio-inspired leading-edge tubercles** — sinusoidal bumps modelled after the knuckles on humpback whale (*Megaptera novaeangliae*) flippers.

These tubercles delay flow separation, reduce stall severity, and improve hydrodynamic performance in both pre-stall and post-stall regimes. The geometry is fully parametric: changing any one input value (radius, helix angle, chord, amplitude, or tubercle count) automatically recalculates all derived constants and updates the SolidWorks equations.

---

## Repository Contents

| File | Description |
|---|---|
| `blade_3d_visualizer.html` | Interactive 3D design tool — live sliders, 3D viewport, copyable SW equations |
| `tubercle_blade_guide.docx` | Full technical document — equations, methodology, SolidWorks guide (8 sections) |
| `LE_xy.txt` | Leading edge XYZ point cloud (1001 points) — import via Insert → Curve → XYZ Points |
| `TE_xy.txt` | Trailing edge XYZ point cloud (1001 points) |
| `blade_model.SLDPRT` | SolidWorks 2023 part file |
| `blade_model.STEP` | Universal CAD export — open in Fusion 360, FreeCAD, CATIA, etc. |

---

## Design Specifications

| Parameter | Symbol | Value |
|---|---|---|
| Airfoil profile | — | NACA 0018 |
| Chord length | c | 100.6 mm |
| Blade arc length | L | 400 mm |
| Helix angle | α | 60° |
| Turbine radius (to TE) | R | 150 mm |
| Axial height | H | 200 mm |
| Tubercle amplitude | A | 8 mm (8% chord) |
| Number of tubercles | N | 5 |
| Tubercle wavelength | λ | 80 mm |

---

## Parametric Equations

Both curves share the same parametric variable **t**, ranging from `0` to `2.3094 rad` (132.3°).  
The sine wave oscillates entirely in the **XY plane** — the Z coordinate is identical for TE and LE at every point.

### Trailing Edge — pure helix, origin at (0, 0, 0)

```
x(t) = 150 * cos(t)
y(t) = 150 * sin(t)
z(t) = 86.6025 * t

t1 = 0     t2 = 2.309401
```

### Leading Edge — same helix + chord offset + sinusoidal tubercles

```
x(t) = (150 + 100.6 + 8 * sin(13.6035 * t)) * cos(t)
y(t) = (150 + 100.6 + 8 * sin(13.6035 * t)) * sin(t)
z(t) = 86.6025 * t

t1 = 0     t2 = 2.309401
```

### Where the numbers come from

| Constant | Formula | Value | Meaning |
|---|---|---|---|
| P | 2π × R / tan(α) | 544.14 mm | Helix pitch — rise per full revolution |
| zr | P / (2π) | 86.6025 mm/rad | Z-rise per radian of t |
| aper | √(R² + zr²) | 173.2051 mm/rad | Arc length per radian of t |
| t_end | arc_len / aper | 2.3094 rad | Total parametric range |
| freq | N × 2π / t_end | 13.6035 | Sine frequency — ensures exactly N tubercles |

---

## The Interactive 3D Tool

Open `blade_3d_visualizer.html` in any modern browser (Chrome, Edge, Firefox — no install needed).

**Left panel** — six parameter sliders. All derived values and equations update in real time.  
**Centre** — 3D viewport. Drag to rotate, scroll to zoom. Switch between 3D, Top (XY), Front (XZ), Side (YZ) views.  
**Right panel** — three-column equation table: variable name | symbolic form | SolidWorks result. Every cell has an individual copy button.  
**Bottom of right panel** — fully expanded, numerically substituted equations ready to paste directly into SolidWorks.

### Research ratio indicators

| Ratio | Current value | Published range | Status |
|---|---|---|---|
| A / chord | 7.95% | 2.5% – 12% | ✅ within range |
| λ / chord | 79.5% | 25% – 50% | ⚠️ above typical — increase N to tighten |

---

## SolidWorks Workflow (Summary)

1. **Tools → Equations → Global Variables** — enter all 10 variables (see tool or guide)
2. **Sketch Ø300 mm circle on Top Plane** — centred at origin (0, 0, 0)
3. **Insert → Curve → Helix/Spiral** — Height 200 mm, Pitch 544.14 mm, Start angle 0° → this is the TE
4. **Insert → Curve → Equation Driven Curve (Parametric)** — paste the 3 LE equations above, t1=0, t2=2.309401
5. **Place 12 NACA 0018 sketches** on planes normal to TE at z = 0, 10, 30, 50, 70, 90, 110, 130, 150, 170, 190, 200 mm
6. **Insert → Boss/Base → Loft** — profiles in order, TE and LE curves as guide rails

Full step-by-step instructions with loft section coordinates and all derived values are in `tubercle_blade_guide.docx`.

---

## Physical Principle — Why Tubercles Work

Each tubercle peak generates a **streamwise vortex** that energises the boundary layer on the downstream surface, delaying flow separation at higher angles of attack. The result is:

- A higher effective stall angle
- Softer, more gradual stall behaviour (no abrupt lift drop)
- Improved performance in turbulent or variable-flow conditions

This mechanism was first characterised by Fish & Lauder (2006) and has since been validated on wind and tidal turbine blades including NACA 0018 profiles (Hansen et al. 2011, Miklosovic et al. 2004).

---

## CFD Next Steps

The geometry is export-ready for meshing. Recommended workflow:

```
SolidWorks (.SLDPRT) → Export .STEP → Import to ANSYS Fluent / OpenFOAM / StarCCM+
```

Suggested parametric study: vary **A/c** ∈ {0.025, 0.05, 0.10} and **λ/c** ∈ {0.25, 0.30, 0.50} independently at fixed helix geometry. This gives a 3×3 = 9-case matrix suitable for a peer-reviewed CFD comparison.

---

## References

- Fish, F.E. & Lauder, G.V. (2006). Passive and active flow control by swimming fishes and mammals. *Annual Review of Fluid Mechanics*, 38, 193–224.
- Miklosovic, D.S., Murray, M.M., Howle, L.E. & Fish, F.E. (2004). Leading-edge tubercles delay stall on humpback whale flippers. *Physics of Fluids*, 16(5), L39–L42.
- Hansen, K.L., Kelso, R.M. & Dally, B.B. (2011). Performance variations of leading-edge tubercles for distinct airfoil profiles. *AIAA Journal*, 49(1), 185–194.
- Pedro, H.T.C. & Kobayashi, M.H. (2008). Numerical study of stall delay on humpback whale flippers. *46th AIAA Aerospace Sciences Meeting*.

---

## License

MIT License — free to use, modify, and distribute with attribution.

---

*Designed and developed as part of an engineering research project on bio-inspired VAWT blade geometry.*  
*Equations, 3D tool, and documentation generated parametrically — all values recalculate from 6 physical inputs.*
