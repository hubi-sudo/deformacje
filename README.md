# EGMS — Jastrzębie-Zdrój: anomalie i wzorce deformacji terenu

Projekt z przedmiotu *Analiza i przetwarzanie danych przestrzennych*. Na podstawie szeregów czasowych przemieszczeń z European Ground Motion Service (EGMS) wskazujemy nietypowe punkty (Isolation Forest), grupujemy punkty o podobnym zachowaniu (K-means) i przypisujemy wyniki do budynków.

## Obszar i dane

| | |
|---|---|
| Obszar | Jastrzębie-Zdrój i okolice (ok. 13,4 × 9,9 km), teren eksploatacji węgla kamiennego |
| Przemieszczenia | EGMS Ortho (L3), siatka 100 m — 3977 punktów, 239 obserwacji od 2020-03-08 do 2024-12-06 |
| Budynki | EUBUCCO v0.2 — 26 847 budynków |

Źródła: [EGMS](https://egms.land.copernicus.eu/), [dokumentacja EGMS](https://land.copernicus.eu/en/products/european-ground-motion-service?tab=documentation), [EUBUCCO](https://eubucco.com/data/download/v0.2).

> **Składowa ruchu.** Rozkład prędkości (symetryczny wokół zera, pary stref o przeciwnych znakach) wskazuje na składową poziomą wschód–zachód EGMS Ortho: wartość dodatnia oznacza ruch na wschód, ujemna — na zachód. Należy to potwierdzić nazwą pobranego pliku (`..._E_...` = wschód–zachód, `..._U_...` = pionowa).

## Struktura repozytorium

| Plik / katalog | Opis |
|---|---|
| `EGMS_Jastrzebie_analiza_PS.ipynb` | skrypt 1: cechy szeregów czasowych, Isolation Forest, K-means, szeregi czasowe, skupiska anomalii, wnioski |
| `EGMS_Jastrzebie_analiza_budynkow.ipynb` | skrypt 2: przypisanie najbliższego PS do budynków (≤ 30 m), mapa budynków, lista budynków do dalszej analizy |
| `dane/` | dane wejściowe: punkty EGMS (`Jastrzebie_zdroj_punkty.csv`) i warstwa budynków (`budynki_jastrzebie_done.gpkg`) |
| `EGMS_Jastrzebie_results.csv` | wynik skryptu 1: cechy, `anomaly_score`, `anomaly`, `cluster` dla każdego PS |
| `EGMS_Jastrzebie_przykladowe_PS.csv` | punkty wybrane do analizy szeregów czasowych |
| `budynki_Jastrzebie_EGMS_results.gpkg` | wynik skryptu 2: budynki z przypisanym PS (EPSG:2180) |
| `budynki_do_dalszej_analizy.csv` | 400 budynków z anomalnym PS, z numerem skupiska anomalii |
| `warstwy.gpkg` | warstwy do QGIS z zapisanymi stylami (klastry, anomalie, budynki) |
| `rysunki/` | mapy i wykresy zapisywane przez notebooki |

## Uruchomienie

1. Dane wejściowe są w katalogu `dane/` (opis w `dane/README.md`).
2. Uruchom `EGMS_Jastrzebie_analiza_PS.ipynb` — zapisuje `EGMS_Jastrzebie_results.csv` i rysunki.
3. Uruchom `EGMS_Jastrzebie_analiza_budynkow.ipynb` — zapisuje warstwę budynków, mapę i listę budynków.

Lokalnie: `pip install -r requirements.txt`. W Google Colab:

```python
!git clone <adres repozytorium>
%cd <nazwa katalogu repozytorium>
```

Mapy z podkładem OSM przygotowano w QGIS na podstawie `warstwy.gpkg` (style zapisane w pliku).

## Najważniejsze wyniki

- **Anomalie:** 199 PS (5%). Są to głównie rzeczywiste, gładkie przemieszczenia rzędu 80–160 mm w niespełna 5 lat oraz punkty z gwałtowną zmianą tempa w 2024 r.
- **Klastry (k = 2):** klaster 0 — stały charakter ruchu (3405 PS), klaster 1 — zmiana tempa w ostatnim roku (572 PS). Silhouette dla k = 2 i k = 3 jest praktycznie równy (0,427); wybrano prostszy podział.
- **Anomalie a klastry:** w klastrze 1 anomalie są 5,6 razy częstsze (16,8% vs 3,0%).
- **Skupiska:** 72% anomalii ma inną anomalię w promieniu 150 m (przy losowym wyborze 25%, p = 0,001). 10 skupisk liczących co najmniej 5 PS leży na brzegach stref silnego ruchu poziomego (do ±37 mm/rok), które otaczają tereny bez PS — najpewniej brzegi niecek osiadania nad eksploatacją górniczą.
- **Budynki:** PS przypisano do 9161 budynków (34%). 400 budynków ma anomalny PS, w tym 173 w skupiskach anomalii. Najwięcej jest ich na wschodnim skraju zwartej zabudowy centrum (skupisko 1, 51 budynków).
