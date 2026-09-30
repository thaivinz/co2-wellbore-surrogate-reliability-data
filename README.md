# CO2 Injection Wellbore Surrogate-Reliability Dataset

High-fidelity simulation datasets supporting the manuscript:

> **Reliability-informed screening of CO2 injection wellbore integrity under cold thermal shock with zero-cohesion separable interfaces**, *Geoenergy Science and Engineering* (under review).

The data were generated with the open-source [GEOS](https://github.com/GEOS-DEV/GEOS) multiphysics simulator for a cased wellbore (steel casing, cement sheath, rock) in axisymmetric plane strain, with pre-split, zero-cohesion separable casing-cement and cement-rock interfaces (Lagrange-multiplier contact, Coulomb friction), subjected to a step cooling of the casing inner surface from an initial temperature T0 = 100 °C. Each row is one converged high-fidelity simulation: the input columns are the sampled thermomechanical and geometric parameters, and the output columns are the resulting interface apertures.

## Files

| File | Rows | Description |
|------|------|-------------|
| `dataset1_submit.csv` | 5000 | **Case 1**: fixed casing geometry, 10 thermomechanical inputs varied. |
| `dataset2_submit.csv` | 2000 | **Case 2**: realistic geometry variation, 13 inputs (the 10 of Case 1 plus casing inner radius, steel wall thickness, and cement thermal conductivity). |

Both datasets were generated with a Sobol quasi-random sequence (seed 42). Casing geometry in Case 1 is held fixed at `r_inner = 0.15707 m`, `t_steel = 0.02073 m` (production reference configuration of the manuscript), so these two variables do not appear as columns in `dataset1_submit.csv`.

## Output (target) columns

The screening response is the **mechanical aperture** (normal displacement jump) at the end of the screening window, **t_s = 1e5 s (27.8 h)**, taken as the maximum over the faces of each interface. The aperture at t_s is converged with respect to mesh and time step. The transient maxima over the simulated horizon (1.1e5 s) are provided as supplementary columns. All apertures are in millimetres.

| Column | Paper symbol | Unit | Description |
|--------|-------------|------|-------------|
| `y_cc_mm` | y_cc | mm | Aperture of the **casing-cement** interface at t_s. |
| `y_cr_mm` | y_cr | mm | Aperture of the **cement-rock** interface at t_s. |
| `y_all_mm` | y_all | mm | Composite aperture at t_s, `y_all = max(y_cc, y_cr)`. **Primary surrogate-training and reliability target.** |
| `y_cc_peak_mm` | | mm | Maximum casing-cement aperture over the simulated horizon (supplementary). |
| `y_cr_peak_mm` | | mm | Maximum cement-rock aperture over the simulated horizon (supplementary). |
| `y_all_peak_mm` | | mm | `max(y_cc_peak, y_cr_peak)` (supplementary). |

The casing-cement peak occurs within the first minutes of cooling and is resolved only with a time step of about 10 s or smaller; the peak columns were computed with the production time step of 1000 s and are therefore indicative only. Use `y_*_mm` (at t_s) to reproduce the manuscript.

### Summary of the primary target `y_all` (µm)

| Dataset | Mean | Median | Std. dev. | Range | Cement-rock controls y_all |
|---------|------|--------|-----------|-------|----------------------------|
| Case 1 | 168.3 | 164.6 | 73.3 | 38.7 to 355.4 | 100 % |
| Case 2 | 117.8 | 110.5 | 52.0 | 23.6 to 308.9 | 98.1 % |

## Input columns

Ranges are the Sobol sampling bounds. "Scale" is the sampling scale (`log10` for variables spanning more than an order of magnitude, otherwise `linear`).

### `dataset1_submit.csv`: Case 1 (10 inputs)

| Column | Paper symbol | Unit | Range | Scale |
|--------|-------------|------|-------|-------|
| `DeltaT` | ΔT | K | [-120, -20] | linear |
| `t_cement` | t_cem | m | [0.020, 0.040] | linear |
| `E_cement` | E_cem | Pa | [5.0e9, 2.0e10] | log10 |
| `alpha_cement` | α_cem | 1/K | [8.0e-6, 1.3e-5] | linear |
| `E_rock` | E_rock | Pa | [1.0e10, 4.0e10] | log10 |
| `alpha_rock` | α_rock | 1/K | [5.0e-6, 1.1e-5] | linear |
| `E_steel` | E_st | Pa | [1.9e11, 2.1e11] | linear |
| `alpha_steel` | α_st | 1/K | [1.05e-5, 1.3e-5] | linear |
| `a0_interface` | a0 | m | [1.0e-6, 5.0e-5] | log10 |
| `k_contact` | k_c | W/(m·K) | [0.5, 3.0] | log10 |

### `dataset2_submit.csv`: Case 2 (13 inputs)

| Column | Paper symbol | Unit | Range | Scale |
|--------|-------------|------|-------|-------|
| `DeltaT` | ΔT | K | [-80, -20] | linear |
| `t_cement` | t_cem | m | [0.015, 0.060] | linear |
| `E_cement` | E_cem | Pa | [3.0e9, 2.5e10] | log10 |
| `alpha_cement` | α_cem | 1/K | [7.0e-6, 1.5e-5] | linear |
| `E_rock` | E_rock | Pa | [5.0e9, 6.0e10] | log10 |
| `alpha_rock` | α_rock | 1/K | [4.0e-6, 1.3e-5] | linear |
| `E_steel` | E_st | Pa | [1.9e11, 2.1e11] | linear |
| `alpha_steel` | α_st | 1/K | [1.0e-5, 1.4e-5] | linear |
| `a0_interface` | a0 | m | [1.0e-6, 1.0e-4] | log10 |
| `k_contact` | k_c | W/(m·K) | [0.1, 5.0] | log10 |
| `r_inner` | r_i | m | [0.10, 0.20] | linear |
| `t_steel` | t_st | m | [0.012, 0.025] | linear |
| `k_cement` | k_cem | W/(m·K) | [0.5, 2.0] | log10 |

## Notes

- Units are SI; apertures are in mm.
- `DeltaT` is the step change of the casing inner-surface temperature relative to T0 = 100 °C, i.e. the imposed inner temperature is T0 + ΔT.
- `a0_interface` is the initial interface aperture used to initialise the contact fields before thermal loading.
- `k_contact` is the interface thermal conductivity; `k_cement` (Case 2) is the bulk cement thermal conductivity.
- The simulations are unconfined (no in-situ stress or casing pressure). The apertures are therefore upper-bound screening responses for well sections with negligible effective confinement; see the manuscript for the effect of confinement.
- All released rows are converged simulations that passed the consistency check described in the manuscript.

## Related resources

Interactive web application using the production surrogate trained on `dataset2_submit.csv`: <https://www.co2wellbore.io.vn/> (source: <https://github.com/thaivinz/co2-wellbore-webapp>).

## Citation

If you use this dataset, please cite the manuscript above. (Full citation to be updated upon publication.)

## License

The dataset is released under the [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.

## Contact

Vin Nguyen-Thai, vin.nguyenthai@vlu.edu.vn
Laboratory for Computational Mechanics, Institute for Computational Science and Artificial Intelligence, Van Lang University, Ho Chi Minh City, Vietnam.
