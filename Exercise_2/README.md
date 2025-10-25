# Problem Sheet 02

Concise summary and per-question explanation for `02-Exercise.ipynb`.

## Purpose

This notebook contains two main exercises demonstrating time-series/functional regression and exploratory analysis with smoothing. It uses Gaussian Process Regression, rolling statistics, LOESS smoothing, and visualizations (including 2D scatter and log-scale plots).

## Files required

- `data_sin.csv` — dataset for Exercise 1.1 (x and y values for a function, likely x*sin(x)).
- `data_astroids.csv` — dataset for Exercise 1.2 (asteroid observations, columns used include `class`, `first_obs`, `H`, `a`, `diameter`).

Place these CSVs in the same folder as the notebook or update the paths in the notebook cells.

## Environment / Dependencies

- Python 3.8+
- numpy
- pandas
- matplotlib
- seaborn
- scikit-learn
- statsmodels

Install with pip if needed:

pip install numpy pandas matplotlib seaborn scikit-learn statsmodels

## Exercise summaries

### Exercise 1.1 — Gaussian Process Regression on a noise-free function

- Goal: Fit a Gaussian Process (GP) to a sampled function (from `data_sin.csv`) and visualize predictions with uncertainty bands, plus compute simple rolling statistics.
- What the notebook does:
  - Loads `data_sin.csv` into `df1` and inspects head/shape.
  - Plots a scatter of `x` vs `y` to visualize the samples.
  - Defines an RBF kernel with a specified length scale and creates a `GaussianProcessRegressor` with multiple optimizer restarts.
  - Fits the GP on X (`x`) and y (`y`), obtains mean prediction and standard deviation using `model.predict(..., return_std=True)`.
  - Plots the observations, the GP mean prediction, and a 95% confidence interval (mean ± 1.96*std).
  - Computes rolling mean and rolling standard deviation (window=5) for `x` and `y`, drops NaNs, and overlays rolling mean and ±1 rolling std dev on a scatter plot.

- Inputs: `data_sin.csv` (columns `x`, `y`)
- Outputs: GP fit plot (mean + 95% CI), scatter with rolling mean and std.

Notes / suggestions:
  - The GP kernel and optimizer settings (n_restarts_optimizer) can be adjusted if fitting is slow or numerical warnings appear.
  - Ensure `X` is 2D when passed to scikit-learn (`X = df1[["x"]]` is correct).

### Exercise 1.2 — Asteroid dataset exploration, quantile grouping, and smoothing

- Goal: Explore relationships in the asteroid dataset (`data_astroids.csv`), visualize how absolute magnitude `H` relates to observation date and semi-major axis `a`, and apply smoothing.
- What the notebook does:
  - Loads `data_astroids.csv` into `df2` and displays the head.
  - Converts `class` to string type and `first_obs` to datetime.
  - Creates a colored scatter plot of `first_obs` vs `H` with color mapping by `a` (semi-major axis) to see how `H` varies over time and with orbital size.
  - Bins `a` into 5 quantile groups using `pd.qcut`, then plots `H` vs `first_obs` separately for each quantile group (different markers/colors) to compare trends across orbital-size groups.
  - Applies LOESS smoothing (via `statsmodels.nonparametric.smoothers_lowess.lowess`) per quantile group to highlight smoothed trends of `H` over time and overlays smoothed curves on scatter plots.
  - Produces a scatter plot of `diameter` vs `H` using a logarithmic x-axis to better inspect relationships across orders of magnitude.

- Inputs: `data_astroids.csv` (expects `first_obs`, `H`, `a`, `diameter`, `class`)
- Outputs: colored scatter (H vs first_obs colored by a), per-quantile scatter + LOESS lines, log-scale diameter vs H scatter.

Notes / gotchas:
  - LOESS requires numeric x values; the notebook sorts by `first_obs` and passes it to `lowess`. Depending on the statsmodels version, converting dates to numeric (e.g., using `.view('int64')` or using ordinal) may be necessary; the code converts results back to datetime for plotting.
  - `pd.qcut` may produce uneven groups if there are many identical `a` values — check for duplicated values or adjust `q`.
  - For large datasets LOESS can be slow; consider subsampling or increasing the `frac` parameter to speed it up.

## How to run

1. Verify `data_sin.csv` and `data_astroids.csv` are present in the notebook folder.
2. Install dependencies (see above).
3. Open `02-Exercise.ipynb` and run cells in order (top-to-bottom). Each exercise cell is self-contained.

## Quick reproduction checklist

- If plots don't appear, ensure Jupyter's plotting backend is enabled (e.g., `%matplotlib inline`).
- If `model.fit` warns or fails, try reducing `n_restarts_optimizer` or adjusting kernel bounds.
- If LOESS plotting raises a date conversion error, explicitly convert `first_obs` to numeric (e.g., `first_obs.astype('int64')`) when calling `lowess`, and convert the smoothed x-values back to datetime for plotting.
