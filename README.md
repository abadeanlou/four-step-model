# Four-Step Travel Demand Model for Milan

The classical four-step model — **trip generation → trip distribution → mode
choice → trip assignment** — implemented end-to-end on **real, open data for
Milan**: the actual major-road network and municipal boundary from
OpenStreetMap, a hexagonal zone system, and land use proxied from OSM
residential areas and workplace features. Pure Python (OSMnx + GeoPandas +
NetworkX + NumPy), no modelling software, no bundled datasets — everything
downloads at run time.

**[→ Read the notebook](notebooks/four_step_model_milan.ipynb)** — every
output is embedded, including the assignment and select-link maps on Milan's
real street geometry.

## What's inside

| Stage | Method |
|---|---|
| Network | OSM major roads (motorway → tertiary), largest strongly connected component, assumed speeds/capacities per road class |
| Zones | ~1.2 km hexagonal grid clipped to the municipal boundary |
| Land use | Population ∝ OSM `landuse=residential` area; jobs ∝ OSM workplace features — scaled to stated control totals |
| 1. Generation | Production/attraction rates, attraction balancing |
| 2. Distribution | Doubly-constrained gravity model, exponential deterrence, Furness/IPF |
| 3. Mode choice | Binary logit, car vs public transport |
| 4. Assignment | All-or-nothing shortest-path loading, v/c ratios by road class |
| Extras | **Select-link analysis**, skim-matrix + GeoJSON exports |

## Honesty notes

- Trip rates, deterrence and logit parameters, and the population/jobs control
  totals are **stated assumptions**, not values calibrated to Milanese surveys
  or counts. The notebook flags every one of them; swapping in census data and
  calibrated parameters is the first upgrade of a real study.
- Assignment is all-or-nothing; the closing section describes the equilibrium
  extension a production model uses.
- I have applied this same workflow to a real metropolitan planning network
  (~58,000 links with 2030/2040 scenario attributes); that dataset is not
  distributable, hence this open-data rebuild.

## Run it

```bash
python -m venv .venv && .venv/Scripts/activate   # or source .venv/bin/activate
pip install -r requirements.txt
jupyter lab notebooks/four_step_model_milan.ipynb
```

First run downloads the Milan network and features from OSM (a few minutes,
cached afterwards).

## Related work

- [routing-engine-osmnx](https://github.com/abadeanlou/routing-engine-osmnx) —
  FastAPI shortest-path service on OSM networks,
  [live demo](https://abadeanlou.com/routing-engine/)
- [Accessibility-using-Transit](https://github.com/abadeanlou/Accessibility-using-Transit) —
  GTFS transit accessibility for seven European cities,
  [live maps](https://abadeanlou.com/accessibility/)
- [bikeflow dashboard](https://abadeanlou.com/bikeflow/) — 24/7 forecasting
  pipeline on live bike-share data
