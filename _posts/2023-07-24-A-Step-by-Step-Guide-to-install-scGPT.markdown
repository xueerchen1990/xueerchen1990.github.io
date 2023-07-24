---
layout: post
title:  "A Step-by-Step Guide to install scGPT"
date:   2023-07-24 09:33:12 -0400
categories: scGPT
published: true
---
# A Step-by-Step Guide to Installing scGPT

In the rapidly evolving field of single-cell omics data analysis, a new player, scGPT, has emerged, showing promising capabilities in managing and interpreting complex single-cell data. As detailed in a [recent preprint](https://www.biorxiv.org/content/10.1101/2023.04.30.538439v2), scGPT is a generative pretrained transformer model that has been pretrained on over 33 million human cells to learn representations of cells and genes.

The beauty of scGPT lies in its flexibility. Once pretrained, the model can be fine-tuned on smaller datasets to perform diverse downstream tasks such as cell type annotation, perturbation response prediction, batch correction, multi-omic data integration, and gene regulatory network inference. Impressively, scGPT has shown state-of-the-art performance on these tasks compared to previous methods.

scGPT is not just a powerful computational tool. It is also a platform for biological discovery. The model's pretrained embeddings and attention weights offer insights into gene-gene interactions, unveiling the hidden biological knowledge that it has captured. The observed scaling law—where larger pretraining data leads to improved performance on downstream tasks after fine-tuning—suggests that scGPT will continue to grow stronger as more cellular sequencing data become available. This makes scGPT the first pretrained foundation model for single-cell data that enables flexible transfer learning.

In this guide, we will walk you through how to install this powerful tool and address any potential hiccups you might encounter in the process.

## Why Use Mamba Over Pip?

We will be using Mamba, a package manager that is a drop-in replacement for conda, for the installation. Here are some reasons why Mamba is a better choice than pip:

- It's fast. Thanks to its parallel solver, Mamba is faster than pip and conda.
- It excels in managing environments and package installations, thus reducing the risk of package conflicts.
- Mamba efficiently resolves dependencies, which is critical for complex packages like scGPT.

## Installation Steps

### Step 1: Install Mamba

Follow the instructions on the [official Mamba documentation](https://mamba.readthedocs.io/en/latest/installation.html) to install Mamba on your machine.

### Step 2: Create a New Mamba Environment

Create a new environment specifically for scGPT:

```bash
mamba create --name scgpt
```

### Step 3: Activate the Environment

Activate the new environment:

```bash
conda activate scgpt
```

### Step 4: Install scGPT

Attempt to install scGPT using pip:

```bash
pip install scgpt
```

### Step 5: Install PyTorch 1.13

In case you encounter an error stating that PyTorch is not installed, install PyTorch 1.13:

```bash
pip install torch==1.13
```

### Step 6: Install nvcc 11.7

If the installation of scGPT fails due to the absence of nvcc, install nvcc 11.7:

```bash
mamba install -y -c "nvidia/label/cuda-11.7.0" cuda-toolkit
```

### Step 7: Install R

If you encounter an error message saying "No such file or directory 'R'", you need to install R:

```bash
sudo apt update
sudo apt install r-base
```

### Step 8: Update GCC

If an ImportError related to `GLIBCXX_3.4.29` not being found pops up, update GCC:

```bash
sudo apt-get update
sudo apt-get upgrade gcc
```

### Step 9: Import scGPT

Finally, import scGPT:

```python
import scgpt
```

You should see "Global seed set to 0", signifying that the installation process is complete.

By following this guide, you should now have a functional installation of scGPT, a powerful tool that is pushing the boundaries of single-cell omics data analysis.
