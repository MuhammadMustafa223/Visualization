# Problem Sheet 04 — Data Visualization and Statistical Analysis

## Purpose and Learning Objectives

This notebook explores two fundamental concepts in data science and statistics through visualization and practical implementation. The exercises bridge theoretical understanding with hands-on application, focusing on demographic data visualization and statistical theory verification.

The first part delves into demographic data visualization techniques, specifically examining population dynamics through animated time series and population pyramids. This demonstrates how different visualization approaches can reveal various aspects of the same dataset — temporal evolution through interactive animations and age-gender distribution through symmetric bar charts. These visualization techniques are essential tools in demographic studies, policy planning, and social science research.

The second part provides a practical demonstration of the Central Limit Theorem (CLT), one of the most fundamental concepts in probability theory and statistics. Through simulation and visualization, it shows how the sum of independent random variables approaches a normal distribution as the sample size increases. This hands-on approach helps build intuition about asymptotic behavior in probability theory and the practical implications of the CLT in real-world data analysis.

Together, these exercises demonstrate the power of Python's visualization libraries (Plotly, Matplotlib, Seaborn) in both applied data analysis and theoretical concept verification, while building practical skills in data manipulation, statistical computing, and interactive visualization.

## Files Required

- `population_us.csv` — US population data by age, sex, and year

## Environment / Dependencies

Required Python packages:
```
numpy
pandas
matplotlib
seaborn
plotly.express
scipy.stats (for normal distribution)
```

Install with pip:
```bash
pip install numpy pandas matplotlib seaborn plotly scipy
```

## Exercise Summaries

### Question 4.1 — Evolution of Age Distribution

#### Goal
Analyze and visualize the evolution of US population age distribution over time, including creating an animated visualization and a population pyramid.

#### Implementation Details

1. Data Loading and Preprocessing:
   - Reads `population_us.csv` containing demographic data
   - Groups data by year and age for total population
   - Separates data by gender for pyramid visualization

2. Visualizations Created:
   - Interactive animated line plot (using Plotly Express):
     - x-axis: age
     - y-axis: population count
     - animation: progression through years
     - Shows how the age distribution changes over time
   
   - Population pyramid for 1990:
     - Horizontal bar chart with males (left) and females (right)
     - Age groups on y-axis
     - Population counts on x-axis
     - Different colors for male/female populations

#### Outputs
- Interactive animation of age distribution changes
- Static population pyramid for 1990
- Both visualizations reveal demographic patterns like:
  - Baby booms
  - Aging population trends
  - Gender distribution across age groups

### Question 4.2 — Central Limit Theorem Demonstration

#### Goal
Demonstrate the Central Limit Theorem through simulation using sums of random ±1 variables, showing convergence to normal distribution as sample size increases.

#### Implementation Details

1. Core Implementation:
   - Function `sample_XN(M, N, seed=None)`:
     - Generates M samples of the normalized sum X_N
     - Each X_N is sum of N random ±1 values divided by √N
     - Returns array of M samples

2. Visualizations:
   - Detailed histogram for N=2 case:
     - 10,000 samples
     - Density-normalized
     - Shows early stages of convergence
   
   - Comparison plot for multiple N values:
     - N = [1, 3, 10, 30, 100]
     - Uses kernel density estimation (KDE)
     - Overlays standard normal for comparison
     - Demonstrates progressive convergence

#### Key Observations
- Small N (1-3): Discrete, non-normal distribution
- Medium N (10-30): Approaching normal shape
- Large N (100): Very close to standard normal
- Visualization clearly shows CLT in action

## How to Run

1. Ensure `population_us.csv` is in the notebook directory
2. Install required packages (see Environment section)
3. Open and run cells in order
4. For Question 4.1:
   - Watch the animated plot for temporal patterns
   - Compare pyramid shapes to typical demographic structures
5. For Question 4.2:
   - Try different M values to see sampling effects
   - Experiment with other N values to observe convergence speed

## Notes and Tips

### For Question 4.1
- The animation may be CPU-intensive; reduce the number of years if needed
- Population pyramid can be adapted for other years by changing the `year` variable
- Consider using `fig.write_html()` to save interactive plots

### For Question 4.2
- Larger M gives smoother distributions but slower execution
- The random seed can be set for reproducibility
- KDE bandwidth affects smoothing; adjust if needed



## Additional Resources

- Documentation links:
  - [Plotly Express Animation](https://plotly.com/python/animations/)
  - [Seaborn KDE Plots](https://seaborn.pydata.org/generated/seaborn.kdeplot.html)
  - [Matplotlib Population Pyramids](https://matplotlib.org/stable/gallery/lines_bars_and_markers/horizontal_barchart_distribution.html)

---
