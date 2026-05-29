# Celtic Sea Offshore Wind Site Suitability

**Project type:** Multi-criteria raster suitability analysis  
**Study area:** Ireland Exclusive Economic Zone (EEZ), Celtic Sea  
**Analysis date:** May 2026  
**Author:** Conor Fahy

---

## Overview

This project screens Ireland's EEZ for offshore wind development potential using a weighted multi-criteria overlay of five spatial datasets: bathymetry, wind speed, distance from shore, vessel traffic density, and Marine Protected Areas. The output classifies the Celtic Sea into suitability tiers at 500 m resolution.

The analysis targets floating offshore wind, which becomes viable at depths beyond the 50 m limit of fixed monopile foundations. Ireland's EEZ is overwhelmingly deep-water territory, making it one of the highest-potential floating wind locations in Europe.

---

## Data sources

| Dataset | Source | Resolution / Format | CRS |
|---|---|---|---|
| Bathymetry | GEBCO 2026 (15 arc-second grid) | ~460 m / GeoTIFF | EPSG:4326 |
| Wind speed (100 m hub height) | Global Wind Atlas v3.3 | ~250 m / GeoTIFF | EPSG:4326 |
| Vessel density | EMODnet Human Activities (annual avg 2024) | ~1 km / GeoTIFF | EPSG:4326 |
| Marine Protected Areas | OSPAR MPA Network 2021 | Vector polygon / SHP | EPSG:4326 |
| EEZ boundary | Marine Regions World EEZ v12 | Vector polygon / SHP | EPSG:4326 |

---

## Methodology

### Coordinate reference system

All analysis was performed in **EPSG:32629 (WGS 84 / UTM Zone 29N)**, appropriate for the Celtic Sea centred near 9°W. All input rasters were reprojected and resampled to a common 500 m grid at this CRS.

### Target grid

```
CRS: EPSG:32629
Resolution: 500 m
Shape: 1,918 rows × 1,469 columns
Extent: (13,000, 5,336,500) → (747,500, 6,295,500) m
Total cells: ~2.82 million
```

### Processing pipeline

1. **Study area**: Ireland EEZ polygon extracted from Marine Regions v12, reprojected to EPSG:32629.
2. **Bathymetry**: GEBCO 2026 clipped to EEZ bounding box, reprojected to 500 m UTM grid.
3. **Wind speed**: Ireland and UK tiles merged (Global Wind Atlas), reprojected to 500 m UTM grid, land-masked.
4. **Distance from shore**: Binary land/sea mask from GEBCO (elevation ≥ 0 m = land), transformed to distance raster using Euclidean distance transform (scipy).
5. **Vessel density**: EMODnet annual average 2024 reprojected to 500 m UTM grid.
6. **MPA mask**: OSPAR polygons rasterised to 500 m UTM grid (binary 0/1).
7. **Scoring**: Each continuous layer converted to 0–5 integer score using thresholds below.
8. **Weighted overlay**: Composite score computed; hard exclusions applied.
9. **Classification**: Continuous score classified into 8 output tiers.

### Scoring thresholds

**Depth (weight: 40%)**

| Depth range | Score | Rationale |
|---|---|---|
| 0 to −50 m | 1 | Transitional — fixed foundation range, less suited to floating |
| −50 to −200 m | 5 | Optimal for floating wind (semi-sub / spar) |
| −200 to −300 m | 4 | Good — deeper floating viable |
| −300 to −500 m | 2 | Challenging — mooring system complexity increases |
| < −500 m | 0 | Currently beyond feasible range |

**Wind speed at 100 m hub height (weight: 35%)**

| Wind speed | Score |
|---|---|
| < 8.0 m/s | 1 |
| 8.0–9.0 m/s | 2 |
| 9.0–9.5 m/s | 3 |
| 9.5–10.5 m/s | 4 |
| > 10.5 m/s | 5 |

**Distance from shore (weight: 25%)**

| Distance | Score | Rationale |
|---|---|---|
| 0–5 km | 0 | Visual impact exclusion buffer |
| 5–12 km | 1 | Near-shore, cumulative impact concerns |
| 12–30 km | 3 | Moderate — cable export viable |
| 30–60 km | 5 | Optimal — far enough for amenity, close enough for grid connection |
| 60–100 km | 4 | Viable — longer export cable cost |
| > 100 km | 2 | Challenging — grid connection and O&M costs high |

### Weighted composite score

```
score = (depth_score × 0.40) + (wind_score × 0.35) + (distance_score × 0.25)
```

Scores range from 0.0 to 5.0. Hard exclusions are applied after scoring.

### Hard exclusions

- **Shipping lanes**: cells where EMODnet vessel density > 5 units are excluded regardless of score.
- **MPAs**: cells intersecting OSPAR Marine Protected Areas are excluded regardless of score.

### Output classification

| Class | Score range | Label | Colour |
|---|---|---|---|
| 0 | — | Excluded sea (too shallow / too deep) | Pale blue-grey |
| 1 | 0–2.0 | Low suitability | Pale yellow |
| 2 | 2.0–3.0 | Moderate-Low | Amber |
| 3 | 3.0–3.75 | Moderate-High | Orange |
| 4 | 3.75–4.5 | High | Red |
| 5 | > 4.5 | Very High | Deep crimson |
| 6 | — | MPA excluded | Green |
| 7 | — | Shipping lane excluded | Dark grey |

---

## Key findings

### Suitability area summary

| Class | Area (km²) | Share of EEZ |
|---|---|---|
| Very High (>4.5) | 52,872 | 28.0% |
| High (3.75–4.5) | 77,298 | 41.0% |
| Moderate-High (3.0–3.75) | 29,309 | 15.5% |
| Moderate-Low (2.0–3.0) | 6,458 | 3.4% |
| Low (<2.0) | 1,413 | 0.7% |
| MPA excluded | 2,844 | 1.5% |
| Shipping excluded | 927 | 0.5% |

**Viable area (≥ Moderate-High, score ≥ 3.0): 159,479 km²** — approximately 84.5% of analysed sea area.

### Wind speed distribution

Celtic Sea wind speeds at 100 m hub height range from **7.10 to 11.28 m/s** (mean: 10.51 m/s). The distribution is strongly concentrated at the high end: the vast majority of offshore cells score 4 or 5 on the wind criterion. Wind speed is not the limiting factor in this analysis.

### Primary constraint: depth

Depth is the main discriminating variable. The optimal floating wind range (50–200 m) covers a large arc of the mid-shelf, broadly corresponding to the high suitability zone visible in the northeast of the study area. The very deep water (>500 m) to the northwest and west contributes the excluded class.

### MPA and shipping footprint

OSPAR MPAs cover 2,844 km² — a relatively small fraction of the EEZ but spatially concentrated in ecologically sensitive shelf areas. Shipping lane exclusions (927 km²) are similarly limited in extent, mainly concentrated along the south coast approaches.

### Spatial pattern

The analysis identifies a broad arc of Very High and High suitability extending northeast of Ireland across the Porcupine Bank approaches and into the Celtic Sea basin. The southwest of the EEZ scores lower primarily due to excessive depth. The near-shore buffer (0–5 km from coast) is scored zero across the entire coastline.

---

## Limitations and assumptions

- **Floating wind only**: The analysis does not model fixed-foundation turbines, which are viable only to ~50 m depth. Including fixed-foundation scoring would increase suitability in the nearshore shelf.
- **Distance proxy for grid connection**: Distance from shore is used as a proxy for export cable cost. A more rigorous analysis would use actual grid connection point locations and cable routing costs.
- **Vessel density threshold**: The 5-unit exclusion threshold is a modelling assumption, not a regulatory definition. Actual exclusion zones would be determined through marine spatial planning and port authority consultation.
- **OSPAR MPAs only**: Sub-national or national designations (e.g. SACs, SPAs under EU Habitats Directive) are not included.
- **Static wind resource**: The Global Wind Atlas represents a long-term mean. Site-level energy yield assessments require met-ocean modelling.
- **No seabed geotechnics**: Seabed sediment type, slope, and geohazards are not modelled.
- **No cumulative impact**: Visual, noise, and ecological cumulative impacts are not assessed.

---

## Tools and workflow

All raster processing was performed in Python using:

- `rasterio` — raster I/O, reprojection, resampling
- `numpy` — array operations, scoring logic
- `scipy.ndimage.distance_transform_edt` — Euclidean distance transform for distance-from-shore
- `pyshp` — direct shapefile binary parsing (required for large OSPAR MPA file)
- `geopandas` — vector reprojection and GeoJSON export
- `matplotlib` — print and web map cartography
- `Leaflet.js` — interactive web map

QGIS (desktop) was used for initial data inspection, EEZ filtering, and GEBCO clipping. All analytical processing was handled in Python due to a GDAL/PROJ.db environment conflict on macOS.

---

## Deliverables

| File | Description |
|---|---|
| `outputs/celtic_sea_suitability.tif` | Final classified raster (uint8, classes 0–7, EPSG:32629) |
| `outputs/celtic_sea_suitability_score.tif` | Continuous composite score (float32, EPSG:32629) |
| `outputs/Celtic_Sea_Wind_Suitability_print.png` | 300 dpi print layout |
| `outputs/Celtic_Sea_Wind_Suitability_web.png` | 96 dpi web version |
| `outputs/suitability_overlay.png` | RGBA raster overlay for Leaflet (transparent land) |
| `outputs/celtic_sea_wind_suitability_map.html` | Interactive Leaflet map (self-contained) |
| `CASE_STUDY.md` | This document |

---

## Data citations

- GEBCO Compilation Group (2026). *GEBCO 2026 Grid*. doi:10.5285/1c44ce99-0a0d-5f4f-e063-7086abc0ea0f
- DTU Wind Energy (2023). *Global Wind Atlas v3.3*. https://globalwindatlas.info
- EMODnet Human Activities (2024). *Vessel Density Annual Average 2024*. https://emodnet.eu/en/human-activities
- OSPAR Commission (2021). *OSPAR Marine Protected Areas Network*. https://odims.ospar.org
- Flanders Marine Institute (2023). *World EEZ v12*. https://www.marineregions.org
