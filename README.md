# Bio-Sniff: QAOA-Based Quantum Optimization for Agricultural Sensor Placement

**Project Category:** Quantum Computing / Quantum Optimization  
**Application Domain:** Smart Agriculture and Early Crop-Disease Monitoring  
**Primary Method:** Quantum Approximate Optimization Algorithm (QAOA)  
**Optimization Formulation:** Quadratic Unconstrained Binary Optimization (QUBO)  
**Benchmarking:** Processor A, Processor B, and Exact Classical Optimization

---

## 1. Project Overview

Bio-Sniff is an agricultural sensing concept designed to investigate the early detection of potential crop-disease-related volatile organic compound (VOC) patterns using gas-sensing technology.

This repository explores a quantum optimization approach for selecting sensor locations within a predefined agricultural field layout. The objective is to select exactly three sensing locations from nine candidate positions while balancing sensing utility, complementary information, and spatial distribution.

The optimization problem is formulated as a binary quadratic objective and investigated using the Quantum Approximate Optimization Algorithm (QAOA). The quantum optimization results are evaluated against an exact classical reference and benchmarked using two processor connectivity configurations under documented circuit and noise assumptions.

The current repository contains a synthetic architecture benchmark and a simulated sensor-dataset audit workflow. It represents an optimization and benchmarking prototype rather than a validated physical field-deployment system.

## 2. Problem Statement

Deploying a limited number of sensors across an agricultural field requires selecting locations that provide useful sensing coverage without unnecessary spatial redundancy.

For the current prototype, nine candidate locations are considered, and exactly three must be selected.

The optimization problem is defined as:

- **Input:** Nine candidate locations and their associated utility and pairwise scoring values.
- **Decision:** Select exactly three locations.
- **Objective:** Maximize the combined sensing utility, complementarity, and spatial distribution score.
- **Quantum approach:** Encode the binary optimization problem as a QUBO and investigate it using QAOA.
- **Classical reference:** Evaluate all valid three-location combinations to determine the exact optimum for the defined objective.

The complete nine-candidate problem contains

$$\binom{9}{3}=84$$

valid three-location combinations, making exhaustive classical verification feasible.

## 3. Optimization Formulation

Let $x_i$ represent whether candidate location $i$ is selected:

$$x_i\in\{0,1\},\qquad i=1,\ldots,9$$

The cardinality constraint is:

$$\sum_{i=1}^{9}x_i=3$$

The prototype uses a weighted objective of the form:

$$\max_x F(x)=0.60U(x)+0.25C(x)+0.15S(x)$$

where:

- $U(x)$ represents normalized candidate sensing utility.
- $C(x)$ represents pairwise complementarity.
- $S(x)$ represents spatial distribution quality.

The weights are experimental design choices and should be assessed through sensitivity analysis.

The constrained maximization problem is converted into a QUBO minimization problem using a cardinality penalty:

$$E(x)=-F(x)+P\left(\sum_i x_i-3\right)^2$$

where $P$ penalizes solutions that do not select exactly three locations.

The penalty strength must be sufficient to enforce the selection constraint without overwhelming the useful objective terms. Feasible QAOA samples are checked before solution quality is evaluated.

**Data qualification:** The currently reported benchmark uses synthetic placement inputs. The objective should not be interpreted as a score derived from measured agricultural-field performance.

## 4. Methodology

### 4.1 Problem construction

The prototype defines candidate locations, a weighted placement objective, and a cardinality constraint. The resulting binary objective is encoded as a QUBO.

### 4.2 Quantum optimization

QAOA is used to explore candidate solutions. The current benchmark uses a depth parameter of $p=1$, a COBYLA classical optimizer, and parameterized cost and mixer circuits.

The optimization is evaluated using the documented shot count, random seed, and processor configuration.

### 4.3 Classical verification

An exact classical search evaluates every valid three-of-nine combination for the full problem. The resulting optimum provides a reference for evaluating the quality of QAOA solutions.

### 4.4 Processor benchmarking

The benchmark compares two processor connectivity configurations under documented gate, noise, and transpilation assumptions.

The evaluation considers solution quality, feasible-sample frequency, optimal-solution probability, circuit depth, CX-gate count, and routing-related metrics.

The five-candidate comparison is used to compare the two processor configurations on the same smaller problem. The full nine-candidate problem is evaluated using Processor B.

### 4.5 Dataset audit

The repository also includes a workflow for inspecting the simulated Bio-Sniff sensor dataset. This workflow examines metadata, class distributions, sensor-response values, and possible saturation.

The dataset audit and placement benchmark are separate components. The existence of sensor signatures does not by itself establish a validated relationship between sensor readings and optimal physical placement.

## 5. Repository Structure

```text
Bio-Sniff-Quantum-Optimization/
├── README.md
├── requirements.txt
├── notebooks/
│   ├── BioSniff_QAOA_Synthetic_Benchmark.ipynb
│   └── BioSniff_Dataset_Audit.ipynb
├── results/
│   ├── biosniff_processor_A_B_results.csv
│   ├── biosniff_benchmark_config.txt
│   └── biosniff_rice_sensor_audit.csv
└── data/
    └── README.md
```

### Directory descriptions

| Path | Purpose |
|---|---|
| `notebooks/` | Benchmark implementation and dataset-audit workflow |
| `results/` | Exported benchmark results, configuration, and audit outputs |
| `data/` | Dataset documentation, provenance, and instructions for accessing or preparing the data |
| `requirements.txt` | Python dependencies for reproducing the notebooks |

## 6. Processor Configurations

The benchmark uses two connectivity configurations.

### Processor A

- Five physical qubits.
- Coupling edges: $(0,1),(1,2),(1,3),(3,4)$.

### Processor B

- Nine physical qubits.
- Coupling edges: $(0,1),(1,2),(1,6),(2,3),(2,6),(2,5),(3,4),(4,5),(5,6),(6,7),(6,8),(8,0)$.

These are the configurations used in the current prototype. Their topology and resource metrics should be interpreted according to the submitted benchmark configuration, not as claims about access to or execution on actual quantum hardware.

### Documented simulation settings

| Parameter | Setting |
|---|---|
| QAOA depth | $p=1$ |
| Shots | 4,096 |
| Random seed | 42 |
| Classical optimizer | COBYLA |
| Maximum optimizer iterations | 60 per optimization run |
| Single-qubit depolarizing probability | 0.001 |
| CX depolarizing probability | 0.005 |
| Readout bit-flip probability | 0.01 |
| Transpilation optimization level | 1 |
| Basis gates | `rz`, `sx`, `x`, `cx` |

Refer to `results/biosniff_benchmark_config.txt` for the recorded configuration. The values in that file should match the actual notebook implementation used to generate the submitted results.

## 7. Evaluation Metrics

The benchmark should report the following metrics where available:

- Exact classical optimum.
- Best feasible QAOA reward.
- Mean reward among feasible samples.
- Feasible-sample frequency.
- Optimal-solution probability.
- Optimality gap relative to the exact classical optimum.
- Transpiled CX-gate count.
- Circuit depth.
- Routing overhead and explicit SWAP count, with their definitions stated clearly.
- Sensitivity to noise and optimization assumptions.

The optimality gap for a positive maximization objective can be expressed as:

$$\operatorname{Gap}(\%)=100\,\frac{R^*-R}{|R^*|}$$

where $R^*$ is the exact optimum and $R$ is the reward of the solution being evaluated.

The evaluation must distinguish the best sampled solution from the probability of sampling that solution. Finding an optimal bitstring in a small fraction of shots does not imply that most QAOA samples are optimal.

## 8. Installation and Reproducibility

### Requirements

Python 3 and the dependencies specified in `requirements.txt` are required.

### Run in Google Colab

1. Open the benchmark notebook from the repository.
2. Select **Runtime → Run all**.
3. Install the documented dependencies if the notebook does not install them automatically.
4. Allow the notebook to complete the optimization, classical verification, and benchmark calculations.
5. Compare the generated outputs with the exported files in `results/`.

Run `BioSniff_Dataset_Audit.ipynb` separately to inspect the dataset.

For a reproducible submission, preserve the random seeds, dependency versions, configuration file, and output files from the same benchmark run.

## 9. Data Provenance and Limitations

The sensor dataset currently used in this prototype is synthetic and simulation-based. It is not a set of measurements collected from a physical paddy field.

The current placement scenarios and scores are also synthetic. They must not be presented as experimentally measured sensor coverage, real-world disease-detection performance, or verified physical node locations.

The present work does not establish:

- Validated disease-specific VOC concentrations.
- Measured VOC-plume propagation across an actual field.
- A verified optimal placement for a physical one-acre paddy field.
- Successful three-node field validation.
- Quantum advantage over classical optimization.

These limitations define the scope of the current benchmark and the requirements for subsequent experimental validation.

## 10. Future Work

Future development will focus on:

1. Auditing and documenting the generation of the simulated sensor signatures.
2. Establishing a documented sensing model that relates candidate positions, possible source locations, environmental conditions, and expected sensor responses.
3. Generating utility and pairwise complementarity matrices from that model.
4. Evaluating sensitivity to spatial assumptions, objective weights, detection thresholds, and noise.
5. Connecting verified physical sensor measurements to the placement model when available.
6. Validating the recommended configuration using three physical sensing nodes and appropriate field measurements.
7. Extending the processor benchmark with additional metrics and appropriately selected classical baselines.

## 11. Conclusion

This repository presents a reproducible prototype for investigating QAOA-based sensor-placement optimization in the Bio-Sniff agricultural sensing concept.

By combining a binary optimization formulation, processor-aware circuit benchmarking, and exact classical verification, the project establishes a foundation for studying the suitability of quantum optimization methods for a constrained agricultural placement problem.

The results should be interpreted within the documented synthetic-data and simulation assumptions. Further work is required to connect the optimization objective to calibrated sensor responses and validated field conditions.

---

**Project:** Bio-Sniff — Quantum Optimization for Agricultural Sensor Placement  
**Primary implementation:** QAOA and QUBO  
**Benchmark reference:** Exact classical optimization  
**Current validation scope:** Synthetic optimization benchmark and simulated dataset audit
