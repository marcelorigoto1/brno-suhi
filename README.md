# Surface heat in Brno, 2000–2026

How has the surface urban heat island of Brno (Czech Republic) changed over the last 25 years, where is the city hottest, and why? This repository answers that with free satellite data only: **Landsat Collection 2 Level-2 surface temperature** (Landsat 5, 7, 8, 9) loaded as a **data cube** from the Microsoft Planetary Computer STAC catalogue, processed in one Python notebook.

**[Open the interactive dashboard]([https://YOUR-USERNAME.github.io/brno-surface-heat](https://marcelorigoto1.github.io/brno-suhi/))** · three synchronised maps (early, middle and recent summer) with true colour, surface heat, vegetation and land cover over the ČÚZK orthophoto, a click inspector, the places that warmed or cooled most, and a year-by-year viewer (2000–2026) with a play button. The yearly images are stored in `docs/years/` (also an animated `heat_by_year.gif`), so upload that folder together with `docs/index.html`.

## Method

| Step | What is done |
|---|---|
| Data | All clear summer (June–August) Landsat scenes 2000–2026 with ≥ 70 % of the city cloud-free (`qa_pixel`), 30 m, loaded year by year with `odc-stac` |
| Heat island | Per scene: median urban LST minus median rural LST. Rural = farmland in a 2–12 km ring at the urban elevation (± 40 m, Copernicus DEM), because Brno lies in a basin with cooler forested hills |
| Trend | Summer medians per year, Theil–Sen slope and Mann–Kendall test; per-pixel trend of the heat anomaly |
| Robustness | Trend recomputed with three rural references (farmland, forest, all non-urban) and July-only scenes; scene dates and sensor mix per period |
| Replication | Method of Dobrovolný (2013) for every 5-year period: temperature by land-cover class, class-based heat island, NDVI and building-density regressions on a 300 m grid; his two original scenes (24 May 2001, 15 June 2006) re-processed for validation |
| Explanation | Local Climate Zones (Demuzere et al. 2022), NDVI, OpenStreetMap land use and 80 000+ building footprints, district ranking |
| Land cover per year | Classified from the same Landsat composites (NDVI, MNDWI) so years are comparable |

## Run it

```bash
pip install -r requirements.txt
jupyter lab Brno_SUHI_2000_2026.ipynb      # or upload it to Google Colab
```

No account or API key is needed (the optional MapTiler basemap reads `MAPTILER_KEY` from an environment variable). A full run takes roughly 20–40 minutes and 2–4 GB of RAM; results go to `outputs/` and the dashboard to `docs/index.html`.

On Windows with PostGIS installed, the first cell removes `PROJ_LIB` / `PROJ_DATA` so rasterio uses its own projection database.

## Results

Results of the run of 21 October 2026 (120 clear summer scenes in 26 of 27 summers; land use and 83,773 buildings from OpenStreetMap). Brno's summer surfaces at the morning overpass warmed by about 0.9 °C per decade, at the same rate as the surrounding farmland, once the month of each scene is taken into account. The heat island itself (urban minus farmland) is about 5.8 °C in June, 4.6 °C in July and close to zero in late August, when harvested fields heat up, and it shows no trend after adjusting for month (−0.1 ± 0.3 °C per decade). The apparent rise from 0.3 °C in 2000–2004 to 3.6 °C in 2020–2026 comes from the calendar: 69 % of the early scenes are from August against 29 % recently (`outputs/robustness_month_adjusted.csv`, `robustness_season_sensor.csv`). Relative to forest, the city warmed by 0.3 to 0.7 °C per decade depending on the method (p = 0.06 month-adjusted, 0.02 on annual medians).

The spatial pattern is stable. Compact mid-rise blocks (+5.5 °C), compact low-rise and large low-rise buildings (+5.0 °C) are the hottest Local Climate Zones, dense trees and water are 5.7 °C cooler, and NDVI explains 87 % of the variance on a 300 m grid, with 1.9 °C of cooling per 0.1 NDVI. Industry and transport areas are 6.2 °C warmer than the area mean, and the warmest districts are Slatina, Černovice, Brno-střed, Brno-jih and Židenice. The strongest local warming occurs where farmland was converted to warehouses and industrial halls in the south and east of the city.

## Limitations

Landsat measures **surface** (not air) temperature at about 10:00, so the stronger afternoon and night heat island is not captured. Daytime heat-island values depend on the state of the surrounding farmland (e.g. the 2018 drought), and the record mixes Landsat 5/7 and 8/9 sensors. Land use and buildings come from present-day OpenStreetMap. Collection 2 uses vegetation in its emissivity estimate, which strengthens the NDVI–temperature relationship.

## Publishing the dashboard

The dashboard is a single static HTML file with all data embedded, so GitHub Pages can host it: push the repository, then in Settings, Pages, choose branch `main` and folder `/docs`. The default basemaps (ČÚZK orthophoto, Esri imagery, CARTO) and the Planetary Computer data need no key. A MapTiler key, if used, is visible to anyone who opens the page source; restrict it to your Pages domain under allowed HTTP origins in MapTiler Cloud.

## Data and credits

Landsat: USGS/NASA (public domain), via Microsoft Planetary Computer · Copernicus DEM GLO-30 and ESA WorldCover · © OpenStreetMap contributors (ODbL) · Global LCZ map: Demuzere et al. (2022), *ESSD* 14: 3835–3873 · Dashboard basemaps: orthophoto © ČÚZK, imagery © Esri, © CARTO.

## References

- Dobrovolný, P. (2013). The surface urban heat island in the city of Brno (Czech Republic) derived from land surface temperatures and selected reasons for its spatial variability. *Theoretical and Applied Climatology*, 112, 89–98.
- Geletič, J., Lehnert, M., Dobrovolný, P. (2016). Land surface temperature differences within local climate zones, based on two central European cities. *Remote Sensing*, 8(10), 788.
- Janků, Z., Dobrovolný, P. (2022). Heat waves amplify the urban canopy heat island in Brno, Czechia. *Meteorology*, 1(4), 30.
- Kirschner, V. et al. (2024). Comparing the effects of green and blue bodies and urban morphology on land surface temperatures close to rivers and large lakes. *Land*, 13(2), 162.

## License

Code: MIT (see `LICENSE`). Data keep their original licenses listed above.
