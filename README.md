
# Grain Storage Deficit in Brazil — 2024

Interactive dashboard cross-referencing **grain production** and **static storage capacity** municipality by municipality, revealing where Brazil cannot store what it harvests.

**🔗 Live dashboard:** https://digobaptistagarcia.github.io/Brazilian_Storage_Defict_2024/

<img width="1900" height="937" alt="image" src="https://github.com/user-attachments/assets/5943e3a7-8ebb-4a4e-b9b8-9bfc70254505" />


---

## What the dashboard shows

Brazil harvested **263 Mt** of grain in 2024 but had capacity to store only **193 Mt** — a **deficit of 70 Mt (26.7%)**, or a coverage ratio of just **73.3%**. The problem is not uniform: while the South runs a surplus, the Center-West, North and Northeast face structural deficits that push grain into open-air storage and harvest-time sales, when prices are worst.

| Metric | Value |
|---|---|
| Total production (2024) | ~263 Mt |
| Static capacity | ~193 Mt |
| Deficit | ~70 Mt (−26.7%) |
| Coverage ratio | 73.3% |
| Municipalities analyzed | 5,565 |

**Coverage by region** (capacity ÷ production): South +8.5% (surplus) · Southeast −6.5% · Center-West −39.4% · Northeast −44.3% · North −54.8%.

---

## Sources and methodology

Data comes from two official public sources, aggregated by municipality via **IBGE code**.

**Storage capacity — CONAB.** Static storage capacity, filtering only the **bulk silo** (*silo graneleiro*) and **silo battery** (*bateria de silos*) types — the units designed for bulk grain.

**Production — PAM / IBGE.** Municipal Agricultural Production (*Produção Agrícola Municipal*), year **2024**, filtering the grains **soybean, corn, sunflower and sorghum**.

**Geographic coordinates — IBGE.** Coordinates released by IBGE, used to position municipalities on the maps.

**Aggregation.** The entire dataset is consolidated by municipality using the **IBGE code**, enabling a direct join between production, capacity and geometry.

### Sign convention

The deficit is calculated as:

```
deficit = capacity − production
```

That is: a **negative value = storage shortfall** (produces more than it stores) and a **positive value = surplus** (stores more than it produces). This convention is applied across all KPIs, charts and maps in the dashboard.

---

## How to use

**Online:** just open the [live dashboard](https://digobaptistagarcia.github.io/Brazilian_Storage_Defict_2024/).

**Locally:** the dashboard is 100% static (HTML + JS, no backend and no build step). Since the data is loaded from local `.js` files, don't open `index.html` directly via `file://` — serve the folder with a simple local server:

```bash
# from the repository root
python -m http.server 8000
# then open http://localhost:8000 in your browser
```

Filters work as a cascade (Region → State → Municipality) and all KPIs, charts and maps react in real time to the selection.

---

## Repository structure

```
├── index.html                # dashboard (interface, filters, charts and maps)
├── dados.js                  # production/capacity/deficit data pre-aggregated by municipality
├── geo.js                    # simplified geometries (municipalities + states) and centroids
├── geojson_UF_municipios/    # raw geojson of states and municipalities (geometry source)
└── Storage Defict 2024.xlsx  # source spreadsheet with the original data
```

---

## Limitations and caveats

Transparency about what the data does **not** cover:

- **Single-year snapshot.** The dataset covers only 2024; there is no historical series, so the dashboard is a snapshot, not a trend.
- **Capacity not segmented by crop.** CONAB capacity is a municipal total, not split by grain — deficit by crop is an inference, not a direct measurement.
- **Coordinates.** The latitudes/longitudes in the original spreadsheet showed deviations; the maps use the **centroids of official IBGE geometries** instead, which are more reliable.
- **One geometry gap.** The municipality of Nazária/PI (code 2206720) has no matching geometry in the geojson and is left off the maps (marginal production, ~207 t).

---

## Tech stack

Vanilla HTML/CSS/JavaScript · [Leaflet 1.9.4](https://leafletjs.com/) for the maps (local geometry, no external tiles) · libraries via CDN · published on GitHub Pages.
