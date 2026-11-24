# JPEG Encoder from Scratch

An educational implementation of the main stages of the JPEG image compression algorithm using Python.

## Project Overview

This project implements the core stages of JPEG image compression from scratch to explore how digital images can be transformed into a more compact representation.

The implementation follows the fundamental JPEG compression pipeline, starting with color space conversion and ending with coefficient encoding using Differential Pulse Code Modulation (DPCM) and Run-Length Coding (RLC).

The main objective is to understand the mathematical and computational concepts behind JPEG compression rather than relying on an existing JPEG encoding library.

## JPEG Compression Pipeline

The implementation consists of the following stages:

### 1. RGB to YCbCr Color Space Conversion

The input image is initially represented in the RGB color space.

It is converted into three components:

- **Y:** Luminance (brightness information).
- **Cb:** Blue-difference chrominance.
- **Cr:** Red-difference chrominance.

This separation allows JPEG to process brightness and color information differently.

### 2. Chroma Subsampling (4:2:0)

The chrominance components (Cb and Cr) are downsampled using a 4:2:0 subsampling approach.

Since human vision is more sensitive to brightness variations than color variations, reducing chrominance information can decrease the amount of data while maintaining reasonable visual quality.

### 3. Image Blocking

Each channel is divided into non-overlapping 8×8 blocks.

Before applying the transformation, pixel values are level-shifted by subtracting 128.

This prepares the image blocks for the Discrete Cosine Transform (DCT).

### 4. Discrete Cosine Transform (DCT)

The DCT converts each 8×8 image block from the spatial domain into the frequency domain.

The resulting coefficients represent different spatial frequencies:

- Low-frequency coefficients represent slowly changing image information.
- High-frequency coefficients represent rapid changes and fine details.

This transformation makes it possible to remove less perceptually important information during quantization.

### 5. Quantization

The DCT coefficients are divided by predefined quantization tables.

Two different quantization tables are used:

- Luminance Quantization Table
- Chrominance Quantization Table

Quantization reduces the precision of less important frequency components and introduces the main lossy step in JPEG compression.

### 6. Zigzag Traversal

The quantized 8×8 blocks are converted into one-dimensional sequences using zigzag traversal.

This ordering places low-frequency coefficients near the beginning and high-frequency coefficients toward the end.

It also helps group zero-valued coefficients together, making subsequent run-length encoding more effective.

### 7. Differential Pulse Code Modulation (DPCM)

DPCM is applied to the coefficient sequences to represent differences between corresponding coefficients in consecutive blocks.

This reduces redundancy between neighboring blocks.

### 8. Run-Length Coding (RLC)

Run-Length Coding is used to represent consecutive zero coefficients more compactly.

Instead of storing every zero individually, the implementation represents zero runs together with the following non-zero coefficient.

This produces a more compact intermediate representation.

## Technologies & Libraries

- Python
- NumPy
- SciPy
- Pillow (PIL)

## Project Structure

```text
jpeg_encoder/
│
├── main.py
│
├── chroma_subsampling.py
├── convert_image_to_matrix.py
├── create_block.py
├── dpcm.py
├── quantization.py
├── rlc.py
├── save_output.py
├── zigzag_traverse.py
│
├── test1.png
│
├── result/
│   └── Generated coefficient files
│
├── requirements.txt
└── README.md
```

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/farzadhashemi1/jpeg_encoder.git
```

### 2. Navigate to the project directory

```bash
cd jpeg_encoder
```

### 3. Create a virtual environment (optional)

```bash
python -m venv venv
```

Activate it:

**Linux / macOS:**

```bash
source venv/bin/activate
```

**Windows:**

```bash
venv\Scripts\activate
```

### 4. Install dependencies

```bash
pip install -r requirements.txt
```

## Usage

The current implementation uses `test1.png` as its input image.

Make sure the input image is located in the project root directory.

Run the encoder:

```bash
python main.py
```

After execution, the intermediate encoding results are saved in the `result/` directory.

The generated files include:

- `result_y_dc_coefficient.txt`
- `result_cb_dc_coefficient.txt`
- `result_cr_dc_coefficient.txt`
- `result_y_rlc_coefficient.txt`
- `result_cb_rlc_coefficient.txt`
- `result_cr_rlc_coefficient.txt`

These files contain the encoded coefficient representations for the luminance and chrominance channels.

## Current Implementation Scope

This project focuses on the fundamental mathematical and computational stages of JPEG encoding.

The current implementation generates intermediate encoded coefficient representations rather than a complete JPEG file.

Entropy coding (such as Huffman coding), JPEG headers, and final JPEG bitstream generation are not currently implemented.

## Learning Objectives

This project was developed to gain a deeper understanding of:

- Digital image processing.
- Color space transformations.
- Frequency-domain image representation.
- Discrete Cosine Transform.
- Lossy compression and quantization.
- Chroma subsampling.
- Zigzag scanning.
- Differential encoding.
- Run-length encoding.

