# Problem Sheet 8 — Particle Flows, Animations, and Density Evolution

Purpose and objectives

This problem sheet explores time-varying particle systems and their visual summaries. The main objective is to connect raw trajectory data to intuitive spatial-temporal visualizations that reveal the structure of particle flows and the evolution of their probability distributions. You will learn how to load structured flow data, plot particle trajectories to inspect individual and collective motion patterns, build frame-by-frame animations that communicate dynamics over time, and compute 2D density estimates (histograms) that show how mass redistributes in space. Emphasis is placed on reproducible visualization choices: selecting plot limits and aspect ratios for meaningful spatial context, choosing marker sizes and alpha blending to avoid overplotting, and using animation parameters (frame interval, blitting) to balance smoothness with performance. From a methodological perspective the sheet teaches two complementary views: Lagrangian (trajectories of particles) and Eulerian (density fields on a grid). These perspectives help interpret physical models, compare alternative coupling/flow strategies (e.g., straight-line coupling vs. curved matching flows), and validate generative or simulation-based models by visually comparing ensembles. By the end of the sheet you should be able to convert a compact numerical dataset (here: `flows.npz`) into publication-ready static figures and interactive animations that make temporal patterns, coherence, and mixing behavior visible to both technical and non-technical audiences.

## Concise per-question explanations

### Visualize Trajectories

- What it does: Loads `flows.npz` which contains time samples `t` and three ensembles of particle positions `xA`, `xB`, `xC` (arrays shaped roughly (nT, nX, 2)). The code iterates over particles and plots their x-y paths over time to produce static trajectory plots for each ensemble.

- Why it matters: Trajectory plots (Lagrangian view) show individual particle paths and make it easy to detect straight-line transport, curvature due to flow fields, collisions, or boundary effects. Comparing `xA`, `xB`, and `xC` side-by-side highlights differences in coupling or flow strategies.

- Key implementation notes: Use a fixed axis range (0–1) and equal aspect to preserve geometric fidelity. Reduce `n_particles` when visual clutter or rendering speed is an issue. Use alpha blending for overlapping trajectories.

### Dynamic Particle Movement Visualization (Animations)

- What it does: Builds an animation using matplotlib's `FuncAnimation`. For each time frame the scatter plot is updated with particle positions, producing an animation that plays through the temporal sequence. The notebook converts the animation to an inline JS/HTML representation (`to_jshtml`) for display in Jupyter.

- Why it matters: Animations reveal temporal ordering and instantaneous patterns (e.g., coherent motion, vortices, transient clustering) that are hard to see in static plots. They also make it simple to compare how different ensembles evolve at the same set of time stamps.

- Key implementation notes: Use `blit=True` where possible for performance. Choose an appropriate `interval` (ms) to control playback speed. For offline rendering, save animations to MP4/GIF with an appropriate writer (FFmpeg for MP4), but be aware of the additional dependency.

### Dynamic Probability Distribution Visualization (Density Evolution)

- What it does: Computes a 2D histogram (density grid) per time frame using `np.histogram2d` and visualizes the resulting density as an image (heatmap) that is animated over time. This is the Eulerian view: instead of following particles, it shows how mass density in spatial bins evolves.

- Why it matters: Density animations are excellent for seeing mixing, spreading, concentration, and transport of mass without tracking individual particles. Useful for model validation, comparing how ensembles mix, and for summarizing large particle counts efficiently.

- Key implementation notes: Choose bin resolution to balance spatial detail with noise (e.g., 50×50 bins). Set consistent colormap limits (`vmin`, `vmax`) across frames for stable color interpretation, or adapt them if you want frame-wise contrast.

Files required and generated
- Required input: `flows.npz` (must be in the same working directory). It should contain arrays named `t`, `xA`, `xB`, `xC` as used in the notebook.
- Generated outputs: Animations are displayed inline in the notebook (JS/HTML). Optionally you can save animations to files (MP4/GIF) using matplotlib writers; the notebook does not save them by default.

Dependencies
- Minimal Python packages (install via pip):

```powershell
pip install numpy matplotlib ipython
```

- Optional for nicer visuals and saving movies: `ffmpeg` (system utility) and `Pillow` (for GIF exports). To install Pillow via pip:

```powershell
pip install pillow
```

How to run
- Open `08-Exercise.ipynb` in Jupyter Notebook or JupyterLab and run cells sequentially.
- Ensure `flows.npz` is present in the same folder.
- To view animations inline, run the animation cells in a browser-enabled Jupyter session. If the inline JS output is too large, consider saving the animation to MP4 and viewing the file externally.

Example: save animation to MP4 (optional)

```powershell
# In a Python cell (not powershell), after creating the FuncAnimation object `ani`:
# ani.save('flow_animation.mp4', writer='ffmpeg', fps=20)
```

Tips & notes
- Reduce `n_particles` when exploratory plotting to speed rendering and avoid overplotting. For final figures, increase particle count if it improves clarity.
- Use consistent color scales for density animations (same `vmin`/`vmax`) to make comparisons meaningful across ensembles.
- If running headlessly (e.g., on a server), configure a non-interactive backend for matplotlib (Agg) and save movies to disk instead of trying to render inline HTML.
