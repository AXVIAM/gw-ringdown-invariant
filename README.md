# A New Invariant in Gravitational-Wave Ringdown Dynamics
Code accompanying the analysis presented in the manuscript  
**"A New Invariant in Gravitational-Wave Ringdown Dynamics"**  
by Christopher P. B. Smolen (AXVIAM).

This repository contains the complete Python pipeline used to extract a simple, dimensionless invariant from the ringdown phase of binary black-hole mergers using publicly available LIGO strain data.

## Overview

This project implements a fully model-independent method to identify a stable quantity in the post-merger relaxation of black holes. The method requires no templates, no parameter estimation, and no theoretical modeling. It uses only GWpy, GWOSC, and basic numerical operations.

The pipeline:

1. Fetches LIGO strain around a gravitational-wave event.
2. Whitens and bandpasses the signal.
3. Crops to a short window around merger.
4. Computes numerical derivatives dh/dt and d2h/dt2.
5. Forms the ratio  
   Xi(t) = |dh/dt| / (|d2h/dt2| + eps).
6. Smooths Xi(t).
7. Identifies a stable plateau shortly after the merger peak.
8. Estimates the ringdown frequency from zero-crossings.
9. Constructs the dimensionless invariant  
   Xi_tilde = 2*pi*f_rd * Xi_plateau.
10. Performs an off-source noise test.
11. Saves a plot and appends the results to a CSV file.

## Repository Contents

```
ligo_fixed_point_gwpy.py        Main analysis script
make_figures.py                 Optional script for paper figures
ligo_fixed_point_results.csv    Results are appended here automatically
README.md                       Project documentation
plot_<event>_<detector>.png     Generated plots
```

## Installation

Clone the repository:

```
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>
```

Install required dependencies:

```
pip install numpy matplotlib gwpy gwosc
```

Python 3.9 or newer is recommended.

## Running the Analysis

Example:

```
python ligo_fixed_point_gwpy.py --event GW150914 --detector H1 --whiten
```

Additional examples:

```
python ligo_fixed_point_gwpy.py --event GW170104 --detector L1 --whiten
python ligo_fixed_point_gwpy.py --event GW170814 --detector H1
python ligo_fixed_point_gwpy.py --event GW170823 --detector H1 --fmin 20 --fmax 500
```

## Command-Line Options

| Argument | Description |
|----------|-------------|
| --event | GWOSC event name (e.g., GW150914) |
| --detector | L1, H1, or V1 |
| --fs | Sampling rate for data (default 4096) |
| --window | Length of cropped window (seconds) |
| --whiten | Apply whitening before bandpass |
| --fmin, --fmax | Bandpass limits |
| --smooth-samples | Smoothing kernel length |
| --ringdown-start, --ringdown-end | Post-peak ringdown window |

Example with custom parameters:

```
python ligo_fixed_point_gwpy.py --event GW170608 --detector H1 \
    --whiten --fmin 25 --fmax 350 --smooth-samples 201
```

## Outputs

### Plots
Each run generates:

```
plot_<event>_<detector>.png
```

This figure shows:

- The filtered strain
- The derivative-ratio Xi(t)
- The ringdown window
- The plateau mean

### CSV Results
All runs append a row to:

```
ligo_fixed_point_results.csv
```

Columns include:

- event  
- detector  
- fs  
- window  
- whiten  
- fmin, fmax  
- smoothing parameters  
- ringdown start and end  
- t_peak  
- Xi_mean  
- Xi_std  
- dimensionless invariant Xi_tilde

## Noise Validation

A built-in off-source check ensures that the plateau is not an artifact of noise. The script automatically:

1. Fetches a noise segment before the event.
2. Applies identical whitening and filtering.
3. Computes Xi(t) in the noise window.
4. Tests for artificial plateau-like behavior.

The output reports:

```
[NOISE OK] No plateau detected in off-source noise.
```

or, rarely:

```
[NOISE WARNING] Off-source data shows partial plateau-like behavior.
```

## Reproducing the Paper Figures

If `make_figures.py` is included, run:

```
python make_figures.py
```

This generates:

```
fig1.pdf    Example strain and Xi(t)
fig2.pdf    Scatter of dimensionless Xi across events
```

## Citation

If this code is used in research, please cite:

Christopher P. B. Smolen,  
"A New Invariant in Gravitational-Wave Ringdown Dynamics"  

Also cite LOSC and GWpy:

- I. M. Pinto and A. Lundgren, SoftwareX 15, 100722 (2021).  
- LIGO Open Science Center, https://losc.ligo.org

## Contact

For questions or collaboration:

Christopher P. B. Smolen  
AXVIAM  
christopher.smolen@axviam.com
