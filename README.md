# CO2 Injection Wellbore Surrogate-Reliability Dataset

High-fidelity simulation datasets supporting the manuscript:

> **Reliability-informed screening of CO2 injection wellbore integrity under cold thermal shock and imperfect interfacial bonding**, submitted to *Geoenergy Science and Engineering*.

The data were generated with the open-source [GEOS](https://github.com/GEOS-DEV/GEOS) multiphysics simulator for a cased thermo-elastic wellbore with imperfect (separable) casing-cement and cement-rock contact interfaces under cold CO2 injection. Each row is one fully converged high-fidelity simulation: the input columns are the sampled thermomechanical and geometric parameters, and the output columns are the resulting maximum normal interface-opening apertures used as surrogate-model training targets.

## Files

| File | Rows | Description |
|------|------|-------------|
| `dataset1_submit.csv` | 5000 | **Case 1** — fixed casing geometry, 10 thermomechanical inputs varied. |
| `dataset2_submit.csv` | 2000 | **Case 2** — realistic geometry variation, 13 inputs (the 10 of Case 1 plus casing inner radius, steel wall thickness, and cement thermal conductivity). |

Both datasets were generated with a Sobol quasi-random sequence (seed 42). Casing geometry in Case 1 is held fixed at `r_inner = 0.15707 m`, `t_steel = 0.02073 m` (a representative production-well configuration matching the GEOS cased thermo-elastic benchmark), so these two variables do not appear as columns in `dataset1_submit.csv`.

## Output (target) columns

All three outputs are the **maximum normal displacement jump (mechanical aperture)** observed at the interface over the simulation horizon (t_final = 1.1e5 s), reported in millimetres. The mechanical aperture is the geometric opening and is an upper bound on the leakage-relevant hydraulic aperture.

| Column | Paper symbol | Unit | Description |
|--------|-------------|------|-------------|
| `y_cc_max_jump_mm` | y_cc | mm | Max normal displacement jump at the **casing-cement** interface. |
| `y_cr_max_jump_mm` | y_cr | mm | Max normal displacement jump at the **cement-rock** interface. |
| `y_all_max_jump_mm` | y_all | mm | Composite worst-case aperture, `y_all = max(y_cc, y_cr)`. **Primary surrogate-training target.** |

## Input columns

Ranges below are the observed min/max in the released data. "Scale" is the sampling scale (`log10` for variables spanning more than an order of magnitude, otherwise `linear`).

### `dataset1_submit.csv` — Case 1 (10 inputs)

| Column | Paper symbol | Unit | Range (observed) | Scale |
|--------|-------------|------|------------------|-------|
| `DeltaT` | ΔT | °C | [-120, -20] | linear |
| `t_cement` | t_cem | m | [0.020, 0.040] | linear |
| `E_cement` | E_cem | Pa | [5.0e9, 2.0e10] | log10 |
| `alpha_cement` | α_cem | 1/K | [8.0e-6, 1.3e-5] | linear |
| `E_rock` | E_rock | Pa | [1.0e10, 4.0e10] | log10 |
| `alpha_rock` | α_rock | 1/K | [5.0e-6, 1.1e-5] | linear |
| `E_steel` | E_st | Pa | [1.9e11, 2.1e11] | linear |
| `alpha_steel` | α_st | 1/K | [1.05e-5, 1.3e-5] | linear |
| `a0_interface` | a0 | m | [1.0e-6, 5.0e-5] | log10 |
| `k_contact` | k_c | W/(m·K) | [0.5, 3.0] | log10 |

### `dataset2_submit.csv` — Case 2 (13 inputs)

| Column | Paper symbol | Unit | Range (observed) | Scale |
|--------|-------------|------|------------------|-------|
| `DeltaT` | ΔT | °C | [-80, -20] | linear |
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

- Units are SI except temperature drop `DeltaT` (°C, equivalently K for a difference) and the aperture outputs (mm).
- `DeltaT` is the casing inner-surface temperature drop relative to the initial reservoir temperature.
- `a0_interface` is the initial (as-built) interface aperture before thermal loading.
- `k_contact` is the effective interface thermal conductivity; `k_cement` (Case 2) is the bulk cement thermal conductivity.
- All released rows are fully converged simulations that passed the consistency check described in the manuscript.

## Citation

If you use this dataset, please cite the manuscript above. (Full citation to be updated upon publication.)

## License

The dataset is released under the [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) license.

## Contact

Vin Nguyen-Thai — vin.nguyenthai@vlu.edu.vn
Laboratory for Computational Mechanics, Institute for Computational Science and Artificial Intelligence, Van Lang University, Ho Chi Minh City, Vietnam.
