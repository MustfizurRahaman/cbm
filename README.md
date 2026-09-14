# The Boreal Ledger

Forest carbon in Trout Lake Forest (120) and Wabigoon Forest (130), northwestern Ontario, 2000–2010, modelled stand by stand with the Carbon Budget Model of the Canadian Forest Sector (CBM-CFS3).

- **Story page:** https://mustfizurrahaman.github.io/cbm/
- **Notebooks:** [`notebooks/`](notebooks/)

## Workflow

| Notebook | What it does |
|---|---|
| [`01_preprocess_inventory.ipynb`](notebooks/01_preprocess_inventory.ipynb) | Forest Resource Inventory → productive forest stands → the seven Standard Import Tool tables and `mapping.json`, split into three stacks under `runs/<forest>/stack<n>/` |
| [`02_run_cbm_cfs3.ipynb`](notebooks/02_run_cbm_cfs3.ipynb) | Imports each stack and runs Makelist and the CBM-CFS3 simulation; checks every results database holds its own stack |
| [`03_postprocess_results.ipynb`](notebooks/03_postprocess_results.ipynb) | Reads the results databases and writes carbon stocks, the yearly carbon budget, disturbed area, age classes and per-polygon biomass for 2000–2010 to `outputs/<forest>/` |

## Data

Not included. Download each dataset and place it as shown:

| Dataset | Source | Place at |
|---|---|---|
| Forest Resources Inventory, Wabigoon Forest and Trout Lake Forest | [Ontario GeoHub](https://geohub.lio.gov.on.ca/datasets/lio::forest-resources-inventory-fri-packaged-product/about) | Export each forest's 2D polygon layer attribute table to `data/fri/wabigon_2D_db.csv` and `data/fri/troutlake_2D_db.csv` (a `.dbf` or the geodatabase layer also works; see notebook 1) |
| Forest management unit boundaries (maps only) | [Ontario GeoHub](https://geohub.lio.gov.on.ca/datasets/lio::forest-management-unit/about) | `data/fmu/` |
| Operational-Scale CBM-CFS3 toolbox | [Natural Resources Canada](https://natural-resources.canada.ca/climate-change/climate-change-impacts-forests/carbon-budget-model) | Install on Windows |
| cbm3_python | [cat-cfs/cbm3_python](https://github.com/cat-cfs/cbm3_python) | `pip install git+https://github.com/cat-cfs/cbm3_python.git` |

## Run

Windows, with the CBM-CFS3 toolbox and a Microsoft Access Database Engine that matches your Python's bitness.

```bash
pip install git+https://github.com/cat-cfs/cbm3_python.git pyodbc pandas matplotlib nbconvert ipykernel
jupyter nbconvert --to notebook --execute --inplace notebooks/01_preprocess_inventory.ipynb   # ~1 min
jupyter nbconvert --to notebook --execute --inplace notebooks/02_run_cbm_cfs3.ipynb           # tens of minutes per stack
jupyter nbconvert --to notebook --execute --inplace notebooks/03_postprocess_results.ipynb    # ~3 min
```

Set `CBM_RUNS_DIR` to keep the large run folders (500–700 MB per stack) on another drive.

## Notes

- Results stop at 2010: the inventories record harvest and fire up to 2009 (Wabigoon) and 2010 (Trout Lake).
- Carbon is in tonnes of carbon (t C).
- Trout Lake figures on the story page come from one of its three stacks (450,202 of about 828,000 ha of forest), because the earlier run imported its other two stacks from the same tables. Running the notebooks in order simulates all three.
