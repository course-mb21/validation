# Method Validation

The validation repository evaluates the methods used by applying them to representative benchmark problems and comparing the results with published literature. The validation covers contact, material models, large deformations, and material failure.

## ED32 cohesive DG (3D)

[`cohesive_ed32_3d.ipynb`](cohesive_ed32_3d.ipynb) contains the complete DOLFINx 0.11
solver, reference data, and plots. Run the cells in order with a DOLFINx kernel.
All results are written to `results_cohesive_ed32_3d/` beside the notebook.
The default run exports 100 displacement states to `displacement.xdmf` and
`displacement.h5`, preserving DG jumps with separate vertices per tetrahedron.
Time denotes nominal strain (0.007 through 0.70); displacement is in mm.
Adaptive intermediate states also appear in `load_curve.csv`.

Set `SMOKE_TEST = True` in the configuration cell for a short two-step elastic
check. Its files go to `results_cohesive_ed32_3d/smoke_test/`.
The full fracture simulation is not part of this check.

## ED32 cohesive DG (2D plane stress)

[`cohesive_ed32_2d.ipynb`](cohesive_ed32_2d.ipynb) uses triangular elements with
two DG1 displacement components, DG0 pressure, and DG0 log thickness stretch.
The local thickness equation enforces zero out-of-plane stress; the reference
thickness of 3 mm scales forces and fracture areas. Default mesh size: `h=3.0` mm.
The default run exports 100 displacement states to `results_cohesive_ed32_2d/`.
Set `SMOKE_TEST=True` for two small elastic load steps, also with `h=3.0`, with
results in its `smoke_test/` subdirectory.

Short regressions (from this directory):

```bash
OMP_NUM_THREADS=1 OPENBLAS_NUM_THREADS=1 python -m unittest test_cohesive_ed32_2d -v
```

These check edge geometry, a finite-strain plane-stress patch, irreversible
opening history, cohesive equilibrium and thickness scaling, and 100 XDMF frames
with discontinuous triangle displacements. They do not validate the full fracture curve.
