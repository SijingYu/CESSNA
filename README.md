# CESSNA: Community-Enhanced Seeded Network Alignment

This repository contains code and data to reproduce the experiments in our paper *Community-Enhanced Semi-seeded Network Alignment (CESSNA): A
Robust Method with Application to Microbiome Networks on community-enhanced seeded graph matching* and its comparisons to other baselines.

## Repository structure

- `CESSNA_code_and_experiments_table1.ipynb`  
  Implementation of the CESSNA algorithm, helper routines, and experiments corresponding to Table 1 of the paper (block-structured synthetic graphs, Wiki, and C. elegans).

- `Experiments_table2-3_non_CESSNA.ipynb`  
  Experiments for Tables 2–3 that run non-CESSNA baselines (SeedGNN, PGM, SGM, multihop methods, etc.) on the same datasets.

- `CESSNA_plots_and_required_data/`  
  Scripts and data used to generate the figures in the paper (e.g., accuracy vs. number of seeds and correlation). The subdirectories contain `.npy` matrices and cached community-label files required by the notebooks.
  Note due to the randomness of seed choices, you might not be able to reproduce the exact same results by running functions yourself so we directly put our results here to generate plots. Our results are statistically robust and any new experiment is expected to behave similarly (just not exactly the same).

- `Experiments3_data/`  
  Correlated graph adjacency matrices for the Experiment 3 synthetic correlated Erdős–Rényi and stochastic block model graphs, stored as `.npy` files indexed by correlation parameter and replicate ID.

Paths inside may need to be changed based on your directory layout. 

## Datasets

### Correlated synthetic graphs

All synthetic correlated SBMs used in the paper were generated from the resources hosted at Johns Hopkins:

- <https://www.cis.jhu.edu/~parky/D3M/SGM/>

All synthetic DCSBMs used in the paper were included in `Experiments3_data/` and they were generated using - <https://docs.neurodata.io/graph-stats-book/representations/ch5/single-network-models_DCSBM.html>

### Real-world graphs

The notebooks also use several standard benchmark graphs:

- **Wiki**: A Wikipedia hyperlink graph with ground-truth community labels (Leiden, and variants with/without singleton communities), stored in `wikiadj.mat`, `wikilabels.mat`, and derived `.npy` label arrays.

- **C. elegans**: Paired chemical and gap-junction connectomes with neuron community labels, stored in `elegansGraph.mat` and `celeganslabels.mat`.

Because of licensing and size constraints, some of these `.mat` and `.npy` files may not be bundled; if they are missing, please follow the instructions in the paper to obtain them and place them into the expected paths (see the first few cells of each notebook).

## CESSNA implementation

The core CESSNA implementation lives in `CESSNA_code_and_experiments_table1.ipynb` and includes:

- Utility functions for community-aware seeding, permutation generation, and evaluation (e.g., `newgeneratearraycomm`, `blockSGM`, and helpers for community-size handling).
- Experiments on:
  - Synthetic block models with varying numbers of seeds.
  - Wiki and C. elegans graphs with both true and Leiden-derived communities, including variants with shuffled vertices and different seeding strategies.

Each experimental block in the notebook is annotated with comments indicating which table or figure in the paper it corresponds to.

## Non-CESSNA baselines (SeedGNN, PGM, SGM, multihop)

The `Experiments_table2-3_non_CESSNA.ipynb` notebook contains our implementations and wrappers for all non-CESSNA baselines used in Tables 2–3. In particular, it:

- Clones the original SeedGNN repository (`git clone https://github.com/Leron33/SeedGNN.git`) and installs its dependencies (PyTorch, torch-geometric, graspologic, etc.).
- Defines `SynGraphfromnumpy`, `run`, and related utilities to evaluate:
  - SeedGNN (graph neural network baseline),
  - MultiHop (1-hop, 2-hop, 3-hop neighborhood matching),
  - PGM, SGM2,
  - MGCN.

### Code adopted from SeedGNN

Portions of this repository—most notably the `SynGraphfromnumpy` routine, evaluation loops for SeedGNN/PGM/SGM/MultiHop/MGCN, and certain data-preprocessing utilities—are adapted from:

> Liren Yu, Jiaming Xu, and Xiaojun Lin.  
> “SeedGNN: Graph Neural Network for Supervised Seeded Graph Matching.”  
> *Proceedings of the 40th International Conference on Machine Learning (ICML)*, 2023.  

Please cite Yu et al. if you use the SeedGNN-based components of this repository in your own work.

## Running the experiments

1. Create a Python environment (for example, Google Colab with Python 3.x, PyTorch, torch-geometric 1.5.0, NumPy 1.20.x, SciPy 1.6.x, seaborn 0.11.x, graspologic 1.0.0; see the `pip install` cells in the notebooks for the exact versions we used).
2. Open the notebooks in Jupyter or Colab.
3. Adjust any paths in the setup cells (e.g., Google Drive mount points, `basepath`) so that they point to your local copies of the data.
4. Run all cells in:
   - `CESSNA_code_and_experiments_table1.ipynb` to reproduce CESSNA results.
   - `Experiments_table2-3_non_CESSNA.ipynb` to reproduce baseline results and Tables 2–3.

Some experiments, especially those involving many replicates or high correlation, can be computationally heavy and may take several hours on a CPU-only environment.

## Contact

For any question of the code, please contact - Sijing Yu at <sjyu@umd.edu>
