# Problem Sheet 01

Concise summary and per-question explanation for `01-Exercise.ipynb`.

## Purpose

This notebook implements three short exercises covering data transformations/visualizations, analyzing algorithm runtimes, and an image hue-rotation demo. The notebook is organized into Exercises 1.1, 1.2, and 1.3. The code uses pandas, seaborn/matplotlib, scikit-learn, and OpenCV.

## Files required

- `mpg-data.csv` — used in Exercise 1.1
- `runtimes.csv` — used in Exercise 1.2
- `BlueAndYellowMacaw_AraArarauna.jpg` — used in Exercise 1.3 (image file)

Make sure these files are in the same folder as the notebook or update paths accordingly.

## Environment / Dependencies

Install the usual Python data stack:

- Python 3.8+
- numpy
- pandas
- matplotlib
- seaborn
- scikit-learn
- opencv-python (cv2)

You can install them with pip, for example:

pip install numpy pandas matplotlib seaborn scikit-learn opencv-python

## Exercise summaries

### Exercise 1.1 — Basic Transformations and Visualizations

- Goal: Inspect an automobile MPG dataset and run simple per-class regressions and aggregations.
- What the notebook does:
  - Loads `mpg-data.csv` into a DataFrame (`df`) and displays shape/head and unique values for `class`.
  - Shows counts of `manufacturer` values to inspect distribution.
  - For each unique car `class`, fits a linear regression predicting engine displacement (`displ`) from highway MPG (`hwy`) using scikit-learn's `LinearRegression` and prints slope and intercept.
  - Creates scatter plots (displacement vs. highway MPG) per car class and overlays a regression line using seaborn for easy visualization.
  - Computes median `hwy` grouped by `class` and `year` and prints this aggregated table.

- Inputs: `mpg-data.csv` (columns used include `class`, `manufacturer`, `displ`, `hwy`, `year`)
- Outputs: printed shapes/head, printed regression coefficients per class, per-class scatter+regression plots, grouped median table.

Notes / assumptions:
  - Regression uses `hwy` as X and `displ` as y in one place and the opposite when plotting; check notebook cells for exact pairing before interpreting coefficients.

### Exercise 1.2 — Algorithm Runtimes

- Goal: Load runtime measurements for different algorithms/sizes/threads and visualize scaling behavior.
- What the notebook does:
  - Reads `runtimes.csv` into a DataFrame (skipping comment lines with `#`).
  - Uses `pd.melt` to reshape the data from wide to long format: the `time1`..`time5` columns are converted into a `Thread` column and a numeric `Runtime` column.
  - Renames thread column labels from `time1..time5` → `Thread_1..Thread_5`, enforces types, and drops missing values.
  - Plots a scatter of problem `size` vs `Runtime` (colored by `Thread`) with a regression overlay to inspect overall trends.
  - For each algorithm (`algo`), creates a line plot of `Runtime` vs `Thread` with `size` as the hue (marker points included). This shows how runtime changes with thread count across problem sizes.

- Inputs: `runtimes.csv` (expected columns: `algo`, `size`, `time1`..`time5`)
- Outputs: reshaped DataFrame `data_melt`, scatter + regression plot, per-algorithm line plots showing runtime vs threads.

Notes:
  - The notebook assumes 5 timing columns (time1..time5); if your data has a different number of runs, update the melt accordingly.

### Exercise 1.3 — Hue Rotation (Image Processing)

- Goal: Demonstrate hue rotation on an RGB image using OpenCV.
- What the notebook does:
  - Loads `BlueAndYellowMacaw_AraArarauna.jpg` using `cv.imread` (BGR by default).
  - Converts the image to RGB for plotting and to HSV for hue manipulation.
  - Defines `rotate_hue(image_rgb, phi)`, which:
    - Converts an RGB image to HSV (OpenCV HSV hue range is [0,180]).
    - Maps a rotation angle phi ∈ [0, 2π) to the [0,180) hue range and adds it to the hue channel with wrap-around.
    - Converts back to RGB and returns the hue-rotated image.
  - Plots five hue-rotated variants corresponding to phi = 0, 2π/5, 4π/5, 6π/5, 8π/5.

- Inputs: the image file
- Outputs: a figure showing the original and four hue-rotated images side-by-side.

Notes / gotchas:
  - OpenCV uses BGR ordering when reading images; notebook converts to RGB before displaying with matplotlib.
  - Hue in OpenCV HSV is scaled 0..180 (half of 0..360), so the code rescales phi accordingly.

## How to run

1. Ensure the required files (`mpg-data.csv`, `runtimes.csv`, image file) are in the same folder as `01-Exercise.ipynb`.
2. Install dependencies (see above).
3. Open and run the notebook cells (order is top-to-bottom). Each exercise is self-contained within its section.

---
