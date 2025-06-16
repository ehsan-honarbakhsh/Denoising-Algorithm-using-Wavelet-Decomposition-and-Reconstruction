# Denoising Algorithm using Wavelet Decomposition and Reconstruction

Wavelet Decomposition and Reconstruction

This project aims to implement several algorithms, starting from reading a grayscale image in
PGM format to filtering and denoising a noisy image.
In the first phase, an algorithm is developed from scratch to read a PGM file and convert it into a
matrix, which will be used as input for subsequent algorithms. Next, a wavelet decomposition
algorithm using Haar filtering is designed to extract the coefficients of the image. This is
followed by the implementation of the reverse process—wavelet reconstruction—to recreate the
original image from its frequency coefficients.
Finally, a denoising algorithm leveraging the developed wavelet decomposition and
reconstruction processes is created. This algorithm is applied to reduce noise in the noisy image,
and its efficiency is evaluated by comparing the denoised image to the original.
