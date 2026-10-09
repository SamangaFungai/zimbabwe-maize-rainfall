# Rainfall, Temperature and Maize Yield in Zimbabwe (1961-2024)

A data cleaning, visualisation and analysis project that asks how much of the year-to-year variation in Zimbabwe's national maize yield is explained by seasonal rainfall and temperature, and whether yields have fallen for the same weather in recent decades.

## Key findings

- **Rainfall matters.** Each extra 100 mm of October-March rainfall is associated with about **150 kg/ha** more national maize yield (main model: +1.53 kg/ha per mm, 95% CI 0.97-2.09). The estimate stays between 1.28 and 1.87 kg/ha per mm across break-year choices from 1985 to 2010.
- **Yields are lower for the same weather after 2000.** The post-2000 period sits about **380 kg/ha** below earlier years at the same rainfall and temperature (95% CI -598 to -168). The fit is actually best if the break is placed in the early-to-mid 1990s (cutoff 1994, adjusted R2 0.623), so the decline appears to start before 2000.
- **Rainfall alone is not enough.** It explains about 27% of yield variation (adjusted R2 0.27). Adding a time trend lifts this to 0.49, and a step change after 2000 with temperature gives 0.565.
- **Drought years cost roughly 40% of yield.** The driest 20% of seasons yield about 900 vs 1,550 kg/ha in 1961-2000, and about 525 vs 925 kg/ha in 2001-2024.
- **Warmer seasons go with lower yields** (about -190 kg/ha per deg C), but the evidence is modest and the interval is wide (-366 to -16).
- **Years the weather does not explain** include 2008 (actual 287 kg/ha vs about 1,030 predicted) and 1963. Adding a 2008 dummy leaves the rainfall effect unchanged (1.53 kg/ha per mm, 95% CI 0.97-2.09), strengthens the temperature estimate slightly (-219 kg/ha per deg C, p = 0.009), and shrinks the post-2000 shift from about -383 to -329 kg/ha (95% CI -521 to -138), so the main findings do not depend on 2008.

## Data

| Source | Content |
|---|---|
| FAOSTAT, Crops and livestock products | Zimbabwe maize (corn): area harvested, yield and production, 1961-2024 (64 years x 3 elements = 192 rows) |
| World Bank Climate Change Knowledge Portal, CRU 0.5-degree | Zimbabwe national mean monthly precipitation (mm) and surface air temperature (deg C), 1901-2025 |

FAOSTAT quality flags are kept: 165 official, 16 estimated and 11 external-organisation figures. Unofficial figures cover area for 1966-69 and 2009, and production for 1967, 1969 and 2009.

## Method

1. **Clean the maize data.** Fix the byte-order mark in the first column name, check for missing values, drop constant columns, and pivot from long to wide (one row per year). Check that yield equals production / area x 1000.
2. **Reshape the climate data.** Each sheet is one row of 1,500 monthly columns, so it is melted to a monthly table.
3. **Align seasons with harvest years.** October-December belong to the following year's harvest (the 1991/92 season is labelled 1992). The growing season is October-March, and only complete six-month seasons are kept. Dec-Feb rainfall is also computed as a mid-season measure.
4. **Merge** on harvest year (64 rows, no missing rainfall).
5. **Model** yield with OLS regression on seasonal rainfall, seasonal temperature and a time term (linear trend or a post-2000 step).
6. **Check robustness.** Drop 2008 and unofficial years, add a 2008 dummy, use autocorrelation-robust (HAC) standard errors, and sweep the break year from 1985 to 2010.

## Figures

| File | Shows |
|---|---|
| `figures/01_timeseries.png` | Yield, actual vs model, above seasonal rainfall |
| `figures/02_scatter_periods.png` | Rainfall vs yield before and after 2000 |
| `figures/03_dry_vs_other.png` | Mean yield in dry vs other seasons, by period |
| `figures/04_residuals.png` | Years the model cannot explain |
| `figures/05_cutoff_sensitivity.png` | Model fit and rainfall effect by break year |

## Limitations

- National averages hide regional differences.
- Seasonal totals miss the timing of dry spells within the season.
- The data is observational, so results show association, not causation.
- Early FAOSTAT values are estimates.
- The break year was chosen with hindsight, so p-values for the best-fitting cutoff are optimistic. The post-2000 step bundles everything that changed after the cutoff (policy, economy, inputs) and is described as a shift, not attributed to a cause.

## How to run

1. Clone the repo, or download it and open the project folder in Jupyter.
2. Install the requirements: `pip install pandas numpy matplotlib statsmodels openpyxl`
3. Check that the two raw files are in `data/raw/`:
   - `FAOSTAT_data_en_10-8-2026.csv`
   - `zim_climate_cru.xlsx`

   If they are missing, download them from the sources above and save them there under exactly these names.
4. Open `maize_rainfall_analysis.ipynb` in Jupyter and choose Kernel > Restart Kernel and Run All Cells. Processed data is written to `data/processed/` and figures to `figures/`.

## Tools

Python, pandas, NumPy, statsmodels, matplotlib, Jupyter.

## Author

Fungai Samanga, BSc Honours Data Science and Systems, University of Zimbabwe
[GitHub](https://github.com/SamangaFungai) | [LinkedIn](https://www.linkedin.com/in/fungai-samanga-65bb91370/) | fungaisamanga09@gmail.com
