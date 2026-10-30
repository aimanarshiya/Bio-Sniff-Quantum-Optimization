# Bio-Sniff QFF 2026 Phase 2 — PS-O-OI

Bio-Sniff continues its Phase 1 Open Innovation sensor-placement problem. The locked inputs describe synthetic placements and must not be interpreted as measured field performance. The Phase 2 Bio-Sniff deliverables document controls over the generic PS-O1/O2/O3 instances in `data/instances.json`.

## Environment and Run

Use Python 3.10–3.12; this repository was verified with Python 3.12.10. Create and activate an environment, then install the pinned direct dependencies:

```powershell
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

Open `main.ipynb`, select the installed Python 3.12 kernel, and run all cells from a clean kernel. Cell 1 checks the environment and official helpers. Cell 2 runs the end-to-end Phase 2 pipeline with checkpointed optimization and measurement caches. Compatible completed work is reused; missing or invalid cache records are recomputed. The optimizer status, including failed attempts and iteration limits, is preserved in the per-seed outputs.

The official organizer helper `qff_phase2.py` is unchanged. The locked `data/instance.json` is the Bio-Sniff input; `data/instances.json` remains the generic starter file and is not used for PS-O-OI.

## Current Measured Summary

The values below are generated from the current result tables. They describe synthetic simulation only.

| Case | Noisy objective ratio | Noisy valid frequency | Random objective ratio | Random valid frequency |
| --- | ---: | ---: | ---: | ---: |
| L on B | 0.7255 | 0.2793 | 0.8157 | 0.1641 |
| L on D | 0.7786 | 0.2891 | 0.8157 | 0.1641 |
| S on A | 0.9094 | 0.5068 | 0.8829 | 0.3125 |
| S on B | 0.9865 | 0.5188 | 0.8829 | 0.3125 |
| S on D | 0.8959 | 0.5118 | 0.8829 | 0.3125 |

For Instance L, the 60-evaluation Step 1A optimizer status was unsuccessful for four of five seeds. The Phase 2 150-evaluation retry reached a successful COBYLA termination for all five L seeds; both attempts and their statuses are recorded. The result above remains a simulated synthetic benchmark, not quantum advantage.

The required two-pair D Bell experiment measured ideal fidelity 1.0000 for both pairs, noisy fidelity 0.9633 for adjacent pair `[0,1]`, and 0.9548 for farthest pair `[0,3]` (graph distance 2). In the noisy L penalty sweep, P=0 produced an invalid QUBO minimum; among tested penalties, P=5 had the highest measured valid-frequency × objective-ratio product. This product is a descriptive comparison, not an official score.

## Outputs

- `results/tables/results.csv`: required per-instance, processor, condition, quality, and resource schema.
- `results/tables/bell.csv`: ideal/noisy Bell correlations, standard errors, fidelity, and CX count.
- `results/tables/penalty_sweep.csv`: L-on-D sweep at P = 0, 0.5, 1, 2, 3, 5, 10.
- `results/tables/seed_runs.csv` and `seed_consistency.csv`: per-seed optimization status, sampled results, and ratio variability.
- `results/optimization/`: smoke test, optimized angles, resource metrics, and resumable experiment checkpoint cache.
- `results/figures/`: Processor D topology, logical/transpiled QAOA circuits, benchmark comparison, and penalty sweep.
- `report.md`: analysis generated from the result tables and checkpoints.

Processor A is correctly marked non-executable for L (nine logical qubits required, five available). Processor A/B/D graphs, official noise rates, 4096-shot evaluation, transpilation level 1, seed 42, and basis gates are taken from the official specification/helper.

## Experimental Analysis and Implementation

We conducted a series of computational experiments to evaluate the proposed Bio-Sniff quantum optimization approach for intelligent sensor placement. The experiments cover QAOA-based optimization, processor topology evaluation, circuit resource analysis, Bell-state verification, penalty sensitivity, and benchmark performance across different configurations.

The implementation, experiment workflows, generated outputs, and supporting analysis are available in this repository. The complete experimental implementation and additional supporting materials are also available here:

**Detailed Implementation and Experimental Materials:** [Insert Google Drive Link]

### Classical and Quantum Optimization Comparison

An important part of our evaluation is comparing classical optimization approaches with quantum optimization using a common problem formulation and evaluation criteria.

The comparison focuses on:
- Solution quality and objective values.
- Valid sensor-placement configurations.
- Execution time and optimization iterations.
- Circuit depth, gate count, and quantum resource requirements.
- Performance across different candidate configurations.

This comparison helps evaluate the strengths, trade-offs, and practical applicability of quantum optimization against classical approaches.

### Sensor Placement and Source Localization

The framework considers multiple possible sensor-placement configurations and their corresponding coverage, utility, complementarity, and spacing characteristics.

For practical source detection and sensor placement, the approach can be extended to evaluate a larger set of feasible sensor combinations. By systematically exploring these combinations and recording the computation time required, the system can identify suitable configurations while considering optimization quality and computational cost.

This provides a foundation for extending sensor placement toward source localization and more realistic VOC-based detection scenarios.

### IoT Experimental Demonstration

Bio-Sniff is designed around an electronic-nose sensor network for agricultural disease monitoring. The computational optimization work is complemented by the physical IoT experimental setup, which will be demonstrated to the hackathon jury.

The demonstration will connect the optimization concept with the practical sensor-network implementation, illustrating how intelligent sensor placement can support real-world agricultural monitoring.

### Scope and Limitations

- The current computational benchmark uses synthetic candidate grids and simulated coverage, utility, complementarity, and spacing values. These are experimental inputs rather than calibrated field measurements.
- Actual VOC transport, environmental variations, and disease-related sensor responses require further calibration and field-level experimentation.
- Evaluating a larger number of sensor combinations can increase computational requirements, making execution time and scalability important considerations.
- The performance of quantum optimization depends on circuit resources, noise, processor topology, and problem size.

### AI Assistance and Reproducibility

GitHub Copilot and Morph AI IDE along with Gemini 3.6 flash  used to assist with inspecting official specifications, implementing the notebook and pipeline, developing validation code, and summarizing experimental results.

The reported computational results are generated through the repository's experimental pipeline using the official helper and are traceable to the saved CSV files, experiment checkpoints, and analysis outputs.
