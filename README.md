# HHG-gatedMLP

This repository contains the datasets, trained models, and Jupyter notebooks for laser parameter estimation from high-order harmonic generation (HHG) spectra using a gated multilayer perceptron (gated MLP).
The HHG spectra are generated using the strong-field approximation (SFA), and neural networks are trained to retrieve laser parameters from single HHG spectra.

## Repository structure

```
HHG-gatedMLP/
│
├── notebooks/
│   ├── gatedMLP.ipynb
│   └── typical_MLP.ipynb
│
├── data/
│   ├── hhg_dataset_1.5cycles.npz
│   ├── hhg_dataset_1.75cycles.npz
│   ├── hhg_dataset_2cycles.npz
│   ├── hhg_dataset_1.5cycles_CEP0-piE0-0.08.npz
│   └── hhg_index_to_energy_au.txt
│
└── models/
    └── *.pth
```
## Data
The HHG datasets are generated from strong-field approximation (SFA) simulations.
Each dataset contains HHG spectra and the corresponding laser parameters used for the SFA calculations.

The data can be loaded as follows:
    import numpy as np
    data = np.load("../data/hhg_dataset_SFA_HHG_Laserparam_2cycles.npz")
    HHG_spec = data["HHG_spec"]
    laser_param = data["y"]   # [E0, sin(CEP), cos(CEP)]

where:
- `HHG_spec`: one-dimensional HHG spectral arrays used as input for machine learning.
- `laser_param`: corresponding laser parameters used as target values for model training.
  - `E0`: laser electric field amplitude
  - `sin(CEP)`, `cos(CEP)`: carrier-envelope phase (CEP) representation

The datasets
- `hhg_dataset_1.5cycles.npz`
- `hhg_dataset_1.75cycles.npz`
- `hhg_dataset_2cycles.npz`
contain 1500 HHG spectra each. The pulse duration is fixed for each dataset, while the laser electric field amplitude (`E0`) and CEP are randomly sampled.

A special dataset,
    hhg_dataset_1.5cycles_CEP0-piE0-0.08.npz
is provided for investigating the CEP dependence of HHG spectra.

In this dataset, the pulse duration is fixed to 1.5 cycles and the laser electric field amplitude is fixed to `E0 = 0.08 a.u.`. The CEP is varied from 0 to π with a step size of π/100:

    CEP = i * π / 100  (i = 0, 1, ..., 100)
resulting in 101 HHG spectra.
The file
    hhg_index_to_energy_au.txt
provides the conversion between the spectral array index and the photon energy axis in atomic units.

## Trained models
The directory `models/` contains pretrained PyTorch models.
Naming convention:

```
gatedMLP_seedX_nYcycle_lossMSE.pth
MLP_seedX_nYcycle_lossMSE.pth
```
where:
* `gatedMLP` : input-dependent gated multilayer perceptron
* `MLP` : conventional multilayer perceptron used for comparison
* `seedX` : random seed used during training
* `nYcycle` : pulse duration
* `lossMSE` : mean squared error loss function

Multiple random seeds are provided for evaluating the average performance and standard deviation.

## Requirements
The notebooks require the following Python packages:

- Python
- PyTorch
- NumPy
- scikit-learn
- Jupyter Notebook

The main libraries used in the notebooks are:
```python
import numpy as np

import torch
import torch.nn as nn
import torch.nn.functional as F
from torch.utils.data import DataLoader, TensorDataset

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import r2_score, mean_absolute_error
import random


## Usage
The notebooks demonstrate:

1. Loading the HHG datasets
2. Training the neural network models using the HHG spectra and corresponding laser parameters, and loading pretrained models
3. Predicting laser parameters from HHG spectra
4. Evaluating model performance

Run the notebooks in the `notebooks/` directory after installing the required Python packages.

## Citation

If you use this repository, please cite:

```
[Reference will be added after publication]
```
