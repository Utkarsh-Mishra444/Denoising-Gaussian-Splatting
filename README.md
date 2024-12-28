# 3D Gaussian Splatting with Denoising and Regularization for Improved Scene Reconstruction

This project introduces a novel approach to 3D scene reconstruction using a modified 3D Gaussian Splatting technique. We enhance the original method by addressing noise and overfitting, leading to improved reconstruction quality, particularly when using challenging input data like low-resolution or wide-angle images.

## Overview

Our core contributions are:

1.  **Robust Point Cloud Denoising:** A two-stage denoising pipeline significantly improves the quality of the initial point cloud obtained from COLMAP, providing a cleaner starting point for optimization.

    *   **Pointwise Distance Pairing:** Removes isolated, spurious points based on their distance to nearest neighbors, effectively filtering outliers.

    *   **DBSCAN Clustering:** Identifies and retains points belonging to dense clusters, discarding noise and outliers that can negatively impact reconstruction quality.

2.  **Novel L2 Regularization:**  We introduce an L2 regularization term during training that penalizes Gaussians deviating from their original starting positions (computed in the denoising stage). This regularization mitigates overfitting and prevents gaussians from randomly placing themselves in space to fit sparse inputs for preventing reconstructions that don't interpolate well.

## Usage and Installation

1.  **Clone the Repository:**

    ```bash
    git clone https://github.com/Utkarsh-Mishra444/Denoising-Gaussian-Splatting.git --recursive
    ```

2.  **Environment Setup:** Follow the original 3D Gaussian Splatting repository's instructions to set up the environment, creating a Conda environment using the provided environment.yml with CUDA 11.3. Or use the environment.yml file that comes with this repository, which already contains additional libraries (like scikit-learn) for the DBSCAN implemenation. Ensure proper installation of the submodules.

3.  **Training:** Run `train.py` with the following key new arguments:

    ```bash
    python train.py --config <path to config file> -s <path to dataset> -m <path to log directory> --apply_dbscan --apply_regularization
    ```

    *   `--apply_dbscan`: Enables DBSCAN clustering for point cloud denoising. (Default: *False*)
    *   `--dbscan_eps <value>`: DBSCAN epsilon parameter.
    *   `--dbscan_min_samples <value>`: DBSCAN minimum samples parameter.
    *   `--cluster_pruning_threshold <value>`: Sets the distance threshold from the cluster center for DBSCAN.
    *   `--apply_regularization`: Enables L2 regularization. (Default: *False*)
    *   `--regularization_weight <value>`: L2 regularization strength.
    *   `--distance_threshold <value>`: Distance threshold for pruning isolated Gaussians during training.

4.  **Evaluation:** Use  `render.py`, `metrics.py` and `render_360.py`, following the original repository's instructions.

## Acknowledgements
This project builds upon the 3D Gaussian Splatting implementation by the GRAPHDECO research group at Inria.  We gratefully acknowledge their foundational work.  Please see their repository for full details and licensing information:

*   [3D Gaussian Splatting for Real-Time Radiance Field Rendering](https://github.com/graphdeco-inria/gaussian-splatting)
