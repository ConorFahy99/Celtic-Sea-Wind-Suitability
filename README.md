# Celtic Sea - Offshore Wind Site Suitability

Multi-criteria raster suitability screening of Ireland's EEZ for floating offshore wind development. Built in Python and QGIS as a GIS portfolio project.

**[→ View interactive map](https://conorfahy99.github.io/Celtic-Sea-Wind-Suitability/outputs/celtic_sea_wind_suitability_map.html)**  
**[→ View findings infographic](https://conorfahy99.github.io/Celtic-Sea-Wind-Suitability/outputs/findings_infographic.html)**

---

## Key findings

| Class | Score | Area (km²) | Share |
|---|---|---|---|
| Very High | > 4.5 | 52,872 | 28.0% |
| High | 3.75–4.5 | 77,298 | 41.0% |
| Moderate-High | 3.0–3.75 | 29,309 | 15.5% |
| Moderate-Low | 2.0–3.0 | 6,458 | 3.4% |
| Low | < 2.0 | 1,413 | 0.7% |
| MPA excluded | - | 2,844 | 1.5% |
| Shipping excluded | - | 927 | 0.5% |

**159,479 km² viable (≥ Moderate-High) - 84.5% of the analysed EEZ.**

Mean wind speed at 100 m hub height: **10.51 m/s**. Wind resource is consistently strong across the entire EEZ; depth is the primary limiting factor.

---

## Methodology

### Criteria and weights

| Criterion | Weight | Data source |
|---|---|---|
| Water depth | 40% | GEBCO 2026 (15 arc-second) |
| Wind speed (100 m) | 35% | Global Wind Atlas v3.3 |
| Distance from shore | 25% | Derived from GEBCO land mask |

**Hard exclusions** applied after scoring:
- Shipping density > 5 units (EMODnet vessel density, annual average 2024)
- OSPAR Marine Protected Areas

### Analysis parameters

- **CRS:** EPSG:32629 (WGS 84 / UTM Zone 29N)
- **Resolution:** 500 m
- **Study area:** Ireland EEZ (Marine Regions World EEZ v12)
- **Grid size:** 1,918 rows × 1,469 columns

### Scoring thresholds

**Depth:**
| Range (m) | Score |
|---|---|
| 0 to −50 | 1 - fixed-foundation range |
| −50 to −200 | 5 - optimal floating wind |
| −200 to −300 | 4 |
| −300 to −500 | 2 |
| < −500 | 0 - beyond current limits |

**Wind speed (m/s):** < 8.0 → 1 | 8.0–9.0 → 2 | 9.0–9.5 → 3 | 9.5–10.5 → 4 | > 10.5 → 5

**Distance from shore:** 0–5 km → 0 | 5–12 km → 1 | 12–30 km → 3 | 30–60 km → 5 | 60–100 km → 4 | > 100 km → 2

**Composite score:** `(depth × 0.40) + (wind × 0.35) + (distance × 0.25)`

---

## Data sources

| Dataset | Source | Year |
|---|---|---|
| Bathymetry | [GEBCO 2026](https://www.gebco.net) | 2026 |
| Wind speed | [Global Wind Atlas v3.3](https://globalwindatlas.info) | 2023 |
| Vessel density | [EMODnet Human Activities](https://emodnet.eu/en/human-activities) | 2024 |
| Marine Protected Areas | [OSPAR MPA Network](https://odims.ospar.org) | 2021 |
| EEZ boundary | [Marine Regions EEZ v12](https://www.marineregions.org) | 2023 |

---

## Deliverables

```
outputs/
├── celtic_sea_wind_suitability_map.html   - interactive Leaflet map (self-contained)
├── findings_infographic.html              - standalone findings page with charts
├── Celtic_Sea_Wind_Suitability_print.png  - 300 dpi print layout
├── Celtic_Sea_Wind_Suitability_web.png    - 96 dpi web version
├── celtic_sea_suitability.tif             - classified raster (uint8, classes 0–7)
└── celtic_sea_suitability_score.tif       - continuous score (float32)
```

---

## Tools

- **Python:** rasterio · numpy · scipy · geopandas · pyshp · matplotlib
- **QGIS:** data inspection, EEZ filtering, initial clip
- **Leaflet.js:** interactive web map

---

## Author

Conor Fahy - May 2026  
[LinkedIn](https://www.linkedin.com/in/conor-fahy) · [GitHub](https://github.com/ConorFahy99)
