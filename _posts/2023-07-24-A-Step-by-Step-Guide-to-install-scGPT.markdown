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


However, to harness the full potential of scGPT, it's crucial to understand how to install it correctly. While the [GitHub repository](https://github.com/bowang-lab/scGPT) simply suggests using `pip install scgpt` for installation, the reality could be a bit more nuanced. You might encounter various issues during the process that the official documentation doesn't cover. Therefore, in this guide, we aim to provide a comprehensive walkthrough of the installation process, preparing you for any potential hiccups you might face. Our goal is to ensure that you can fully leverage this powerful tool without being stalled by technical roadblocks.

## Why Mamba?

Before diving into the installation steps, it's essential to understand why we're using Mamba, a fast and reliable package manager, as opposed to pip:

- Mamba uses a parallel solver, which makes it faster than pip and conda.
- It manages environments and package installations more efficiently, minimizing the chances of package conflicts.
- Lastly but most importantly, mamba can resolve dependencies quickly and accurately, especially when installing complex packages like `cuda-toolkit`.

## Installing scGPT: A Step-by-Step Guide

### Step 1: Install Mamba

Follow the guidelines provided in the [official Mamba documentation](https://mamba.readthedocs.io/en/latest/installation.html) for a fresh and hassle-free installation.

### Step 2: Create a New Mamba Environment

Once Mamba is installed, initiate a new environment specifically for scGPT:

```bash
mamba create --name scgpt
```

### Step 3: Activate the Environment

Activate the new environment with the following command:

```bash
conda activate scgpt
```

### Step 4: Attempt to Install scGPT

Start the scGPT installation process using pip:

```bash
pip install scgpt
```

### Step 5: Install PyTorch 1.13

You might encounter an error indicating that PyTorch is not installed. As stated in scGPT's [to-do list](https://github.com/bowang-lab/scGPT#to-do-list), the package does not yet support PyTorch 2.0. Therefore, we need to install PyTorch 1.13:

```bash
pip install torch==1.13
```

### Step 6: Attempt to Install scGPT Again

With PyTorch installed, try installing scGPT again:

```bash
pip install scgpt
```

### Step 7: Install nvcc 11.7

If the installation fails again with a `build flash_attn failed` error, it means no nvcc was found. The installation requires an nvcc version compatible with the PyTorch compiled version. Since PyTorch is compiled with CUDA 11.7, we need nvcc 11.7:

```bash
mamba install -y -c "nvidia/label/cuda-11.7.0" cuda-toolkit
```

### Step 8: Attempt to Install scGPT Again

With nvcc 11.7 installed, try installing scGPT once more:

```bash
pip install scgpt
```

If you encounter a "No such file or directory 'R'" error message, proceed to the next step.

### Step 9: Install R

To resolve the aforementioned error, install R using the following commands:

```bash
sudo apt update
sudo apt install r-base
```

### Step 10: Attempt to Install scGPT Again

Now, you can try installing scGPT again:

```bash
pip install scgpt
```

### Step 11: Update GCC

Even after successful installation, you might face an ImportError related to `GLIBCXX_3.4.29` when trying to import scGPT. This issue can be resolved by updating GCC:

```bash
sudo apt-get update
sudo apt-get upgrade gcc
```

### Step 12: Import scGPT

Finally, open a Python shell and import scGPT:

```python
import scgpt
```

If you see "Global seed set to 0", it means scGPT has been successfully installed.

By following this guide, you should have successfully navigated through the common installation issues and are now ready to start exploring your single-cell omics data with the power of scGPT!
