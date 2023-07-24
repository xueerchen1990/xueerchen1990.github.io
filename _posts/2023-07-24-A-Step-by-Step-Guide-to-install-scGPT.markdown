---
layout: post
title:  "A Step-by-Step Guide to install scGPT"
date:   2023-07-24 09:33:12 -0400
categories: scGPT
published: true
---

In the evolving world of omics, scGPT has emerged as an open-source Foundation Model for Single-Cell Omics. With its ability to model single-cell transcriptomics and other single-cell omics data, it has become a tool of choice for many bioinformaticians and computational biologists. This blog will provide a comprehensive guide on installing scGPT, solving common error issues along the way.

More information about scGPT is available at [GitHub](https://github.com/bowang-lab/scGPT) and a detailed description of the project can be found in the [associated research paper](https://www.biorxiv.org/content/10.1101/2023.04.30.538439v2).

## Why Use Mamba Over Pip?

Before we jump into the installation process, it's worth discussing why we'll use Mamba, a package manager that is a drop-in replacement for conda. Here are some reasons why Mamba is preferable over pip:

- Mamba is faster than pip and conda, thanks to its use of a parallel solver.
- It manages environments and package installations in a more reliable and efficient manner.
- Mamba excels in resolving dependencies, reducing the risk of package conflicts.

## Installing scGPT

Let's walk through the installation process step by step.

### Step 1: Install Mamba

We'll begin by installing Mamba. For the freshest installation, follow the instructions detailed in the [official documentation](https://mamba.readthedocs.io/en/latest/installation.html).

### Step 2: Create a New Mamba Environment

Once Mamba is installed, we'll create a new environment specifically for scGPT. Open your terminal and type the following command:

```bash
mamba create --name scgpt
```

### Step 3: Activate the Environment

Next, activate the environment with the following command:

```bash
conda activate scgpt
```

### Step 4: Install scGPT

Let's try installing scGPT using pip:

```bash
pip install scgpt
```

You may encounter an error stating that PyTorch is not installed. 

### Step 5: Install PyTorch 1.13

At the time of writing, PyTorch 2.0 is not supported by scGPT. Therefore, we'll install an older version, specifically PyTorch 1.13:

```bash
pip install torch==1.13
```

### Step 6: Install nvcc 11.7

Upon reattempting the installation of scGPT, another error might appear: `build flash_attn failed`. This error is caused by the absence of nvcc, so we need to install it. Given that PyTorch is compiled with CUDA 11.7, we need a compatible version of nvcc. Use the following command to install nvcc 11.7:

```bash
mamba install -y -c "nvidia/label/cuda-11.7.0" cuda-toolkit
```

### Step 7: Install R

If another error message saying "No such file or directory 'R'" pops up during the installation of scGPT, we need to install R. Run the following commands:

```bash
sudo apt update
sudo apt install r-base
```

### Step 8: Update GCC

Even after successfully installing scGPT, when opening a Python shell and running `import scgpt`, you might face an ImportError related to `GLIBCXX_3.4.29` not being found. To resolve this issue, we need to update GCC:

```bash
sudo apt-get update
sudo apt-get upgrade gcc
```

### Step 9: Import scGPT

Now, open a Python shell and try to import scGPT again:

```python
import scgpt
```

The command should work fine, outputting "Global seed set to 0", signifying that the installation process is complete.

Following this guide will help you overcome common installation hurdles. Now, you can enjoy analyzing your single-cell omics data with scGPT!

