# Transportation Route & CO₂e Calculator

A single-page web application that lets you **compare land, sea, and air transport routes** and estimate well-to-wheel CO₂-equivalent emissions.  
Built with Leaflet, Turf.js, and public routing / geocoding APIs, the simulator is completely client-side—no backend required.
> **Methodology:** Emission factors and calculation logic follow the **Global Logistics Emissions Council (GLEC) Framework v3.4** (2024) guidance for *default* intensity values and tonne-kilometre aggregation.

---

## Features

|                                     | Land | Sea | Air |
|-------------------------------------|:----:|:---:|:---:|
| Great-circle path & shipping lanes  | ✓    | ✓   | ✓   |
| Multiple intermediate way-points    | ✓    | ✓   | ✓   |
| Nearest hub auto-selection          | —     | ✓   | ✓   |
| Interactive Leaflet map             | ✓    | ✓   | ✓   |
| Ton ↔ TEU cargo conversion          | ✓    | ✓   | ✓   |
| Real-time distance & CO₂e summary   | ✓    | ✓   | ✓   |
| Search history → CSV (UTF-8 BOM)    | ✓    | ✓   | ✓   |

*Double-click* the map to add / remove a waypoint.  
*Clear Waypoints* resets the route without refreshing the page.

---

## Tech Stack

| Layer        | Library / Service                                                 |
|--------------|-------------------------------------------------------------------|
| Maps & UI    | [Leaflet 1.9](https://leafletjs.com) + OpenStreetMap tiles         |
| Geodesics    | Native JS great-circle helper + [Turf.js 6](https://turfjs.org)    |
| Geo-routing  | **Land:** OSRM demo server<br>**Sea:** IMO shipping-lane GeoJSON<br>**Air:** great-circle interpolation |
| Geo-coding   | [Nominatim (OSM)](https://nominatim.openstreetmap.org)             |
| Data sets    | UN/LOCODE ports, world shipping lanes, airports JSON (mwgg)        |
| Tooling      | No build step—vanilla HTML/CSS/JS                                  |

---

## Configuration

| Variable                      | File / Section            | Default                               | Notes                                                                                |
| ----------------------------- | ------------------------- | ------------------------------------- | ------------------------------------------------------------------------------------ |
| `EF_ROAD`, `EF_SEA`, `EF_AIR` | `index.html` → `<script>` | 0.062 / 0.016 / 0.500 (t-CO₂e / t-km) | Update if you follow a different GHG methodology.                                    |
| `TARGET_WPI`, `TARGET_IATA`   | same                      | Main global hubs                      | Set to `[]` if you want **all** ports / airports displayed (may impact performance). |
| Tiles URL                     | Leaflet layer             | OSM                                   | Replace with Mapbox, Thunderforest, etc. (API key may be required).                  |

---

## CSV Output

Each successful search appends (or updates) a row in an in-memory log:

| Column                        | Description                                       |
| ----------------------------- | ------------------------------------------------- |
| `Timestamp`                   | ISO 8601 (UTC)                                    |
| `Mode`                        | `land` \| `sea` \| `air`                          |
| `Origin`, `Destination`       | Raw address strings you entered                   |
| `HubOrigin`, `HubDestination` | Port or airport codes (blank for land-only)       |
| `Cargo`, `Unit`               | Numeric value + `ton`/`teu`                       |
| `Distance_km`                 | Route length incl. way-points **after** last edit |
| `CO2e_t`                      | Total WTW emissions (t-CO₂e)                      |

**The Download CSV** button prepends a UTF-8 BOM so Excel reads non-ASCII addresses correctly.

---

## Contributing

1. Fork the repo and create your branch: git checkout -b feat/my-feature.
2. Commit your changes: git commit -m 'feat: add my feature'.
3. Push to the branch: git push origin feat/my-feature.
4. Open a pull request 🙏

### Road-map ideas
- Container size / mass breakout
- Rail / inland-waterway modes
- Region-specific emission factors (Well-to-Tank vs Tank-to-Wheel)
- Offline PWA support + service-worker caching
- Congestion heat-map overlay for shipping lanes

---

## License
Released under the MIT License. See [LICENSE](https://github.com/miumigy/rtco2/blob/main/LICENSE) for details.
