# A Denoising Image Algorithm Using Discrete Wavelet Transform in Python (from Scratch, Without PyWavelets):

I developed a sophisticated image denoising algorithm entirely from scratch in Python, without relying on external libraries like PyWavelets. The project consists of four main steps:

1-PGM Image Reader: Implemented a custom reader for PGM (Portable GrayMap) image files from scratch in Python.

2-Wavelet Decomposition and Denoising: Built the discrete wavelet transform (DWT) algorithm using the Haar wavelet, without using any specialized libraries. I applied a denoising strategy based on statistical analysis of the detail coefficients obtained from the wavelet decomposition.

3-Wavelet Reconstruction and Evaluation: Reconstructed the denoised image from the modified coefficients and evaluated its quality by comparing it to the original image.

4-Benchmarking: Assessed the performance of my algorithm by benchmarking it against several commonly used denoising methods.

## The full description of the algorithm can be found in this 
([https://raw.githubusercontent.com/<username>/<repo>/<branch>/<path-to-pdf>](https://github.com/ehsan-honarbakhsh/Denoising-Algorithm-using-Wavelet-Decomposition-and-Reconstruction/blob/main/Docs/Denoising%20Algorithm.pdf))
