---
authors:
  - shubhamdang
date: 2024-04-13
categories:
  - se1
readtime: 2
---

# Unknown Protein Analysis with BioPython

In this blog post, we will provide a comprehensive guide on analysing unknown protein using Biopython. Our setup revolves around a single node with Biopython installed.


Let's start with a step-by-step process, starting from creating virtual machines on the Alces Cloud platform, followed by installation of biopython and then analysis of Unknown protein.
<!-- more -->

## Launch the Instance  
All the steps to launch and connection to instance is provided in [link](../../docs/starter/instance.md).

## Setup
- First update package lists and install essential dependencies for jupyter lab and notebook.
    ```
    sudo dnf update
    sudo dnf install python3 python3-devel gcc libffi-devel openssl-devel
    ```


- Install Biopython using pip:
    ```
    python3 -m pip install biopython
    ```

- Verification can be done using below command.
    ```
    biopython --version
    ```

## What is Biopython and its capabilities

Biopython is a widely used open-source library for computational biology and bioinformatics tasks. It's written in Python and provides tools for a variety of bioinformatics tasks, making it a powerful resource for researchers, developers, and bioinformaticians alike. Here's a brief overview of its capabilities:

- Sequence Analysis: Biopython offer functionality for the manipulation and analysis of biological sequence data, including DNA, RNA, and protein sequences. 

- File Parsing: It supports parsing various file formats commonly used in bioinformatics, such as FASTA, GenBank, BLAST, and PDB formats. This allows users to read, write, and manipulate sequence data stored in different formats seamlessly.

- Structural Biology: Biopython provides tools for working with biomolecular structures, enabling tasks such as parsing and analysing protein structures from PDB files, calculating structural properties, and performing structure alignments.

- Phylogenetics: It includes modules for phylogenetic analysis, allowing users to construct, manipulate, and visualize phylogenetic trees. This is particularly useful for studying evolutionary relationships between different species or sequences.

- Population Genetics: Biopython offers functionality for population genetics analysis, including computation of genetic diversity indices, estimation of evolutionary distances, and simulation of genetic data.

- Bioinformatics Algorithms: It implements various algorithms commonly used in bioinformatics, such as sequence alignment algorithms (e.g., Smith-Waterman, Needleman-Wunsch), sequence searching algorithms (e.g., BLAST), and phylogenetic reconstruction methods (e.g., neighbour-joining, maximum likelihood).

- Integration with Other Tools: Biopython can be integrated with other bioinformatics tools and databases, facilitating workflows and enabling seamless access to external resources.


## Analyse Unknown Protien 

Biopython simplifies analysis of unknown proteins by enabling researchers to calculate key properties like amino acid count, charge at specific pH, aromaticity, and isoelectric point. These features, along with the instability index, provide a foundation for protein classification and further studies. Below is the example to showcase biopython capabilities in analysing unknown protein.

```
from Bio.SeqUtils.ProtParam import ProteinAnalysis
import pprint

# below is given the Unknown Protein.
X = ProteinAnalysis("MAEGEITTFTALTEKFNLPPGNYKKPKLLYCSNGGHFLRILPDGTVDGT"
                    "RDRSDQHIQLQLSAESVGEVYIKSTETGQYLAMDTSGLLYGSQTPSEEC"
                    "LFLERLEENHYNTYTSKKHAEKNWFVGLKKNGSCKRGPRTHYGQKAILF"
                    "LPLPV")

analysis_dict = {
"Amino Acid Count" : X.count_amino_acids(),
"Amino Acid Percentage" : X.get_amino_acids_percent(),
"Molecular Weight" : X.molecular_weight(),
"Aromaticity":  X.aromaticity(),
"Instability Index" : X.instability_index(),
"Isoelectric Point" : X.isoelectric_point(),
"Charge At pH" : X.charge_at_pH(7)
}

pp = pprint.PrettyPrinter(indent=4)
pp.pprint(analysis_dict)
print(analysis_dict)

Output
------
{   'Amino Acid Count': {   'A': 6,
                        'C': 3,
                        'D': 5,
                        'E': 12,
                        'F': 6,
                        'G': 14,
                        'H': 5,
                        'I': 5,
                        'K': 12,
                        'L': 18,
                        'M': 2,
                        'N': 7,
                        'P': 8,
                        'Q': 6,
                        'R': 6,
                        'S': 10,
                        'T': 13,
                        'V': 5,
                        'W': 1,
                        'Y': 8},
'Amino Acid Percentage': {   'A': 0.039473684210526314,
                                'C': 0.019736842105263157,
                                'D': 0.03289473684210526,
                                'E': 0.07894736842105263,
                                'F': 0.039473684210526314,
                                'G': 0.09210526315789473,
                                'H': 0.03289473684210526,
                                'I': 0.03289473684210526,
                                'K': 0.07894736842105263,
                                'L': 0.11842105263157894,
                                'M': 0.013157894736842105,
                                'N': 0.046052631578947366,
                                'P': 0.05263157894736842,
                                'Q': 0.039473684210526314,
                                'R': 0.039473684210526314,
                                'S': 0.06578947368421052,
                                'T': 0.08552631578947369,
                                'V': 0.03289473684210526,
                                'W': 0.006578947368421052,
                                'Y': 0.05263157894736842},
'Aromaticity': 0.09868421052631579,
'Charge At pH': 0.9258119875149688,
'Instability Index': 41.980263157894726,
'Isoelectric Point': 7.7224523544311525,
'Molecular Weight': 17103.1617}
```


!!! note 
    For more information and operations, please check the Biopython documentation [link](https://biopython.org/wiki/Documentation)
