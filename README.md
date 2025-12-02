# Fold-Change Detection (FCD) in Biological Systems

A computational exploration of Fold-Change Detection (FCD) - a fundamental property in biological sensory systems that enables organisms to respond to relative changes in signals rather than absolute values.

## Overview

This project demonstrates how biological systems achieve robustness and adaptability through FCD mechanisms. The code implements four key biological scenarios where FCD provides critical advantages over conventional signal processing.

## Key Concepts

**Fold-Change Detection (FCD)** is a property where a system's response depends on the *ratio* of current to past input values, rather than absolute signal levels. Mathematically:

- **FCD System**: Response = f(u(t) / x(t)) where x(t) adapts to track u(t)
- **Non-FCD System**: Response = f(u(t)) - absolute signal processing

This property enables:
- Scale-invariant responses across orders of magnitude
- Robustness to cell-to-cell variability
- Adaptation to changing baseline conditions
- Contrast detection independent of lighting conditions

## Simulations

### 1. FCD Test: Step Response Comparison

**File**: Cell 1 in `code.ipynb`

Demonstrates the fundamental difference between FCD and non-FCD systems using step inputs.

**Systems Tested**:
- **Linear Integral Feedback**: No FCD - responses scale with absolute input amplitude
- **Nonlinear FCD Feedback**: Full FCD - identical response profiles to fold-equivalent inputs
- **Incoherent Feed-Forward Loop (IFFL)**: Natural FCD implementation in biology

**Key Result**: When inputs change by the same fold (e.g., 1→2 vs 2→4), FCD systems produce identical responses while non-FCD systems show amplitude-dependent behavior.

**Output**: `figure1_fcd_step_response.png`

### 2. Bacterial Chemotaxis: Gradient Search

**File**: Cell 2 in `code.ipynb`

Simulates how bacteria navigate chemical gradients using FCD-based sensing.

**Scenario**: 200 agents searching for an attractant peak (Gaussian centered at x=5) under varying concentration scales (0.1×, 1×, 10×, 100×).

**Mechanisms**:
- **FCD Agents**: Compute gradient as ratio y = C/x_adapt, where x_adapt slowly tracks ambient concentration
- **Non-FCD Agents**: Respond to absolute concentration values
- **Run-and-Tumble**: Lower tumble rate when moving up gradient (y > 1), higher when moving down (y < 1)

**Key Result**: FCD enables successful chemotaxis across 4 orders of magnitude in concentration scale. Non-FCD agents fail at extreme scales due to subthreshold (too weak) or saturation (too strong) responses.

**Output**: 
- `figure2_chemotaxis_trajectories.png` - Sample trajectories
- `figure3_chemotaxis_performance.png` - Success rate metrics

### 3. Cellular Signaling: Noise Robustness

**File**: Cell 3 in `code.ipynb`

Tests how FCD handles cell-to-cell variability (extrinsic noise).

**Scenario**: 100 simulated cells receiving the same signal (step: 1→2) but with different protein expression levels (multiplicative gain noise).

**Noise Levels Tested**: σ = 0.1, 0.2, 0.4, 0.8 (log-normal variability)

**Key Result**: FCD systems normalize out multiplicative gain variations, producing more uniform population responses. The Coefficient of Variation (CV = std/mean) remains lower for FCD across all noise levels.

**Output**:
- `figure4_signaling_traces.png` - Population response traces
- `figure5_signaling_cv.png` - CV comparison across noise levels

### 4. Visual Contrast Detection

**File**: Cell 4 in `code.ipynb`

Models how visual systems detect edges independent of illumination levels.

**Scenario**: A sharp reflectance edge (R: 0.2 → 0.8 at x=0.5) viewed under illuminations spanning 0.01 to 100 (4 orders of magnitude).

**Visual Processing**:
- **FCD System**: y_FCD = G(u/x) where u = I(t)·R(x) and x adapts to ambient light
- **Non-FCD System**: y_NF = H(u) - direct brightness response with fixed dynamic range

**Edge Detection Metric**: D(t) = max_x |∂y/∂x| (maximum spatial gradient)

**Key Result**: 
- FCD maintains constant edge strength D_FCD ≈ constant across all illumination levels
- Non-FCD fails at extremes: D_NF → 0 at low I (subthreshold) and high I (saturation)

**Output**:
- `figure6_visual_contrast_responses.png` - Spatial response profiles
- `figure7_edge_detection_performance.png` - Edge strength metrics
- `figure8_visual_extreme_conditions.png` - Detailed comparison at extreme conditions

## Requirements

```bash
pip install numpy scipy matplotlib
```

Or install from requirements file:
```bash
pip install -r requirements.txt
```

## Usage

Open and run the Jupyter notebook:

```bash
jupyter notebook code.ipynb
```

Run cells sequentially to generate all simulations and figures. Each cell is self-contained and produces publication-ready visualizations.

## Mathematical Framework

### FCD Criterion

A system exhibits FCD if its response to a scaled input u → αu is identical to its response to the original input u, after appropriate time rescaling.

**Formal Definition**: 
- Input: u(t) → αu(t)
- FCD Property: y(t; αu) = y(t; u) for all α > 0

### Implementation Architectures

1. **Nonlinear Feedback**:
   ```
   dx/dt = k·x·(u/x - y₀)
   y = u/x
   ```

2. **Incoheherent Feed-Forward Loop (IFFL)**:
   ```
   dx/dt = u - x
   dy/dt = u/x - y
   ```

Both produce the critical ratio-based readout that enables FCD.

## Biological Relevance

### Examples in Nature:

1. **Bacterial Chemotaxis** (E. coli)
   - CheY-P modulation via methylation adaptation
   - Enables navigation across 10⁵-fold concentration ranges

2. **Visual Adaptation** (Retinal photoreceptors)
   - Rapid gain control in phototransduction cascade
   - Maintains contrast sensitivity from starlight to sunlight (10¹⁰-fold range)

3. **Growth Factor Signaling** (RTK pathways)
   - Receptor downregulation provides gain control
   - Reduces cell-to-cell variability in developmental decisions

4. **Osmotic Stress Response** (HOG pathway in yeast)
   - Fold-change in osmolarity triggers adaptive response
   - Absolute osmolarity determines steady-state, not transient dynamics

## Key Findings

1. **Scale Invariance**: FCD systems respond identically to fold-equivalent stimuli across orders of magnitude
2. **Noise Robustness**: FCD filters out multiplicative noise (e.g., protein expression variability)
3. **Adaptation**: Automatic adjustment to baseline conditions without recalibration
4. **Contrast Detection**: Visual edge detection independent of illumination levels
5. **Efficient Gradient Sensing**: Chemotaxis success maintained across environmental scales

## References

- Adler & Tso (1974) - "Decision-making in bacteria: chemotactic response of Escherichia coli to conflicting stimuli"
- Goentoro et al. (2009) - "The incoherent feedforward loop can provide fold-change detection in gene regulation"
- Shoval et al. (2010) - "Fold-change detection and scalar symmetry of sensory input fields"
- Alon (2019) - "An Introduction to Systems Biology: Design Principles of Biological Circuits"

## Author

Systems Biology Project - Computational Exploration of Fold-Change Detection

## License

This project is for educational and research purposes.

---

## Visualization Guide

### Figure Descriptions:

- **Figure 1**: Side-by-side comparison of linear vs FCD vs IFFL responses to fold-equivalent steps
- **Figure 2**: Chemotaxis trajectories showing agent movement toward attractant peak
- **Figure 3**: Performance metrics (distance to peak, success rate) across concentration scales
- **Figure 4**: Population response variability under gain noise
- **Figure 5**: CV comparison showing FCD robustness to multiplicative noise
- **Figure 6**: Visual response profiles across 5 illumination levels
- **Figure 7**: Edge detection performance metrics
- **Figure 8**: Detailed three-condition comparison (low/normal/high illumination)

All figures are saved at 300 DPI for publication quality.
