---
authors:
  - shubhamdang
date: 2024-04-12
categories:
  - se1
readtime: 2
---

# Predict Molecule Solubility With Deepchem

In this blog post, we will provide a comprehensive guide on analysing molecule solubility using deepchem. Our setup revolves around a single node with Jupyter notebook installed.


Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, followed by installation of jupyter lab and notebook and then analysis of molecule solubility.
<!-- more -->


## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

## Install Jupyter Notebook and Lab
All the steps to install jupyter notebook and lab is provided in [link](./jupyter-lab-notebook.md).



## Setup
- First we need to make sure that DeepChem is running, We are going to use model based on tensorflow, because of that we need to add \[tensorflow\] to the pip install command to ensure the necessary dependencies are also installed.

    ```In []: pip install --pre deepchem[tensorflow]
    ```

-  Once Installation is complete, verify the installation of deepchem using below command.
    ```
    In []:  import deepchem as dc
            dc.__version__
    ```

## Steps to Train a model with Deepchem

- Deepchem has a collection of chemical and molecular data sets. For this blog, we can use the Delaney solubility datasets to train our model.

    ```
    In []:  tasks, datasets, transformers = dc.molnet.load_delaney(featurizer='GraphConv')
            train_dataset, valid_dataset, test_dataset = datasets
    ```

- After acquiring our data, the subsequent task involves model creation. We'll employ a specialized model known as a 'graph convolutional network,' abbreviated as 'graphconv'.

    ```
    In []:  model = dc.models.GraphConvModel(n_tasks=1, mode='regression', dropout=0.2)
    ```

- We now need to train the model on the data set. We simply give it the data set and tell it how many epochs of training to perform.
    ```
    In []:  model.fit(train_dataset, nb_epoch=100)
    ```

- Let's assess the model's performance! We can do this by applying an evaluation metric on the test set. In this case, we'll use the Pearson correlation coefficient (r^2) to measure how well the model's predictions match the actual values. We can also evaluate this metric on the training set for comparison.
    ```
    In []:  metric = dc.metrics.Metric(dc.metrics.pearson_r2_score)
            print("Training set score:", model.evaluate(train_dataset, [metric], transformers))
            print("Test set score:", model.evaluate(test_dataset, [metric], transformers))

    Output
    ------
    Training set score: {'pearson_r2_score': 0.9323622956442351}
    Test set score: {'pearson_r2_score': 0.6898768897014962}
    ```

!!! note
    Observe that the score is higher on the training set compared to the test set. Typically, models exhibit superior performance on the specific data they were trained on, as opposed to similar yet independent datasets. This phenomenon, known as 'overfitting,' underscores the importance of evaluating your model on an independent test set.


- Let's narrow our focus to the first ten molecules from the test set. For each, we'll display the chemical structure, represented as a SMILES string, alongside the predicted log(solubility). Additionally, we'll provide context by including log(solubility) values from the test set.

    ```
    In []:  solubilities = model.predict_on_batch(test_dataset.X[:10])
            for molecule, solubility, test_solubility in zip(test_dataset.ids, solubilities, test_dataset.y):
                print(solubility, test_solubility, molecule)
    
    Output
    ------
    [-1.8629359] [-1.60114461] c1cc2ccc3cccc4ccc(c1)c2c34
    [0.6617248] [0.20848251] Cc1cc(=O)[nH]c(=S)[nH]1
    [-0.5705674] [-0.01602738] Oc1ccc(cc1)C2(OC(=O)c3ccccc23)c4ccc(O)cc4 
    [-2.0929456] [-2.82191713] c1ccc2c(c1)cc3ccc4cccc5ccc2c3c45
    [-1.4962314] [-0.52891635] C1=Cc2cccc3cccc1c23
    [1.8620405] [1.10168349] CC1CO1
    [-0.5858227] [-0.88987406] CCN2c1ccccc1N(C)C(=S)c3cccnc23 
    [-0.9799993] [-0.52649706] CC12CCC3C(CCc4cc(O)ccc34)C2CCC1=O
    [-1.0176951] [-0.76358725] Cn2cc(c1ccccc1)c(=O)c(c2)c3cccc(c3)C(F)(F)F
    [0.05622783] [-0.64020358] ClC(Cl)(Cl)C(NC=O)N1C=CN(C=C1)C(NC=O)C(Cl)(Cl)Cl 
    ```


!!! note 
    For more details about the please check the [link](https://github.com/deepchem/deepchem/blob/master/examples/tutorials/The_Basic_Tools_of_the_Deep_Life_Sciences.ipynb)
