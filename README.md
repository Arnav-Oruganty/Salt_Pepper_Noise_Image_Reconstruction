# Image Reconstruction using Convex Optimization

## Overview

This project investigates image reconstruction from salt-and-pepper noise using convex optimization techniques. The goal is to recover clean grayscale images by solving optimization problems that balance data fidelity and image smoothness.

Two reconstruction approaches are implemented and compared:

1. **Easy Problem (Quadratic Regularization)**
   - Uses smoothness constraints based on neighboring pixel differences.
   - Solved as a Quadratic Program (QP) using OSQP.

2. **Hard Problem (Total Variation Regularization)**
   - Uses Total Variation (TV) regularization to better preserve edges.
   - Solved as a convex optimization problem using SCS.

The project evaluates reconstruction quality using Mean Squared Error (MSE), objective values, and solver execution times.

---

## Mathematical Formulation

Given a noisy image \(X_{corr}\), reconstruct the clean image \(X\).

## Easy Formulation

Given a corrupted image $X_{corr}$, we reconstruct the image $X$ by solving:

$$
\min_X \|X - X_{corr}\|_F^2 +
\lambda \left(
\sum (X_{i,j+1}-X_{i,j})^2 +
\sum (X_{i+1,j}-X_{i,j})^2
\right)
$$

where:

- $\|X - X_{corr}\|_F^2$ is the data fidelity term.
- The regularization term promotes smoothness between neighboring pixels.
- $\lambda$ controls the trade-off between reconstruction fidelity and smoothness.
---

## Hard Formulation (Total Variation Regularization)

The Total Variation (TV) formulation is given by:

$$
\min_X \|X - X_{corr}\|_F^2 + \lambda\,TV(X)
$$

where

$$
TV(X)=\sum_{i,j}\sqrt{(X_{i+1,j}-X_{i,j})^2+(X_{i,j+1}-X_{i,j})^2}
$$

TV regularization preserves edges while removing noise, often producing sharper reconstructions than quadratic regularization.

---

## Dataset Preparation

Images are obtained from the Kaggle dataset:

- Salt-and-Pepper Noise Images Dataset

Processing steps:

1. Download source images.
2. Convert images to grayscale.
3. Extract central \(128 \times 128\) patches.
4. Generate noisy versions with:
   - 5% noise
   - 8% noise
   - 10% noise
5. Store both clean and corrupted images.

Directory structure:

```text
dataset_small/
│
├── ground_truth/
├── noise_5/
├── noise_8/
└── noise_10/
```

---

## Project Structure

```text
Image_Reconstruction/
│
├── prepare_dataset.py
│
├── run_easy_analysis.py
├── run_hard_analysis.py
│
├── easy_optima.py
├── hard_optima.py
│
├── dataset_small/
│
├── easy_version_results_small/
├── hard_version_results_small/
│
└── optima/
    ├── easy/
    └── hard/
```

---

## Installation

### Clone Repository

```bash
git clone <repository-url>
cd Image_Reconstruction
```

### Install Dependencies

Using uv:

```bash
uv sync
```

or using pip:

```bash
pip install cvxpy ecos numpy matplotlib scikit-image kagglehub
```

---

## Running the Project

### Step 1: Prepare Dataset

```bash
python prepare_dataset.py
```

This will:

- Download the dataset
- Create image patches
- Generate noisy images

---

### Step 2: Analyze Easy Formulation

```bash
python run_easy_analysis.py
```

Outputs:

- Reconstructed sample images
- MSE vs λ plots
- Reconstruction performance metrics

---

### Step 3: Analyze Hard Formulation

```bash
python run_hard_analysis.py
```

Outputs:

- TV-based reconstructions
- MSE vs λ plots
- Performance evaluation

---

### Step 4: Generate Optimal Reconstructions

#### Easy Problem

```bash
python easy_optima.py
```

Uses:

| Noise Level | Optimal λ |
|------------|-----------|
| 5% | 1.0 |
| 8% | 1.2 |
| 10% | 1.4 |

---

#### Hard Problem

```bash
python hard_optima.py
```

Uses:

| Noise Level | Optimal λ |
|------------|-----------|
| 5% | 100 |
| 8% | 100 |
| 10% | 100 |

---

## Evaluation Metric

### Mean Squared Error (MSE)

$$
MSE=\frac{1}{MN}\sum_{i=1}^{M}\sum_{j=1}^{N}\left(X^{recon}_{i,j}-X^{true}_{i,j}\right)^2
$$

Lower MSE values indicate better reconstruction quality.

---

### Objective Value

Measures the optimization objective achieved by the solver.

---

### Solver Time

Average computational time required to solve each reconstruction problem.

---

## Solvers Used

| Formulation | Solver |
|------------|---------|
| Easy (QP) | OSQP |
| Hard (TV) | SCS |

---

## Results

The project compares:

- Reconstruction quality
- Noise robustness
- Computational efficiency
- Sensitivity to regularization parameter λ

Generated outputs include:

- Reconstructed images
- MSE-vs-λ plots
- Average objective values
- Average solver runtimes

---

## Technologies Used

- Python 3.10+
- CVXPY
- NumPy
- Scikit-Image
- Matplotlib
- KaggleHub
- OSQP
- SCS

---

## Future Improvements

- Adaptive regularization parameter selection
- Color image reconstruction
- Larger benchmark datasets
- Alternative denoising priors
- First-order optimization methods for scalability

---

## Authors

Developed as part of a convex optimization and image reconstruction study exploring denoising through regularized optimization techniques.
