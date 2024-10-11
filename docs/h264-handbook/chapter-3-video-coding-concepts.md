# Video Coding Concepts

## 3.1 Introduction

**compress _vb._: to squeeze together or compact into less space; condense**

**compression _noun_: the act of compression or the condition of being compressed**

Compression is the act or process of compacting data into a smaller number of bits. Video compression (video coding) is the process of converting digital video into a format for transmission or storage, whilst typically reducing the number of bits.

Compression involves a complementary pair of systems, a compressor (encoder) and a decompressor (decoder). The encoder converts the source data into a compressed form occupying a reduced number of bits, prior to transmission or storage, and the decoder converts the compressed form back into a representation of the original video data.

Data compression is achieved by removing **redundancy**, the content of data unnecessary for its faithful reproduction. Lossless methods reduce the statistical redundancy in order to reconstruct exact copies of the original data. This method has higher quality at the expense of lower compression rates. Lossy methods reduce the subjective redundancy, elements of the image that are unnecessary for the reconstruction of the visually perceived image. This means that these methods can leverage the way the human visual system (HSV) works in order to reduce information that can pass without notice. 

The redundancy can be temporal or spatial. In the temporal domain, video codecs explore the correlation between sequential frames of video captured around the same timespan. In the spatial domain, video codecs explore the correlation between neighboring pixels that share the same aspects of luma and chroma, due to normally belonging to the same object (not including edges).

The H.264 Advanced Video Coding standard shares a number of common features with other popular compression formats. These formats are based upon a CODEC model that uses predictions and/or block-based motion compensation, transform, quantization and entropy coding.

## 3.2 Video CODEC

A video CODEC encodes a source image or video sequence into a compressed form and decodes this to produce a copy or approximation of the source sequence. The CODEC represents the original video sequence by a **model**, an efficiently reduced representation of the source information that can be reconstructed into the original. The ideal is to represent the information with the highest fidelity while using the least amount of information possible. These two aspects are commonly adversarial, if one grows larger the other one gets smaller, this can be translated into _a higher quality image results in a lower compression rate_.

A video encoder consists of three main functional units: a **prediction model**, a **spatial model** and an **entropy model**. 

The input for the prediction model is the original 'raw' video sequence. It attempts to reduce redundancy by exploiting similarities between neighboring video frames or image samples, _typically by constructing a prediction of the current video frame or block of video data_. This prediction is created by spatial extrapolation from neighboring image samples (group of pixels), **intra prediction**, or by compensating for differences between the frames, **inter or motion prediction**. The output of the prediction model is twofold: a residual frame, created by subtracting the prediction frame from the original, and a set of parameters describing the kind of prediction used.

The residual frame forms the input to the spatial model, which makes use of similarities between local samples in the residual frame to reduce spatial redundancy. In H.264AVC, this is done by applying a transform to the frame, along with a quantization to drop irrelevant data. First, the transform reduces the image frame into a set of coefficients, which in turn are quantized into only relevant values for the reconstruction. The set of quantized coefficients are the output of the spatial model.

Finally, these coefficients together with the set of parameters from the prediction model are coded into a bitstream using the entropy encoder. This removes statistical redundancy in the data, for example, _representing commonly occurring vectors and coefficients by short binary codes_. The compressed bitstream is then transmitted or stored, and consists of coded prediction parameters, coded residual coefficients and header information.

Over at the decoder part, the received encoded bitstream is decoded by an entropy decoder, after which the spatial model is decoded to reconstruct the residual frame. The decoder uses the prediction parameters, _together with previously decoded image pixels_, to create a prediction of the current frame, and the frame itself is reconstructed by adding the residual frame to this prediction.

![[docs/img/encoder-drawing.jpg]]

## 3.3 Prediction model

The prediction model receives the original frame and is responsible for removing redundancies in order to generate a residual frame with just the differences between the original and the predicted frame. To do so, the prediction model explores temporal and spatial components. Temporal components are the relation between previously coded frames, and spatial components are the relation between image blocks inside the same frame. The more accurate the prediction, the less energy is contained in the residual.

### 3.3.1 Temporal prediction

The predicted frame is created from one or more previous or future frames, called **reference frames**. The model explores motion information present in said frames in order to improve its prediction accuracy.

#### 3.3.1.1 Prediction from the previous frame

This is the simplest method of temporal prediction. The current frame is predicted from the previous frame. The residual is formed by subtracting the current frame from the previous frame, but this method is not efficient, because a lot of energy is still present in the residual frame. Much of this energy is due to objects moving between frames, and a way to improve this is compensating motion.

#### 3.3.1.2 Changes due to motion

Causes of changes between video frames include motion, uncovered regions and lighting changes. Types of motion include rigid object motion, such as a moving car, deformable object motion, such as a person speaking, and camera motion, such as panning, tilt, zoom and rotation. An uncovered region is when a moving object suddenly reveals unseen regions. Motion correspond to pixel movement between frames, and as such it is possible to estimate the trajectory of each of these pixels, producing a field of pixel trajectories known as the **optical flow**.

With an accurate optical flow, it is possible to form an accurate prediction of most of the pixels of the current frame by moving each pixel from the reference frame along the direction of its optical flow vector. Sadly, this is not feasible for the decoder due to the large amount of processing required to iterate each pixel and shift it towards its vector.

#### 3.3.1.3 Block-based motion estimation and compensation

This simple method applies motion estimation only to regions of the frame chosen by comparing the energy of the residual between the previous and current region. For each region block of the current frame, a matched block from the previous frame is chosen, based on the lower energy residual. This chosen block is then the predictor for that current frame block, and the residual of this block is passed along with the offset information to the decoder, so that it can find exactly what region block corresponds to that regional residual block.

_The process of finding the best region block is called **motion estimation**_.

_The process of subtracting the current block from the previous block to form a residual block is called **motion compensation**_.

_The offset information with the location of the current and the predictor block is called the **motion vector**_.

The decoder receives the residual block and the motion vector. It then applies the motion vector information to grab the predictor region block, and adds it to the residual in order to reconstruct the original block.

This method is very popular due to its simple implementation and computation. The fact that it applies rectangular blocks to the image provides the use of common block-based image transforms, such as the Discrete Cosine Transform. However, "real world" objects rarely have neatly defined rectangular edges, and as such this method won't be optimal for regions that involves, for example, deformable objects, rotation, warping and complex motion such as clouds and fog. Despite all that, block-based motion compensation is the basis of the temporal prediction model used by all current video coding standards.

#### 3.3.1.4 Motion compensated prediction of a macroblock

The **macroblock**, corresponding to a 16 x 16 pixel region of a frame, is the basic unit for motion compensated prediction in a number of important visual coding standard, including H.264/AVC.

##### Motion estimation:

Motion estimation of a macroblock involves finding a 16 x 16 sample region in a reference frame that closely matches the current macroblock. The search for the best match is performed inside a search area, that expands from the centered position of the current macroblock.

##### Motion compensation:

The lume and chroma samples of the selected 'best' matching region in the reference frame are subtracted from the current macroblock to produce a residual macroblock that is encoded and transmitted together with a _motion vector describing the position of the best matching region relative to the current macroblock position_.

![[docs/img/motion-estimation.png]]

There are a lot of variations of the motion estimation and motion compensation (MEMC) explained above. For example, future frames might be more meaningful to the prediction of a macroblock, and then should be encoded before the current one, giving an out-of-order transmission. Sometimes there is a significant change in motion between two frames, and as such there is no gain in using motion compensation, in such cases the CODEC might use intra coding instead. Not every moving object will be encompassed in a 16 x 16 pixel size block, and dynamic sizes might be applied to better envelop objects on different scenes. 

#### 3.3.1.5 Motion compensation block size

Smaller motion compensation block sizes can produce better motion compensation results. However, a smaller block size leads to increased complexity, with more search operations to be carried out, and an increase in the number of motion vectors that need to be transmitted. Sending more vectors means sending more bits of information, and this overhead might not be beneficial compared to the residual size gains. Because of that, the best approach is to adapt the block size to the image characteristics. In large homogeneous regions, a big macroblock can capture all the information contained within, on the other hand, in small edgy and complex regions a small macroblock size might be necessary in order to attend to the complex shapes and as such reduce the residual energy while not overheading it too much.

#### 3.3.1.6 Sub-pixel motion compensation

Objects might not always move by integer valued pixels. And as such, sometimes it is better to explore regions with _sub-pixel_ values, by interpolating half-pixel positions. Sub-pixel motion estimation and compensation involves searching sub-pixel interpolated positions as well as integer-pixel positions and choosing the position that gives the best match and minimizes the residual energy.

On the example below, the first stage of the motion estimation is to find the best match on the integer pixel grid (circles). The encoder searches the half-pixel positions immediately next to this best match (squares) to see whether the match can be improved and, if required, the quarter-pixel positions next to the best half-pixel position (triangles) are then search. The final match, at an integer, half-pixel or quarter-pixel position, is subtracted from the current block or macroblock.

![[docs/img/sub-pixel-motion-estim.png]]

In general, 'finer' interpolation provides better motion compensation performance, producing a smaller residual at the expense of increased complexity. The performance gain tends to diminish as the interpolation steps increase.

There is a trade-off in compression efficiency associated with more complex motion compensation schemes.

### 3.3.2 Spatial model: intra prediction

In the intra prediction mode, macroblocks of the current frame are predicted based on the already encoded macroblocks within the same frame. By doing so, it guarantees that only information already available for the decoder is used. This approach leverages the assumption that neighboring pixels are commonly correlated, and as such it will use this neighboring blocks of pixels in order to perform the prediction. Once the prediction has been generated, it is subtracted from the current block, similarly of that of inter prediction. The residual is transformed and encoded, together with an indication of how the intra coding was performed.

![[docs/img/intra-preds-available-samples.png]]

![[docs/img/intra-preds-process.png]]

## 3.4 Image model

Natural images are often difficult to compress in their original form because of the high correlation between neighboring image samples. Efficient motion compensation or intra prediction reduces local correlation in the residual, making it easier to compress than the original video frame. The function of the **image model** is to further decorrelate image or residual data and to convert it into a form that can be efficiently compressed using an entropy coder.

Practical image models typically have three main components, _transformation_ to decorrelate and compact the data, _quantization_ to reduce the precision of the transformed data and _reordering_ to arrange the data to group together significant values.

### 3.4.1 Predictive image coding

Predictive image coding such as motion compensation is a method that combines previous or neighboring frames/regions in such a way that an approximation of the current frame or region is predicted. This predicted sample is then subtracted from the current region in order to create a residual.

If the prediction is successful, the energy in the residual is lower than in the original frame and the residual can be represented with fewer bits.

In order to further reduce the residual, lossy process such as quantization are employed. This means that the decoded pixels may not be identical to the original ones due to losses during encoding. This could lead to a cumulative mismatch or 'drift' between the encoder and decoder. _To avoid this, the encoder should itself decode the residual and reconstruct each pixel_. In this way, both encoder and decoder use the same prediction P(X) and drift is avoided.

### 3.4.2 Transform coding

The purpose of the transform stage in an image or video CODEC is to convert image or motion-compensated residual data into another domain, _the transform domain_. The choice of transform depends on a number of criteria:

1. Data in the transform domain should be decorrelated, i.e. separated into components with minimal inter-dependence, and compact, i.e. most of the energy in the transformed data should be concentrated on a small number of values.
2. The transform should be reversible.
3. The transform should be computationally tractable, e.g. low memory requirement, achievable using limited-precision arithmetic, low number of arithmetic operations, etc.

Transforms tend to fall into two categories, block-based and image-based. Block-based transforms operates on blocks of N x N image or residual samples, they have low memory requirements and are well suited to compression of block-based motion compensation residuals, but tend to suffer from artifacts at block edges ('blockiness'). Examples of block-based transforms include the Karhunen-Loeve Transform (KLT), Singular Value Decomposition (SVD) and the ever-popular Discrete Cosine Transform (DCT) and its approximations.  Image-based transforms are employed over the hole image or over a large portion of it, called a 'tile'. They have a better compression rate at the cost of higher memory requirements. The most famous one is the Discrete Wavelet Transform, DWT, or just 'wavelet'.

#### 3.4.2.2 DCT

The idea of using the DCT is to transform pixels into coefficients. These coefficients can be dropped by a defined threshold using quantization. Even with a lower number of coefficients,, there is still the ability to recover the original image with only some loss of data, hopefully just the insignificant parts of it.

The Discrete Cosine Transform (DCT) operates on **X**, a block of N x N samples, typically image samples or residual values after prediction, to create **Y**, an N x N block of coefficients. The action of the DCT and its inverse, the IDCT can be described in terms of a transform matrix **A**.

The forward DCT (FDCT) of an N x N sample block is given by:

$Y = AXA^T$

and the inverse DCT (IDCT) by:

$X = A^TYA$

Where $X$ is a matrix of samples, $Y$ is a matrix of coefficients and $A$ is an N x N transform matrix. The elements of $A$ are:

![[docs/img/dct-A-matrix.png]]

**_Example: N = 4_**

The transform matrix $A$ for a 4 x 4 DCT is:

![[docs/img/dct-A-4x4-matrix.png]]

The cosine function is symmetrical and repeats after 2$\pi$ radians, and hence $A$ can be simplified to:

![[docs/img/simplified-dct-a-matrix.png]]

**The output of a 2-dimensional FDCT is a set of N x N coefficients representing the image block data in the DCT domain, which can be considered as 'weights' of a set of standard _basis patterns_.**

Any image block may be reconstructed by combining all N x N basis patterns, with each basis multiplied by the appropriate weighting factor (coefficient).

**_Example 1 Calculating the DCT of a 4 x 4 block_**

Let $X$ be a 4 x 4 block of samples from an image:

![[docs/img/dct-example-x-block.png]]

The Forward DCT of $X$ is given by: $Y = AXA^T$. The first matrix multiplication, $Y'=AX$, corresponds to calculating the 1-dimensional DCT of each **column** of $X$. For example, $Y'00$ is calculated as follows:

![[docs/img/dct-y00-calc.png]]

The complete result of the column calculations is:

![[docs/img/dct-column-clac.png]]

Carrying out the second matrix multiplication, $Y = Y'A^T$, is equivalent to carrying out a 1-D DCT on each **row** of $Y'$.

![[docs/img/dct-row-calc.png]]

Note that the order of the row and column calculations does not affect the final result.

**_Example 2 Image block and DCT coefficients_**

Figure 3.30 shows an image with a 4 x 4 block selected, and Figure 3.31 shows the block in close-up, together with the DCT coefficients. The advantage of representing the block in the DCT domain is not immediately obvious, since there is no reduction in the amount of data; instead of 16 pixels values, we need to store 16 DCT coefficients. The usefulness of the DCT becomes clear when the block is reconstructed from a subset of the coefficients.

![[docs/img/image-with-4x4-block.png]]

![[docs/img/img-block-dct-close-up.png]]

Setting all the coefficients to zero except the most significant, coefficient (0,0) described as the 'DC' coefficient, and performing the IDCT gives the output block shown in Figure 3.32 (a) below, and it is the mean of the original pixel values. Calculating the IDCT of the two most significant coefficients gives the block shown in Figure 3.32 (b).

Adding more coefficients before calculating the IDCT produces a progressively more accurate reconstruction of the original block, and by the time five coefficients are included (Figure 3.32 (d)), the reconstructed block is a reasonably close match to the original. _Hence, it is possible to reconstruct an approximate copy of the block from a subset of the 16 DCT coefficients. Removing the coefficients with insignificant magnitudes, for example by **quantization**, enables image data to be represented with a reduced number of coefficient values at the expense of some loss of quality_.

#### 3.4.2.3 Wavelet

The 'wavelet transform' that is popular in image compression is based on sets of filters with coefficients that are equivalent to discrete wavelet functions. The basic operation of the transform is as follows: A pair of filters is applied to the signal to decompose it into a low frequency band (L) and a high frequency band (H). Each band is subsampled by a factor of two, so that the two frequency bands each contain N/2 samples. With the correct choice of filters, this operation is reversible.

This process can be applied to a 2-D signal such as an intensity image (grayscale). First, the image is filtered on the x dimension by a low-pass and a high-pass filter. The resulting images are then downsampled by a factor of two, followed by another low and high pass filtering, but now on the y dimension. A final downsampling is applied in order to produce four sub-band images: LL, HL, LH, HH.

![[docs/img/wavelets-process.png]]

These four 'sub-band' images can be combined to create an output image with the same number of samples as the original. 

LL is the original image, low-pass filtered in horizontal and vertical direction and subsampled by a factor of two. HL is high-pass filtered in the vertical direction and contains residual vertical frequencies, LH is high-pass filtered in the horizontal direction and contains residual horizontal frequencies and HH is high-pass filtered in both horizontal and vertical directions.

Between them, the four sub-band images contain all the information present in the original image, but the sparse nature of them makes it amenable to compression.

![[docs/img/wavelet-sub-bands.png]]

In an image compression algorithm, the LL sub-image can be filtered again in order to produce another level of decomposition, and repeated a set number of times. The high-pass filtered images contain information of low impact for the image and can then be dropped in order to further compress the image.

### Quantization

A quantizer maps a signal with a range of values X to a quantized signal with a reduced range of values Y. It should be possible to represent the data with fewer bits, since the range between them is smaller. There are **scalar quantizer** and **vector quantizer**. The first maps a single input from the original range into a single quantized output on the new scale. The second maps a group of inputs (vector) to a group of output on the quantized space.

#### 3.4.3.1 Scalar quantization

A simple example of scalar quantization is the process of rounding a fractional number to the nearest integer. The process is lossy (not reversible) since it is not possible to determine the exact value of the original fractional number from the rounded integer.

A more general example of a uniform quantizer is:

![[docs/img/uniform-quantizer.png]]

Where $QP$ is a quantization 'step size'. The quantized output levels are spaced at uniform intervals of $QP$.

In the video compression pipeline, quantizer is usually split into a forward quantizer $FQ$ in the encoder, and an inverse quantizer or rescaler($IQ$) at the decoder. The most important parameter is the **step size** QP between successive re-scaled values. The bigger the step, the bigger the compression rate, but also the information and quality loss. The inverse is also true. And this is due to the range of inputs that will fall into the same levels of quantized outputs, and as such will both reduce the amount of bits and the fine-grained details contained in between.

Quantization is usually applied to the transformed coefficients of DCT or wavelet. These coefficients contain several insignificant near-zero values, and can be zeroed down by the quantization in order to improve compression while still maintaining quality. The output of a forward quantizer is therefore typically a 'sparse' array of quantized coefficients, mainly containing zeros.

#### 3.4.3.2 Vector quantization

A vector quantizer maps a set of input data, such as a block of image samples, to a single value (codeword). At the decoder, each codeword maps to an approximation of the original set of input data, a 'vector'. The set of vectors is stored at the encoder and decoder in a _codebook_. A typical application of vector quantization to image compression is as follows:

1. Partition the original image into regions such as N x N pixel blocks.
2. Choose a vector from the codebook that matches the current region as closely as possible.
3. Transmit an index that identifies the chosen vector to the decoder.
4. At the decoder, reconstruct an approximate copy of the region using the selected vector.

The quantization can be applied in the image or spatial domain, to motion compensated data and to transformed data. Key issues with this approach are the design of the codebook and the efficient search of the codebook for the optimal codeword.

### 3.4.4 Reordering and zero encoding

In a transform-based image or video encoder, the output of the quantizer is a sparse array containing a few non-zero coefficients and a large number of zero-valued coefficients. _Re-ordering to group together non-zero coefficients and efficient encoding of zero coefficients are applied prior to entropy coding._

#### 3.4.4.1 DCT

**_Coefficient distribution_**

The significant DCT coefficients of a block of image or residual samples are typically the 'low frequency' positions around the DC (0, 0) coefficient.

The figure below plots the probability of non-zero DCT coefficients at each position in an 8 x 8 block in a QCIF residual frame. The non-zero DCT coefficients are clustered around the top-left (DC) coefficient, and the distribution is roughly symmetrical in the horizontal and vertical directions.

![[docs/img/dct-coeff-dist.png]]

**_Scan_**

After quantization, the DCT coefficients for a block are reordered to group together non-zero coefficients, enabling efficient representation of the remaining zero-valued quantized coefficients. The optimum re-ordering path or scan order depends on the distribution of non-zero DCT coefficients. For a typical frame block with a distribution similar to Figure 3.39 above, a suitable scan order is a zigzag starting from the DC or top-left coefficient. Starting from the DC, the coefficients are stored in a 1-D array. Non-zero coefficients tend to be grouped together at the start of the re-ordered array, followed by long sequences of zeros.

**_Run-Level Encoding_**

It is expected that non-zero coefficients are grouped together at the start of the array. In order to efficiently compress the large amount of zeroes present between the non-zero values, a pair of (run-level) values are formed in order to represent how many zeroes there are (**runs**) between a non-zero value (**level**). 

**_Example_**

1. Input array: 16, 0, 0, -3, 5, 6, 0, 0, 0, 0, -7, ...
2. Output values: (0, 16), (2, -3), (0, 5), (0, 6), (4, -7)...
3. Each of these output values (a run-level pair) is encoded as a separate symbol by the entropy encoder.

To signal the end of non-zero values, a special symbol is used. This is required because the re-ordered non-zero coefficients are probably contained in the first positions of the array, and there is no need to keep reading it after the last one. In a 2-D run-level encoding, another output is required in order to contain the **last** flag. If a 3-D run-level-last encoding is used, each output value will have the flag information signaling if it is the last one or not, such as (0, 16, 0), ..., (4, -7, 1) on the example above.

#### 3.4.4.2 Wavelet

**_Coefficient distribution_**

Many coefficients, in higher sub-bands, are near zero and may be quantized to zero without significant loss of image quality. Non-zero coefficients tend to be related to structures in the image. When a coefficient in a lower-frequency sub-band is non-zero, there is a strong probability that coefficients in the corresponding position in higher-frequency sub-bands will also be non-zero. 

We may consider a 'tree' of non-zero coefficients, starting with a 'root' in a low-frequency sub-band. A single coefficient in the LL band of layer 1 has one corresponding coefficient in each of the other bands of layer 1, meaning that these four coefficients correspond to the same region in the original image. The layer 1 coefficient maps to four corresponding child coefficients in each sub-band at layer 2, because layer 2 have twice the resolution of layer 1.

![[docs/img/wavelet-coef-children.png]]

**_Zerotree encoding_**

It is desirable to encode the non-zero wavelet coefficients as compactly as possible prior to entropy coding. An efficient way of achieving this is to encode each tree of non-zero coefficients starting from the lowest or root level of the decomposition. 

Starting by the coefficient at the lowest layer, encode it and follow to its child coefficients at the next layer up. Keep doing this until a zero-valued coefficient is reached. Considering that children of a zero coefficient are also zero, we can represent the remaining children as a single code symbol, identifying a tree of zeros (**zerotree**). Over at the decoder side, the processing of non-zero coefficients is performed until a zerotree is reached, by then the decoder can just fill the remaining children as zeroes. This is the basis of the **embedded zero tree** (EZW) method of encoding wavelet coefficients.

## 3.5 Entropy coder

The entropy encoder converts a series of symbols representing elements of the video sequence into a compressed bitstream suitable for transmission or storage. Input symbols may include quantized transform coefficients run-level or zerotree encoded, motion vectors with integer or sub-pixel resolution, marker codes that indicate a resynchronization point in the sequence, macroblock headers, picture headers, sequence headers and supplementary information, 'side' information that is not essential for correct decoding.

### 3.5.1 Predictive coding

Certain symbols are highly correlated in local regions of the picture. For example, the average or DC value of neighboring intra-coded blocks of pixels may be very similar, neighboring motion vectors may have similar x and y displacements and so on. _Coding efficiency can be improved by predicting **elements** of the current block or macroblock from previously-encoded data and encoding the difference between the prediction and the actual value_.

The motion vector for a block or macroblock indicates the offset to a prediction reference in a previously encoded frame. Vectors for neighboring blocks or macroblocks are often correlated because object motion may extend across large regions of a frame. As such, compression of the motion vector field may be improved by predicting each motion vector from previously encoded vectors. For example, in the figure below, a simple prediction for the vector of the current macroblock $X$ is the horizontally adjacent macroblock $A$. Alternatively, three or more previously-coded vectors may be used to predict the current vector. The difference between the predicted and actual motion vector, the Motion Vector Difference or MVD, is encoded and transmitted.

### 3.5.2 Variable-length coding

A variable-length encoder maps input symbols to a series of codewords, variable length codes or VLCs. Each symbol maps to a codeword and codewords may have varying length but must each contain an integral number of bits. Frequently-occurring symbols are represented with short VLCs, whilst less common symbols are represented with long VLCs. Over a sufficiently large number of encoded symbols, this leads to compression of the data.

#### 3.5.2.1 Huffman coding

Huffman coding assigns a VLC to each symbol based on the probability of occurrence of different symbols. According to the original scheme proposed by Huffman in 1952, it is necessary to calculate the probability of occurrence of each symbol and to construct a set of variable length codewords.

**_Example 1: Huffman coding, Sequence 1 motion vectors_**

The motion vector difference (MVD) for video sequence 1 must be encoded. The table below lists the probability of the most commonly-occurring motion vectors and their **information content**, $log_2(1/p)$. To achieve optimum compression, _each value should be represented with exactly $log_2(1/p)$ bits_. '0' is the most common value, and the probability drops for larger motion vectors. This distribution is representative of a sequence containing **moderate motion**.

![[docs/img/probs-mvds-huffman.png]]

_1. Generating the Huffman code tree_

To generate a Huffman code table for this set of data, the following iterative procedure is carried out:

1. Order the list of data in increasing order of probability.
2. Combine the two lowest-probability data items into a 'node' and assign the joint probability of the data items to this node.
3. Re-order the remaining data items and node(s) in increasing order of probability, and repeat step 2.

The procedure is repeated until there is a single 'root' node that contains all other nodes and data items listed 'beneath' it. This procedure is illustrated in the figure below.

![[docs/img/huffman-tree-ex1.png]]

_2. Encoding_

Each 'leaf' of the binary tree is mapped to a variable-length code. To find this code, the tree is traversed from the root node, D in this case, to the leaf or data item. For every branch, a 0 or 1 is appended to the code.

Encoding is achieved by transmitting the appropriate code for each data item. Once the tree is generated, the codes may be stored in a look-up table.

High probability data items are assigned short codes, e.g. 1 bit for the most common vector '0'. The lengths of the Huffman codes, each an _integral_ number of bits, do not match the ideal lengths given by $log_2(1/p)$. No code contains any other code as a prefix, which means that, reading from the left-hand bit, each code is uniquely decodable.

![[docs/img/huff-codes-seq1.png]]

_3. Decoding_

In order to decode the data, the decoder must have the look-up table of symbols, or the probabilities of motion vectors prior to transmission. Each uniquely-decodeable code is converted back to the original data, for example, sending 0111000 can be decoded as:

1. 011 is decoded as (1)
2. 1 is decoded as (0)
3. 000 is decoded as (-2).

If the probability distributions are accurate, Huffman coding provides a relatively compact representation of the original data. However, to achieve optimum compression, a separate code table is required for each sequence due to different probability distributions. 

#### 3.5.2.2 Pre-calculated Huffman

There are two main disadvantages with Huffman coding. The first one is that the code tree must be known to both encoder and decoder, and as such must be transmitted prior to the data. The second one is that in order to generate the tree, the probability distribution of the hole encoded video sequence must be calculated, and this could generate unacceptable delays. For these reasons, _image and video coding standards define sets of codewords based on the probability distributions of 'generic' video material_. 

As an example, MPEG-4 Visual (Simple Profile) have pre-calculated VLC tables for Transform Coefficients (TCOEF) and Motion Vector Difference (MVD). For TCOEF, MPEG-4 Visual uses 3-D coding of quantized coefficients in which each codeword represents a combination of (run, level, last). A total of 102 specific combinations of (run, level, last) have VLCs assigned to them. And as for MVD, differentially coded motion vectors are each encoded as a pair of VLCs, one for the x-component and one for the y-component.

These code tables are clearly similar to 'true' Huffman codes since each symbol is assigned a unique codeword, common symbols are assigned shorted codewords and, within a table, no codeword is the prefix of any other codeword. The main difference from 'true' Huffman coding is that codewords are pre-calculated based on 'generic' probability distributions.

### 3.5.3 Arithmetic coding

The variable length coding schemes described in the section above share the fundamental disadvantage that assigning a codeword containing an integral number of bits to each symbol is sub-optimal, since the optimal number of bits for a symbol depends on the information content and is usually a fractional number.

Arithmetic coding provides a practical alternative to Huffman coding that can more closely approach theoretical maximum compression ratios. An arithmetic encoder converts a sequence of data symbols into a single fractional number and can approach the optimal fractional number of bits required to represent each symbol.

For each symbol present in a sequence, a slice of the range of fractional numbers between 0 and 1 is assigned, based on the probability of that symbol to occur. Following the same example above, when we try to encode the motion vector values (-2, -1, 0, 1, 2) we will assign each vector a **sub-range** within the range 0.0 to 1.0, depending on its probability of occurrence. In this example, -2 has the probability of 0.1 and is given the subrange 0-0.1, meaning that the first 10 percent of the total range will be reserved to this symbol. -1 has a probability of 0.2 and is given the next 20 percent of the total range, that is the subrange 0.1-0.3.

After assigning a sub-range to each vector, the total range 0-1.0 has been divided amongst the data symbols (the vectors) according to their probabilities.

![[docs/img/mv-probs-subrange.png]]

![[docs/img/sub-range-example.png]]

_Encoding procedure_ for vector sequence (0, -1, 0, 2).

1. Set the initial range: 0 → 1.0
2. Find the sub-range of the first symbol (0): 0.3 → 0.7
3. Set this sub-range as the new range
4. Find the sub-range of the next symbol (-1): 0.1 → 0.3
5. Set the new range as the sub-range of the actual range: 
	1. Actual range: 0.3 → 0.7
	2. New range: 0.34 → 0.42
6. Find the sub-range of the next symbol (0): 0.3 → 0.7
7. Set the new range as the sub-range of the actual range:
	1. Actual range: 0.34 → 0.42
	2. New range: 0.364 -> 0.396
8. Find the sub-range of the next symbol (2): 0.9 -> 1.0
9. Set the new range as the sub-range of the actual range:
	1. Actual range: 0364 -> 0.396
	2. New range: 0.3928 → 0.396
10. Define a number that falls between the final range as the chosen encoding number.
	1. Choose 0.394

After finding the final sub-range, any number that falls between it can be chosen as the final symbol transmitted. It can be represented as a fixed-point fractional number using 9 bits, so our data sequence (0, -1, 0, 2) is compressed to a 9-bit quantity.

![[docs/img/arith-coding-sub-range-example.png]]

_Decoding procedure_

1. Set the initial range: 0 → 1.0
2. Find the sub-range in which the received number falls. This indicates the first data symbol.
	1. Received number: 0.394
	2. Current range: 0 → 1.0
	3. Number is inside the 0.3 → 0.7 sub-range
	4. Decoded symbol is 0
3. Set the new range to this sub range
	1. Current range: 0 → 1.0
	2. New range: 0.3 → 0.7
4. Find the sub-range **of the new range** in which the received number falls. This indicates the second data symbol.
	1. Received number: 0.394
	2. Current range: 0.3 → 0.7
	3. **_If 0.3 is 0% and 0.7 is 100%, then 0.394 is inside the 10 to 30%_**
	4. Decoded symbol is -1
5. ***New range is 10 to 30% of the last range***:
	1. Current range: 0.3 → 0.7
	2. New range: 0.34 → 0.42
6. Find the sub-range of the new range in which the received number falls. This indicates the third data symbol.
	1. Received number: 0.394
	2. Current range: 0.34 → 0.42
	3. **_If 0.34 is 0% and 0.42 is 100%, then 0.394 is inside the 30 to 70%_**
	4. Decoded symbol is 0
7. ***New range is 30 to 70% of the last range***:
	1. Current range: 0.34 → 0.42
	2. New range: 0.364 → 0.396
8. Find the sub-range of the new range in which the received number falls. This indicates the fourth data symbol.
	1. Received number: 0.394
	2. Current range: 0.364 -> 0.396
	3. ***If 0.364 is 0% and 0.396 is 100%, then 0.394 is inside the 90 to 100%***
	4. Decoded symbol is 2

The principal advantage of arithmetic coding is that the transmitted number, 0.394 in this case, is not constrained to an integral number of bits for each transmitted data symbol. The optimal compression for this video sequence is 8.28bits, arithmetic coding achieved 9 bits, a value that a scheme using an integral number of bits for each data symbol such as Huffman coding could not achieve.

#### 3.5.3.1 Context-based Arithmetic Coding

Successful entropy coding depends on accurate models of symbol probability. Context-based Arithmetic Encoding (CAE) uses local spatial and/or temporal characteristics to estimate the probability of a symbol to be encoded.

## 3.6 The hybrid DPCM/DCT video CODEC model

The major video coding standards released since the early 1990s have been based on the same generic design or model of a video CODEC that incorporates a motion estimation and compensation first stage, sometimes described as DPCM, a transform stage and an entropy encoder. The model is often described as a hybrid DPCM/DCT CODEC. Any CODEC that is compatible with H.261, H.263, MPEG-1, MPEG-2, MPEG-4 Visual, H.264/AVC or VC-1 has to implement a similar set of basic coding and decoding functions.

The figure below shows the basic DPCM/DCT hybrid encoder and decoder. In the encoder, the video frame n ($F_n$) is processed to produce a coded or compressed bitstream. In the decoder, the compressed bitstream is decoded to produce a reconstructed video frame $F'_n$. The reconstructed output frame is not usually identical to the source frame. Most of the functions of the decoder are actually contained within the encoder, the reason for which will be explained later.

![[docs/img/dpcm-dct-encoder.png]]

***Encoder data flow***

There are two main data flow paths in the encoder, left to right (encoding) and right to left (reconstruction). 

The encoding flow is as follows:

1. An input video frame $F_n$ is presented for encoding and is processed in units of a macroblock, corresponding to a 16 x 16 pixel region of the video image.
2. $F_n$ is compared with a **reference** frame, for example the previous encoded frame ($F'_n-1$). A motion estimation function finds a 16 x 16 region in $F'_n-1$ that 'matches' the current macroblock in $F_n$. The offset between the current macroblock position and the chosen reference region is a motion vector MV.
3. Based on the chosen motion vector MV, a motion compensated prediction P is generated, the 16 x 16 region selected by the motion estimator. P may consist of interpolated sub-pixel data.
4. P is subtracted from the current macroblock to produce a residual or difference macroblock D.
5. D is transformed using the DCT. Typically, D is split into 8 x 8 or 4 x 4 sub-blocks and each sub-block is transformed separately.
6. Each sub-block is quantized (X).
7. The DCT coefficients of each sub-block are reordered and run-level coded.
8. Finally, the coefficients, motion vector and associated header information for each macroblock are entropy encoded to produce the compressed bitstream.

The reconstruction data flow is as follows:

1. Each quantized macroblock X is rescaled and inverse transformed to produce a decoded residual D'. Note that the non-reversible quantization process means that D' is not identical to D, because distortion has been introduced.
2. The motion compensated prediction P is added to the residual D' to produce a reconstructed macroblock. The reconstructed macroblocks are combined to produce reconstructed frame $F'_n$.

After encoding a complete frame, the reconstructed frame $F'_n$ may be used as a reference frame for the next encoded frame $F_n+1$.

***Decoder data flow***

1. A compressed bitstream is entropy decoded to extract coefficients, motion vector and header for each macroblock.
2. Run-level coding and reordering are reversed to produce a quantized, transformed macroblock X.
3. X is rescaled and inverse transformed to produce a decoded residual D'.
4. The decoded motion vector is used to locate a 16 x 16 region in the decoder's copy of the previous (reference) frame $F'_n-1$. This region becomes the motion compensated prediction P.
5. P is added to D' to produce a reconstructed macroblock. The reconstructed macroblocks are saved to produce decoded frame $F'_n$.

After a complete frame is decoded, $F'_n$ is ready to be displayed and may also be stored as a reference frame for the next decoded frame $F'_n+1$.

It is clear that the encoder includes a decoding path: rescale, IDCT, reconstruct. **This is necessary to ensure that the encoder and decoder uses identical reference frames $F'_n-1$ for motion compensated prediction.