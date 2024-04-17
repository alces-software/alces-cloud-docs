---
authors:
  - shubhamdang
date: 2024-04-15
categories:
  - se1
readtime: 2
---

# FermiNet Tutorial: Calculating the Ground State Energy of the H2 Molecule Using DeepChem
In this blog, we will demonstrate how to use the FermiNet model implemented in Deep Chemistry (DeepChem) to calculate the ground state energy of the hydrogen (H2) molecule. We will cover the following steps:

- Launch the Instance in Alces Cloud and install Jupyter Notebook.
- Installing required packages
- Initializing and pretraining the FermiNet model
- Training the FermiNet model to obtain the ground state energy
<!-- more -->

## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

## Install Jupyter Notebook and Lab
All the steps to install jupter notebook and lab is provided in [link](./jupyter-lab-notebook.md).


## Installing Required Packages
First, make sure you have installed the latest version of Python and pip. Next, run the following commands to install the necessary packages:
```
!pip install --pre deepchem
!pip install torch_geometric
!pip install pyscf
```
## Initializing and Pretraining the FermiNet Model
To initialize the FermiNet model, provide a list of lists containing the nuclear coordinates in the following format: [[element symbol, [3D coordinates]]]. For example, to define an H2 molecule, create the variable H2_molecule as follows:
```
H2_molecule = [['H', [0, 0, 0]], ['H', [0, 0, 1.4135151]]]
```

Next, initialize the FermiNet model with the desired configuration. In our case, we set the spin to 0, ionic charge to 0, and four batches to improve accuracy.

```
from deepchem.models.torch_models.ferminet import FerminetModel
import torch
import logging

logger = logging.getLogger()
logger.setLevel(logging.DEBUG)

mol = FerminetModel(H2_molecule, spin=0, ion_charge=0, batch_no=4)
mol.train(nb_epoch=150)
# After pretraining, verify the obtained energy:

mol.model.forward(torch.from_numpy(mol.molecule.x))
energy = mol.model.calculate_electron_electron() \
          - mol.model.calculate_electron_nuclear() \
          + mol.model.nuclear_nuclear_potential \
          + mol.model.calculate_kinetic_energy()
mean_energy = torch.mean(energy)
print("Energy before start:", mean_energy)
```

This should produce an energy close to the Hartree-Fock (HF) ground state energy.

## Training the FermiNet Model to Obtain the Ground State Energy
Before beginning the actual training process, ensure that the "prepare_train" method has been called to perform Markov Chain Monte Carlo (MCMC) burn-in and reinitialize electron positions. Then, call the "train" method to initiate the FermiNet training.

```
import logging

logger = logging.getLogger()
logger.setLevel(logging.DEBUG)

mol.prepare_train()
mol.train(nb_epoch=100, lr=0.0002, std=0.04)
```

Once completed, access the final_energy attribute of the mol object to view the net average ground state energy over all training epochs.

```
mol.final_energy.item()
```

This value should be close to the expected theoretical value of approximately 1.174476 Hartrees. To achieve higher precision, increase the number of iterations or fine-tune other hyperparameters such as MCMC step proposals and layer sizes.
