# Wavelet-Based Image Denoising from Scratch

## Overview

This project implements a custom algorithm for denoising noisy grayscale images using the Discrete Wavelet Transform (DWT) with the Haar wavelet, implemented from scratch in Python. The algorithm performs multi-level wavelet decomposition, applies denoising techniques (median filtering on approximation coefficients and custom thresholding on detail coefficients), and reconstructs the image. It compares the results with standard denoising methods like mean filtering, median filtering, BayesShrink, and VisuShrink using metrics such as Mean Squared Error (MSE) and Structural Similarity Index (SSIM).

The implementation avoids external wavelet libraries (e.g., PyWavelets) and relies on core libraries like NumPy and SciPy for convolutions and basic operations. It is tested on PGM images from a dataset, specifically using the "Barbara" image as an example.

Key features:

- Custom Haar wavelet decomposition and reconstruction.
- Denoising using a combination of median filtering and hard/soft thresholding.
- Performance evaluation and visualization of results.
- Comparison with baseline and advanced denoising techniques.

## Requirements

- Python 3.12+
- Libraries:
  - NumPy (`numpy`)
  - Matplotlib (`matplotlib`)
  - SciPy (`scipy`)
  - scikit-image (`skimage`) for metrics, data conversion, and built-in denoising methods

Install dependencies using:

```
pip install numpy matplotlib scipy scikit-image pandas
```

## Dataset

The code assumes a dataset structure with PGM images:

- Original images in `Dataset/original/` (e.g., `barbara.pgm`).
- Noisy images in `Dataset/noisy_1/` (e.g., `barbara.pgm`).

PGM files are read in both ASCII (P2) and binary (P5) formats. Ensure the dataset is placed in the root directory or adjust paths accordingly.

## Usage

1. Place your PGM images in the `Dataset/` directory as described above.

2. The script will:
   - Load and display the original and noisy images with histograms.
   - Perform wavelet decomposition and display subbands.
   - Reconstruct the original image to verify the transform (MSE should be near zero).
   - Apply the custom denoising algorithm (using hard thresholding by default).
   - Reconstruct and display the denoised image.
   - Compute and print MSE and SSIM between original/denoised and noisy/denoised.
   - Apply baseline methods (mean, median, BayesShrink, VisuShrink).
   - Display a comparison plot of denoised images.
   - Print MSE/SSIM for all methods and plot bar charts for comparison.

Customization:

- Change the wavelet level with `level = 3` (default: 3).
- Switch thresholding method in `denoising_method` (e.g., `'soft'` instead of `'hard'`).
- Adjust kernel sizes for mean/median filters (default: 5).
- Modify the image paths or add support for other formats.

## Algorithm Details

### 1. Image Loading and Preprocessing

- Read PGM images (supports P2 ASCII and P5 binary formats).
- Convert to float [0, 1] range using `img_as_float`.
- Pad the image to even dimensions if needed for downsampling in DWT.
- Display images and histograms for visual inspection.

### 2. Wavelet Decomposition

- Uses Haar wavelet filters:
  - Low-pass: `[1/√2, 1/√2]`
  - High-pass: `[1/√2, -1/√2]`
- Performs 2D DWT via 1D convolutions along rows and columns, followed by downsampling (every 2nd element).
- Multi-level decomposition (up to specified `level`).
- Outputs coefficients: approximation (LL), horizontal (LH), vertical (HL), diagonal (HH) for each level.

### 3. Denoising

- **Approximation coefficients**: Apply median filtering with a kernel (e.g., 3x3) to smooth low-frequency components.
- **Detail coefficients** (horizontal, vertical, diagonal):
  - Estimate noise variance from the diagonal subband: `σ = median(|HH|) / 0.6745`, then variance = `σ²`.
  - Compute total variance and coefficient variance (total - noise).
  - Custom thresholding:
    - Threshold `T = √noise - √coefficient_variance`.
    - For each coefficient:
      - If `|coeff - mean(coeffs)| > T`, set to 0 (remove large deviations, assuming signal).
      - Else:
        - Hard: Keep original value.
        - Soft: Shrink by `sign(coeff) * max(|coeff| - T, 0)`.
- Supports 'hard' or 'soft' modes (default: 'hard').

Note: The thresholding logic inverts traditional approaches by preserving small deviations (noise?) and removing large ones (signal?). This may be tuned based on empirical results.

### 4. Wavelet Reconstruction

- Upsample coefficients by inserting zeros.
- Apply inverse filters via 2D convolutions (low-low, low-high, etc.).
- Sum subbands to reconstruct the image at each level.
- Crop padding to match original dimensions.

### 5. Baseline Comparisons

- **Mean Filter**: Simple averaging with a kernel (e.g., 5x5).
- **Median Filter**: Median over a kernel (e.g., 5x5).
- **BayesShrink**: scikit-image's wavelet denoising with BayesShrink method (soft mode).
- **VisuShrink**: scikit-image's wavelet denoising with VisuShrink method (soft mode, sigma estimated).

### 6. Evaluation Metrics

- **MSE**: Mean Squared Error between original/denoised images.
- **SSIM**: Structural Similarity Index for perceptual quality.
- Bar plots for MSE and SSIM across methods using Pandas and Matplotlib.

[Full Description can be found here](https://github.com/ehsan-honarbakhsh/Denoising-Algorithm-using-Wavelet-Decomposition-and-Reconstruction/blob/main/Docs/Denoising%20Algorithm.pdf)
