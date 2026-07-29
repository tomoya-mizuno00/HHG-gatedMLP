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
Each dataset contains 1500 HHG spectra and the corresponding laser parameters used for the SFA calculations.
The data can be loaded as follows:

```python
data = np.load("data/hhg_dataset_SFA_HHG_Laserparam_2cycles.npz")

HHG_spec = data["HHG_spec"]
laser_param = data["y"]  # [E0, sin(CEP), cos(CEP)]
The file

```
hhg_index_to_energy_au.txt
```
provides the correspondence between the spectral array index and the photon energy axis in atomic units.
The datasets correspond to different pulse durations:

* `1.5cycles`
* `1.75cycles`
* `2cycles`

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
