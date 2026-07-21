# GitHub setup guide

Steps to publish this project with a live GitHub Pages URL, same as MetroLink.

---

## 1. Create the repo on GitHub

1. Go to https://github.com/new
2. Repository name: `Celtic-Sea-Wind-Suitability`
3. Description: `Multi-criteria offshore wind site suitability analysis - Ireland EEZ, Celtic Sea`
4. Set to **Public**
5. Leave "Add a README file" **unchecked** (you already have one)
6. Click **Create repository**

---

## 2. Initialise and push from your project folder

Open Terminal and run these commands one at a time.

```bash
cd "/Users/conorfahy/Desktop/QGIS Work/Offshore Wind Suitability - Celtic Sea"
```

```bash
git init
git add README.md CASE_STUDY.md outputs/
git commit -m "Initial commit: Celtic Sea offshore wind suitability analysis"
```

```bash
git branch -M main
git remote add origin https://github.com/ConorFahy99/Celtic-Sea-Wind-Suitability.git
git push -u origin main
```

> If Git asks for credentials, use your GitHub username and a **Personal Access Token** (not your password). Generate one at: https://github.com/settings/tokens - select `repo` scope.

---

## 3. Enable GitHub Pages

1. Go to your repo on GitHub
2. **Settings → Pages**
3. Source: **Deploy from a branch**
4. Branch: `main` · Folder: `/ (root)`
5. Click **Save**

GitHub will show a green banner with your URL after 1–2 minutes:
```
https://conorfahy99.github.io/Celtic-Sea-Wind-Suitability/
```

---

## 4. Live URLs

Once Pages is active:

| Page | URL |
|---|---|
| Interactive map | `https://conorfahy99.github.io/Celtic-Sea-Wind-Suitability/outputs/celtic_sea_wind_suitability_map.html` |
| Findings infographic | `https://conorfahy99.github.io/Celtic-Sea-Wind-Suitability/outputs/findings_infographic.html` |

> Always test in **incognito** (`Cmd+Shift+N`) after a new push - the browser caches old versions.

---

## 5. Update the README links

Once you have the live URLs, open `README.md` and replace the placeholder links near the top:

```markdown
**[→ View interactive map](https://conorfahy99.github.io/Celtic-Sea-Wind-Suitability/outputs/celtic_sea_wind_suitability_map.html)**
**[→ View findings infographic](https://conorfahy99.github.io/Celtic-Sea-Wind-Suitability/outputs/findings_infographic.html)**
```

Then push the update:
```bash
git add README.md
git commit -m "Add GitHub Pages links to README"
git push
```

---

## 6. Files being pushed

The `outputs/` directory contains:

```
outputs/
├── celtic_sea_wind_suitability_map.html   296 KB  - interactive map (self-contained)
├── findings_infographic.html               21 KB  - findings page with charts
├── Celtic_Sea_Wind_Suitability_web.png    217 KB  - web map image
├── Celtic_Sea_Wind_Suitability_print.png  884 KB  - print layout
├── celtic_sea_suitability.tif             109 KB  - classified raster (GIS use)
├── celtic_sea_suitability_score.tif       953 KB  - continuous score raster (GIS use)
└── suitability_overlay.png                 56 KB  - Leaflet overlay layer
```

Total push: ~2.5 MB - well within GitHub's limits. The raw data files (`data/`) are large (several GB) and are excluded from the commit intentionally.

---

## 7. Add to your CV/portfolio

Once live, add the map URL to:
- Your CV under Projects
- Your LinkedIn profile (Featured section or Projects)
- Any cover letters referencing spatial analysis or offshore wind work
