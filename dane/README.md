# Dane wejściowe

Dane wejściowe obu notebooków:

| Plik | Zawartość | Skąd |
|---|---|---|
| `Jastrzebie_zdroj_punkty.csv` | punkty EGMS Ortho przycięte do obszaru Jastrzębia-Zdroju: `pid`, `easting`, `northing` (EPSG:3035), parametry (`mean_veloc`, `accelerati`, `seasonalit`, `rmse` …) i kolumny dat `YYYYMMDD` | [EGMS](https://egms.land.copernicus.eu/), wycięte w QGIS |
| `budynki_jastrzebie_done.gpkg` | obrysy budynków z określonym CRS | [EUBUCCO v0.2](https://eubucco.com/data/download/v0.2), wycięte do obszaru |
