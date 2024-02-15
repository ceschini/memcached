# H.264 transform and coding

# 7.1 Introduction

The first step of H.264 codec, the prediction, is a lossless process that can recreate the exact original data. However, transforming and quantization represent the lossy counterparts of the standard, due to loss of information over the quantization process in order to improve compression performance. Here we will develop these steps.

## 7.2 Transform and quantization

### 7.2.1 The H.264 transforms

In earlier standards for image and video compression such as JPEG, MPEG-2 and MPEG-4 the transform is defined as an equation of a two-dimensional discrete cosine transform (2-D DCT). The implementation of such an equation can lead to approximations to certain irrational multiplication factors, and different implementations can produce different results that in turn leads to mismatches between encoder and decoder, producing a 'drift' effect of cumulative distortion in the decoder output. Even though these standards tried to limit this effect by specifying an accuracy criterion, the error persisted. 

In H.264/AVC, the transform and quantization processes are designed for the mitigation of such errors while providing efficient coding of video data, facilitating low complexity implementations. This is achieved by:


- Using a core transform, an integer transform, that can be carried out using integer or fixed-point arithmetic and,
- integration a normalization step with the quantization process to minimize the number of multiplications required to process a block of residual data.

Also, the scaling and inverse transform processes carried out by the decoder are exactly specified in the standard, removing the chance for mismatch between different transform implementations.

### 7.2.2 Transform processes

#### 7.2.2.1 Overview of transform processes

The inverse transform and re-scaling or 'inverse quantization' processes are defined in the H.264/AVC standard and must be followed by every compliant decoder. The corresponding forward transform and quantization is not standardized, but can be derived from them.

![[inverse-transform-quantization.png]]

In an H.264 encoder, a block of residual coefficients is transformed and quantized. The basic transform, 'core transform', is a 4 x 4 or 8 x 8 **integer transform**, a scaled approximation to the Discrete Cosine Transform, DCT. In certain cases, part of the output of this integer transform is further transformed, 'DC transform', using a Hadamard Transform. The transform coefficients are scaled and quantized.

## 7.3 Black scan orders

A block of transform coefficients are scanned, i.e. converted to a linear array, prior to entropy coding. The scan order is intended to group together significant coefficients, i.e. non-zero quantized coefficients. In a typical block in a progressive frame, non-zero coefficients tend to be clustered around the top left 'DC' coefficient. In this case, zigzag scan order may be the most efficient. After scanning the block in this order, the coefficients are placed in a linear array, in which most of the non-zero coefficients tend to occur near the start of the array.

However, in an interlaced field or a field of a progressive frame converted from interlaced content, vertical frequencies in each block tend to dominate because the field is vertically subsampled from the original scene. This means that non-zero coefficients tend to occur at the top and towards the left side of the block. A block in a field macroblock is therefore scanned in a modified field scan order.

![[zig-zag-scan.png]]

## 7.4 Coding

A coded H.264 stream or an H.264 file consists of a series of coded symbols. These symbols make up the syntax described in Chapter 5 and include parameters, identifiers and delimiting codes, prediction types, differentially coded motion vectors and transform coefficients. The standard specifies several methods for coding the symbols into a binary pattern to be transmitted or stored as part of the bitstream. These methods are as follows:

- **Fixed length code**: A symbol is converted into a binary code with a specified length ($n$ bits).
- **Exponential-Golomb variable length code**: The symbol is represented as an Exp-Golomb codeword with a varying number of bits ($v$ bits). In general, shorter Exp-Golomb codewords are assigned to symbols that occur more frequently.
- **CAVLC**: Context-Adaptive Variable Length Coding, a specially-designed method of coding transform coefficients in which different sets of variable-length codes are chosen depending on the statistics of recently-coded coefficients, using context adaptation.
- **CABAC**: Context-Adaptive Binary Arithmetic Coding, a method of arithmetic coding in which the probability models are updated based on previous coding statistics.

Symbols occuring in the syntax above the slice data level are coded using Fixed Length Codes or Exp-Golomb codes. Symbols at the slice data level and below are coded in one of two ways. If CABAC mode is selected, all of these symbols are coded using CABAC; otherwise, coefficient values are coded using CAVLC and other symbols are coded using fixed length or Exp-Golomb codes.

### 7.4.1 Exp-Golomb Coding

Exponential Golomb codes, Exp Golomb or ExpG, are binary codes with varying lengths constructd according to a regular pattern. Variable length binary codes such as ExpG codes may be used as an efficient way of representing data symbols with varying probabilities. By assigning short codewords to frequently-occurring symbols and long codewords to less common data symbols, the data may be represented in a compressed form.

### 7.4.2 Context Adaptive Variable Length Coding, CAVLC

CAVLC is used to encode residual, scan ordered blocks of transform coefficients. CAVLC is designed to take advantage of several characteristics of quantized coefficient blocks. For example, after prediction, transformation and quantization, blocks are typically sparse, often containing mostly zeros. CAVLC uses run-level coding to compactly represent strings of zeros.

The figure below shows a simplified overview of the CAVLC encoding process. A block of coefficients is scanned using zigzag or field scan and converted into a series of variable length codes (VLCs). Certain VLC tables are chosen based on local statistics, such as the number of non-zero coefficients in neighboring blocks and the magnitude of recently-coded coefficients.

![[cavlc.png]]

CAVLC encoding of a 4 x 4 block of transform coefficients proceeds as follows.

**1. Encode the number of coefficients and trailing ones (coeff_token)**

The first VLC, coeff_token, encodes both the total number of non-zero coefficients (Total-Coeffs) and the number of trailing +/- 1 values (T1). Total-Coeffs can range from 0 (no coefficients in the 4 x 4 block), to 16, i.e. 16 non-zero coefficients. T1 can take values from 0 to 3. If there are more than three trailing +/- 1s, only the last three are treated as 'special cases' and any others are coded as normal coefficients.

**2. Encode the sign of each T1**

For each trailing +/- T1 signalled by coeff_token, the sign is encoded with a single bit, 0 = +, 1 = -, **in reverse order**, starting with the highest-frequency T1.

**3. Encode the levels of the remaining non-zero coefficients**

The **level**, i.e. the sign and magnitude, of each remaining non-zero coefficient in the block is encoded **in reverse order**, starting with the highest frequency and working back towards the DC coefficient. The choice of VLC to encode each successive level is context adaptive and depends on the magnitude of the previous coded level.

**4. Encode the total number of zeros before the last coefficient**

TotalZeros is the sum of all zeros preceding the highest non-zero coefficient in the reordered array and is coded with a VLC. The reason for sending a separate VLC to indicate TotalZeros is that many blocks contain zero coefficients at the start of the array and, as will be seen later, this approach means that zero-runs at the start of the array need not be encoded.

**5. Encode each run of zeros**

The number of zeros preceding each non-zero coefficient (run_before) is encoded in reverse order. A run_before parameter is encoded for each non-zero coefficient, starting with the highest frequency, with two exceptions:

1. If there are no more zeros left to encode
2. If it is not necessary to encode run_before for the final or lowest frequency non-zero coefficient.

### 7.4.3 Context Adaptive Binary Arithmetic Coding, CABAC

CABAC is an optional entropy coding mode available in Main and High profiles. CABAC achieves good compression performance through:

- (a) selecting probability models for each syntax element according to the element's context,
- (b) adapting probability estimates based on local statistics and
- using arithmetic coding rather than variable-length coding.

The definition of the decoding process is designed to facilitate low-complexity implementations of arithmetic encoding and decoding. Overall, CABAC can provide improved coding efficiency compared with CAVLC at the expense of greater computational complexity on some processing platforms.

