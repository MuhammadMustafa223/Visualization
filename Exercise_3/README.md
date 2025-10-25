# Problem Sheet 03

Concise summary and per-question explanation for `03-Exercise`

## Purpose

This notebook contains two main exercises:
- Exercise 1.1 — preprocess and visualize precipitation/station data, interpolate hourly precipitation onto a grid and produce 24 hourly maps.
- Exercise 1.2 — simple mesh generation and visualization examples (unit cube, triangular mesh of a disk, cylinder surface) using Plotly.

## Files required

- `zehn_min_rr_Beschreibung_Stationen.txt` — station metadata (parsed with latin1 encoding).
- `10min_processed.csv` — precipitation records (columns include `stationid`, `date`, `rain`).
- `griddata.npz` — precomputed grid arrays (`geolat`, `geolong`, `ind`) for interpolation.

Put these files in the same folder as the notebook or update the paths used in cells.

## Environment / Dependencies

- Python 3.8+
- numpy
- pandas
- matplotlib
- seaborn
- scipy (for griddata)
- plotly

Install with pip if needed:

pip install numpy pandas matplotlib seaborn scipy plotly

## Exercise summaries

### Exercise 1.1 — Station parsing, precipitation aggregation, and gridded interpolation

- Goal: Convert a station description text file to a CSV-like DataFrame, inspect station locations/elevations, aggregate precipitation to hourly sums, and interpolate station rainfall to a geographic grid for each hour of a selected day.

- What the notebook does (step-by-step):
  1. Reads `zehn_min_rr_Beschreibung_Stationen.txt` line-by-line (encoding: latin1), extracts station records with at least 6 tokens, and builds `stations_df` with columns `Stations_id`, `Von_datum`, `Bis_datum`, `Stationshoehe`, `geoBreite`, `geoLaenge` (cast to appropriate numeric types).
  2. Plots station locations: longitude vs latitude, colored by elevation, saves `stations_scatter.png` and displays the figure.
  3. Loads `10min_processed.csv`, parses `date` using format `%Y%m%d%H%M`, computes an hourly floor (`hour`) and replaces missing sentinel values `-999` in `rain` with `0`.
  4. Aggregates precipitation to hourly totals per station (`hourly_precip`) and creates a scatter of total precipitation by `stationid`.
  5. Loads `griddata.npz` to obtain `geolat`, `geolong`, and a boolean mask `ind` describing the domain.
  6. For a specific date (the code iterates hours for `2024-04-20`), it:
     - selects hourly station totals, merges with station coordinates,
     - interpolates station point values onto the 2D grid using `scipy.interpolate.griddata(method='linear')`,
     - applies the mask `ind` to blank out invalid areas,
     - plots an imshow of the interpolated precipitation for each hour and arranges 24 subplots in a 6×4 grid.

- Inputs: station text file, `10min_processed.csv`, `griddata.npz`.
- Outputs: `stations_scatter.png`, 24 imshow maps of interpolated hourly precipitation (displayed in a single figure).

Notes and gotchas:
- The station file is parsed using a whitespace split — if the file format differs (extra spaces or headers), adjust the parser.
- The code uses `encoding='latin1'` for station file reading; keep that if decoding errors occur.
- Date parsing relies on the exact format `%Y%m%d%H%M` — if times look wrong, inspect a sample of `date` strings first.
- The notebook replaces `rain == -999` with `0`. Confirm that `-999` is indeed the missing-value sentinel in your CSV.
- `griddata(..., method='linear', fill_value=0)` is used; linear interpolation will leave NaNs outside convhull — the notebook masks with `ind` and sets masked cells to NaN for plotting.

Suggested minor improvements:
- Save the 24-hour figure to disk (use `plt.savefig(...)`) if you need to include results in reports.
- Add an explicit check for required keys inside `griddata.npz` (`geolat`, `geolong`, `ind`) and for required columns in CSVs.

### Exercise 1.2 — Mesh generation and 3D visualization (Plotly)

- Goal: Demonstrate simple triangular meshes and surfaces using `plotly.graph_objects.Mesh3d`.

- What the notebook does:
  1. Defines eight corner `points` of the unit cube and a set of `triangles` (indices) that triangulate the cube faces; visualizes the triangulated cube surface with Plotly Mesh3d.
  2. Builds a polar grid (radial × angular), converts to Cartesian coordinates, constructs triangle indices per grid cell, and plots a triangular mesh approximation of the unit disk.
  3. Builds a cylindrical grid (height × angle), converts to Cartesian coordinates, triangulates the rectangular grid into triangles, and visualizes the cylinder surface.

- Inputs: no external data required for this exercise — meshes are generated procedurally.
- Outputs: interactive Plotly figures for the cube, disk, and cylinder meshes (displayed inline or in the browser depending on environment).

Notes and gotchas:
- Plotly figures are interactive; in some headless environments you may need to use an offline renderer or save static images (`fig.write_image`) which may require the `kaleido` package.
- Mesh sizes (n_radial, n_angular, n_height, n_angle) control resolution: larger values increase visual fidelity but also CPU/memory usage.

## How to run

1. Ensure the station text file, `10min_processed.csv`, and `griddata.npz` are in the notebook folder.
2. Install dependencies (see above).
3. Open the notebook and run the cells top-to-bottom. The precipitation interpolation section can be computationally heavier — you can first run the station parsing and `hourly_precip` aggregation to sanity-check data.
