# Automated Deep Learning Approach for Nucleus Segmentation in Fluorescence LEXY Videos

This repository contains the Python scripts used to perform the data acquisition, model training, and performance evaluation for our study on deep learning-based nucleus segmentation in fluorescence LEXY videos. You can get the full paper [here](./LEXY.pdf).

## Abstract
Nucleus segmentation in fluorescence microscopy is crucial for studying nucleocytoplasmic transport, particularly with optogenetic systems like LEXY (light-inducible nuclear export). LEXY allows precise, reversible control of nuclear protein export via irradiation, but videos of cells expressing LEXY present significant segmentation challenges. As irradiation progresses, the nuclear-cytoplasmic contrast diminishes, leading to frames where the nucleus becomes difficult or impossible to distinguish from the cytosol. To address these challenges, an automated deep learning approach is proposed that integrates temporal information to enhance segmentation accuracy in these challenging fluorescence LEXY videos. Using pretraining on the large, high-quality Human Protein Atlas (HPA) dataset, I evaluated U-Net and two architectures, Track-Net and Siam-Net, which incorporate the previous frame mask and a reference frame and mask, respectively. Given the heterogeneity of the LEXY dataset, which contains only 10 videos, I generated synthetic videos from HPA data designed to mimic LEXY conditions. On these synthetic sequences, Track-Net and Siam-Net exhibited significant error propagation over time, resulting in poor segmentation performance. In contrast, the base U-Net model produced more consistent, though still suboptimal, predictions. Testing the base U-Net on the actual LEXY dataset showed that pretraining on HPA improved segmentation, achieving a Dice score of 0.55. It is clear that more training data and exploration of alternative architectures is needed to fully address the segmentation challenges posed by LEXY videos.

## Repository Contents

The project is organized into a data processing pipeline, model implementations, and result files.

*   **`get_hpa_data.py`**
    *   This script handles the entire data preparation workflow. It downloads image data from the Human Protein Atlas, uses Cellpose to generate segmentation masks for the nuclei, and processes these images into training, validation, and test sets.
    *   It also contains the logic for generating the synthetic video sequences that mimic the cell translocation and intensity changes observed in the real LEXY videos.

*   **`Models/`**
    *   This directory contains the PyTorch implementations of the neural network architectures and the training scripts. The model architectures are consistent across files, but the scripts are tailored for different datasets and experimental setups.
    *   `u_net_hpa.py`, `tracknet_hpa.py`, `siamnet_hpa.py`: These are the primary training scripts for the large HPA dataset. They use a standard train/validation split and also include code for evaluating performance on the synthetic HPA video sequences.
    *   `u_net_lexy.py`, `tracknet_lexy.py`, `siamnet_lexy.py`: These are specialized scripts for the LEXY dataset. They handle the loading and preprocessing of raw `.tif` files and use cross-validation for evaluation. They can also load model weights pre-trained on the HPA dataset.

*   **`Results/`**
    *   This directory contains the output `.txt` files from the model evaluation experiments.
    *   The file names clearly describe the experiment (e.g., `HPAseq_results_ResNet34_UNet_dice.txt`), and the files contain the quantitative metrics (Dice, Hausdorff distance, etc.) discussed in the paper.
