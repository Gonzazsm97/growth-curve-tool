# Interactive Growth Curve & Doubling Time Analyzer

## Overview
This R Shiny application allows us to analyze T. cruzi growth data. It automatically generates growth curves, calculates doubling times, and performs statistical comparisons between groups (still working on this). 

Crucially, it includes a **"Pipetting Simulator"** for cumulative density calculations. If you perform daily counts and resets (dilutions) to maintain cells in log phase, the app can take your raw daily counts and back-calculate the cumulative density based on your specific volume and reset targets.

Access: https://gonzazsm97.github.io/growth-curve-tool/

## Data Format (CSV)
Your input file must be a `.csv` with at least the following columns:
1. **Day:** Numeric (0, 1, 2, 3...)
2. **Density/Count:** The cell counter values. 
3. **Replicate:** Identifier for replicates (1, 2, 3...)
4. **Sample Type:** The name of the cell line or condition (e.g., WT, KO, Tet+).

**Example CSV Structure:**
CellLine, Day, Replicate, Count, Heme_Conc
WT,       0,   1,         5.0,   0
WT,       0,   2,         4.8,   5
KO,       1,   1,         10.5,  25
...

## How to Use

### 1. Upload & Map
* Upload your CSV file using the file browser in the sidebar.
* Use the dropdown menus to map your CSV columns to the app's required fields:
* **Day/Time Column**
* **Density Column**
* **Replicate Column**
* **Sample Type 1** (The main grouping variable)
* **Sample Type 2** (A second grouping like Tet treatment or Heme concentration, etc)

### 2. Select Calculation Mode
The app handles two types of data inputs:

#### Option A: Cumulative Density Already Calculated
Select this if your CSV already contains the final, mathematically corrected densities (e.g., numbers reaching 10^8 or 10^9). The app will plot these values directly without modification.

#### Option B: Calculate from Raw Counts (Pipetting Simulator)
Select this if your CSV contains the **raw counts** you obtained on the hemocytometer/counter that day (e.g., oscillating between 5x10^6 and 20x10^6).

* **Required Input Parameters:**
* **Density Format:** * *Full Numbers:* e.g., 25,000,000
* *Abbreviated:* e.g., 25 (representing 25x10^6)
* **Total Volume:** The working volume of media in your flask/plate (e.g., 3 mL).
* **Target Reset Density:** The density you dilute back down to every day (e.g., 5x10^6 cells/mL).

* **The Logic:**
* The app calculates how much volume you *kept* to hit your target density.
* It rounds this volume to **1 decimal place** (simulating real pipetting precision).
* It calculates the dilution factor for that day and applies it cumulatively to future days.

### 3. Visualize & Export
* **Growth Curve Tab:** View the log-scale growth over time. You can toggle log scale, facet by different variables, and change line types.
* **Doubling Time Tab:** View a bar chart of calculated doubling times. The app automatically performs t-tests to compare groups.
* **Calculation Details Tab:** Verify the exact "Volume Kept" and "Fresh Media Added" for every replicate to ensure the simulation matches your lab work.
* **Exports:** Download high-resolution plots (PNG/SVG) or summary tables (CSV/Excel) for your lab notebook or presentations.