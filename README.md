# 🏔️ Above the Clouds – Lower Silesia (south)

**English** · [Polski](#-po-polsku)

An interactive, single-file web map that shows **where the terrain rises above the top of the cloud layer** (the "sea of clouds" view) in the southern part of Lower Silesia and the neighbouring Czech Sudetes – plus **fog** and **mountain peaks** – for the next **48 hours**.

> **Live demo:** `https://<your-username>.github.io/<repository>/` *(replace after deployment)*
>
> ![Screenshot](docs/screenshot.png) *(add a screenshot to `docs/screenshot.png`)*

## Features

- **Terrain vs. clouds.** Grey = terrain inside a cloud layer. For terrain *above* the cloud top the colour shows the cloud cover in oktas: pale blue 6–8, green 4–6, yellow 2–4, red 1–2, black 0. No colour = below the cloud base or no clouds.
- **Fog (estimate).** Violet where 2 m relative humidity exceeds an adjustable threshold.
- **Peaks ▲.** A built-in list of peaks (name and height); positions are resolved with the Open-Meteo geocoding API (GeoNames) and snapped to the matching summit in the terrain data. Each peak is shown as *above clouds / in cloud / below clouds / no clouds*. A dashed outline marks an **uncertain** result (margin < 300 m or neighbouring weather points disagree).
- **48-hour window table.** For the highest and watched peaks: when are they above the clouds? Click a cell to jump to that hour.
- **Click the map** for terrain height and the cloud layers at that point.
- **EN | PL** switch, a how-to popup, a loading banner, and a map locked to the study area (only the needed map tiles are loaded).

## How it works

1. **Weather data** from the [Open-Meteo](https://open-meteo.com) API (DWD **ICON-D2**). ~7 km grid, refined to ~3.5 km wherever terrain exceeds 700 m. Per point: low and mid cloud cover, 2 m relative humidity and cloud cover at 7 pressure levels (975–700 hPa). Pressure-level heights come from a sparse ~15 km grid and are interpolated.
2. **Cloud layers** are contiguous pressure levels whose (humidity-derived) cloud cover exceeds a threshold (default 50 %). Base and top are estimated between levels, so **vertical accuracy is roughly 300–500 m**.
3. **Terrain** comes from AWS Terrain Tiles (Terrarium); each ~720 m cell is compared with the layers interpolated (inverse-distance, 4 nearest points) from the weather grid.
4. **Peaks** are matched to the layers at their position and height; uncertainty is flagged as described above.
5. **Caching and limits.** Forecasts are cached in the browser (IndexedDB, 3 h), the peak list for 30 days. Requests are paced and retried automatically on HTTP 429 (per-minute, hourly and daily limits are recognised).

## Data sources and attribution

| Data | Source | Licence / terms |
|---|---|---|
| Forecast | [Open-Meteo](https://open-meteo.com), DWD ICON | CC BY 4.0 |
| Terrain heights | [AWS Terrain Tiles](https://registry.opendata.aws/terrain-tiles/) (Terrarium) | see source |
| Peak positions | [GeoNames](https://www.geonames.org) via the Open-Meteo Geocoding API | see source |
| Base map | [OpenTopoMap](https://opentopomap.org) | CC-BY-SA |
| Map library | [Leaflet](https://leafletjs.com) 1.9.4 | BSD-2-Clause |

The free Open-Meteo API is for **non-commercial** use and has rate limits per IP address. OpenTopoMap and Overpass are public servers with fair-use policies – check them before sharing the page widely.

## Limitations

- Results are **model-based estimates** for orientation only – **not a substitute for official mountain forecasts** or safety decisions.
- The fog estimate (humidity threshold) and the cloud threshold have **not been validated** against observations; tune them against webcams or meteograms.
- Open-Meteo has announced that the DWD ICON models move to native grids on 30 November 2026. The behaviour of the variables used here after that date has **not been verified**.

## Run locally

Open `index.html` in a modern browser (internet connection required). The first load downloads data and may take a couple of minutes; later loads use the cache.

## Configuration

Constants at the top of the script in `index.html`:

- `POLY_N`, `POLY_S` – boundary of the study area; `BBOX` – bounding box
- `CFG` – grid steps, densification height (`denseAbove`), pacing, cache lifetime
- `MANUAL_PEAKS` – list of peaks (name, height, optionally lat/lon)

## Deploy on GitHub Pages

1. Create a **public** repository and add `index.html` and this `README.md`.
2. *Settings → Pages → Build and deployment → Deploy from a branch → `main` / `(root)` → Save.*
3. After a minute or two the site is available at `https://<your-username>.github.io/<repository>/`.

## License

Add a licence of your choice (e.g. MIT) as `LICENSE`. Note that the data sources above have their own terms.

---

## 🇵🇱 Po polsku

Interaktywna mapa w jednym pliku HTML, która pokazuje, **gdzie teren wznosi się ponad wierzchołek warstwy chmur** („morze chmur") w południowej części Dolnego Śląska i w sąsiednich czeskich Sudetach – a także **mgłę** i **szczyty** – na najbliższe **48 godzin**.

> **Demo:** `https://<twoja-nazwa>.github.io/<repozytorium>/` *(podmień po wdrożeniu)*

### Funkcje

- **Teren a chmury.** Szary = teren w warstwie chmur. Dla terenu *ponad* wierzchołkiem chmur kolor oznacza zachmurzenie w oktantach: blady niebieski 6–8, zielony 4–6, żółty 2–4, czerwony 1–2, czarny 0. Brak koloru = pod podstawą chmur lub bez chmur.
- **Mgła (szacunek).** Fiolet tam, gdzie wilgotność względna na 2 m przekracza regulowany próg.
- **Szczyty ▲.** Wbudowana lista szczytów (nazwa i wysokość); położenie jest ustalane geokodowaniem Open-Meteo (GeoNames) i dociągane do pasującego szczytu w danych terenu. Stan: *ponad chmurami / w chmurze / pod chmurami / bez chmur*. Przerywany kontur = wynik **niepewny** (zapas < 300 m lub rozbieżność sąsiednich punktów pogodowych).
- **Tabela okien na 48 godzin** dla najwyższych i wybranych szczytów; kliknięcie komórki przechodzi do danej godziny.
- **Kliknięcie w mapę** pokazuje wysokość terenu i warstwy chmur w tym miejscu.
- Przełącznik **EN | PL**, okienko z instrukcją, baner ładowania i mapa ograniczona do obszaru (ładowane są tylko potrzebne kafelki).

### Jak to działa

1. **Dane pogodowe:** API [Open-Meteo](https://open-meteo.com) (DWD **ICON-D2**). Siatka ok. 7 km, zagęszczana do ok. 3,5 km tam, gdzie teren przekracza 700 m. Na punkt: zachmurzenie niskie i średnie, wilgotność na 2 m i zachmurzenie na 7 poziomach ciśnienia (975–700 hPa). Wysokości poziomów pochodzą z rzadkiej siatki ok. 15 km i są interpolowane.
2. **Warstwy chmur** to ciągłe poziomy ciśnienia, dla których zachmurzenie (wyliczane z wilgotności) przekracza próg (domyślnie 50 %). Podstawa i wierzchołek są szacowane między poziomami, więc **dokładność pionowa to ok. 300–500 m**.
3. **Teren:** AWS Terrain Tiles (Terrarium); każda komórka ok. 720 m jest porównywana z warstwami interpolowanymi (odwrotność odległości, 4 najbliższe punkty).
4. **Szczyty** są dopasowywane do warstw w swoim położeniu i na swojej wysokości.
5. **Pamięć podręczna i limity:** prognozy 3 h, lista szczytów 30 dni (IndexedDB w przeglądarce). Zapytania są rozłożone w czasie i automatycznie ponawiane po HTTP 429.

### Źródła danych

Tabela źródeł i licencji – patrz wersja angielska powyżej. Darmowe API Open-Meteo jest do użytku **niekomercyjnego** i ma limity na adres IP.

### Ograniczenia

- Wyniki to **szacunki z modelu**, tylko orientacyjne – **nie zastępują oficjalnych prognoz górskich** ani decyzji dotyczących bezpieczeństwa.
- Szacunek mgły i próg chmury **nie zostały zwalidowane** z obserwacjami – dostrajaj je na podstawie kamerek lub meteogramów.
- Open-Meteo zapowiedziało przejście modeli DWD ICON na siatki natywne od 30 listopada 2026. Działanie użytych tu zmiennych po tej dacie **nie zostało sprawdzone**.

### Uruchomienie i wdrożenie

Otwórz `index.html` w nowoczesnej przeglądarce (wymagany internet). Wdrożenie: repozytorium **publiczne** → *Settings → Pages → Deploy from a branch → `main` / `(root)`*.
