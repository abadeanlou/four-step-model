# Four-Step Travel Demand Model for Milan

The classical four-step model — **trip generation → trip distribution → mode
choice → trip assignment** — implemented end-to-end on **real, open data for
Milan**, and pushed past the textbook baseline: real GTFS transit times, user
equilibrium via MSA, and a policy scenario. Pure Python (OSMnx + GeoPandas +
SciPy + NetworkX), no modelling software, no bundled datasets — everything
downloads at run time.

**[→ Read the notebook](notebooks/four_step_model_milan.ipynb)** · 
**[→ interactive maps live at abadeanlou.com/four-step](https://abadeanlou.com/four-step/)**

## What's inside

| Stage | Method |
|---|---|
| Network | OSM major roads, largest strongly connected component, assumed speeds/capacities per class, compressed to SciPy sparse form for fast repeated assignment |
| Zones & land use | ~1.2 km hex grid; population ∝ OSM residential area, jobs ∝ OSM workplace features, scaled to stated control totals |
| 1. Generation | Production/attraction rates, attraction balancing |
| 2. Distribution | Doubly-constrained gravity model, exponential deterrence, Furness/IPF |
| 3. Mode choice | Binary logit on **real scheduled transit times** — a frequency-based transit graph built from Milan's official GTFS feed (boarding waits from AM-peak headways, scheduled in-vehicle times, walking transfers) |
| 4. Assignment | **User equilibrium via the Method of Successive Averages** with BPR volume-delay, relative-gap convergence plot, v/c comparison against all-or-nothing |
| Scenario | **Through-traffic ban in a 2.5 km inner cordon** (an Area-C-style measure): two-class equilibrium assignment, difference map of where banned trips relocate |
| Extras | Select-link analysis at equilibrium, car + PT skim matrices, GeoJSON exports, folium interactive maps |

## Data sources

- Road network, boundary, land-use proxies: **OpenStreetMap** (via OSMnx)
- Public transport: the official **GTFS feed** published on the
  [Comune di Milano open-data portal](https://dati.comune.milano.it/dataset/ds929-orari-del-trasporto-pubblico-locale-nel-comune-di-milano-in-formato-gtfs)
  (produced by AMAT from ATM scheduled service data)

## Honesty notes

- Trip rates, deterrence and logit parameters, BPR coefficients, and the
  population/jobs control totals are **stated assumptions**, not values
  calibrated to Milanese surveys or counts. The notebook flags every one.
- The transit graph is frequency-based (headway/2 boarding waits), not a
  timetable router; the closing section lists what a production model adds.
- I have applied this same workflow to a real metropolitan planning network
  (~58,000 links with 2030/2040 scenario attributes); that dataset is not
  distributable, hence this open-data rebuild.

## Run it

```bash
python -m venv .venv && .venv/Scripts/activate   # or source .venv/bin/activate
pip install -r requirements.txt
jupyter lab notebooks/four_step_model_milan.ipynb
```

First run downloads the Milan network, OSM features, and the ~26 MB GTFS feed
(cached afterwards). Full execution takes several minutes.

## Related work

- [routing-engine-osmnx](https://github.com/abadeanlou/routing-engine-osmnx) —
  FastAPI shortest-path service on OSM networks,
  [live demo](https://abadeanlou.com/routing-engine/)
- [Accessibility-using-Transit](https://github.com/abadeanlou/Accessibility-using-Transit) —
  GTFS transit accessibility for seven European cities,
  [live maps](https://abadeanlou.com/accessibility/)
- [bikeflow dashboard](https://abadeanlou.com/bikeflow/) — 24/7 forecasting
  pipeline on live bike-share data
