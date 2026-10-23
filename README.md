# Surface heat in Brno, 2000–2026

How has the surface urban heat island of Brno (Czech Republic) changed over the last 25 years, where is the city hottest, and why? This repository answers that with free satellite data only: **Landsat Collection 2 Level-2 surface temperature** (Landsat 5, 7, 8, 9) loaded as a **data cube** from the Microsoft Planetary Computer STAC catalogue, processed in one Python notebook.

**https://marcelorigoto1.github.io/brno-suhi/** · three synchronised maps (early, middle and recent summer) with true colour, surface heat, vegetation and land cover over the ČÚZK orthophoto, a click inspector, the places that warmed or cooled most, and a year-by-year viewer (2000–2026) with a play button. The yearly images are stored in `docs/years/` (also an animated `heat_by_year.gif`), so upload that folder together with `docs/index.html`.

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

## Data and credits

Landsat: USGS/NASA (public domain), via Microsoft Planetary Computer · Copernicus DEM GLO-30 and ESA WorldCover · © OpenStreetMap contributors (ODbL) · Global LCZ map: Demuzere et al. (2022), *ESSD* 14: 3835–3873 · Dashboard basemaps: orthophoto © ČÚZK, imagery © Esri, © CARTO.

## References

- Dobrovolný, P. (2013). The surface urban heat island in the city of Brno (Czech Republic) derived from land surface temperatures and selected reasons for its spatial variability. *Theoretical and Applied Climatology*, 112, 89–98.
- Geletič, J., Lehnert, M., Dobrovolný, P. (2016). Land surface temperature differences within local climate zones, based on two central European cities. *Remote Sensing*, 8(10), 788.
- Janků, Z., Dobrovolný, P. (2022). Heat waves amplify the urban canopy heat island in Brno, Czechia. *Meteorology*, 1(4), 30.
- Kirschner, V. et al. (2024). Comparing the effects of green and blue bodies and urban morphology on land surface temperatures close to rivers and large lakes. *Land*, 13(2), 162.

## License

Code: MIT (see `LICENSE`). Data keep their original licenses listed above.
