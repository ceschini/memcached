# Recommendation H.264: Advanced Video Coding

By [vcodex.com](https://www.vcodex.com/an-overview-of-h264-advanced-video-coding/)

## 1 What is H.264?

H.264 is an industry standard for video compression. It defines a format or syntax for compressed video and a method for decoding this syntax to produce a displayable video sequence. The document does not actually specify how to encode (compress) digital video, this is left for the manufacturer of the video encoder.

![[h264_intro_1.png]]

## 2 How does an H.264 codec work?

An H.264 video encoder carries out _prediction_, _transform_ and _encoding_ processes, in order to produce a compressed H.264 bitstream.

### 2.1 Encoder processes

#### Prediction

The encoder processes a frame of video in units of a **Macroblock** (16x16 displayed pixels). It forms a **prediction** of the macroblock based on previously-coded data, either from the current frame (**intra** prediction) or from other frames that have already been coded and transmitted (**inter** prediction). The encoder subtracts the prediction from the current macroblock to form a residual¹.

**Intra** prediction uses 16x16 and 4x4 block sizes to predict the macroblock from surrounding, previously-coded pixels within the same frame.

![[intra-preds.png]]

**Inter** prediction uses a range of block sizes (from 16x16 down to 4x4) to predict pixels in the current frame from similar regions in previously-coded frames.

![[inter-preds.png]]

¹ Finding a suitable inter prediction is often described as **motion estimation**. Subtracting an inter prediction from the current macroblock is **motion compensation**.

#### Transform and quantization

A block of residual samples is transformed using a 4x4 or 8x8 **integer transform**, an approximate form of the Discrete Cosine Transform (DCT). The transform outputs a set of **coefficients**, each of which is a weighting value for a standard basis pattern. When combined, the weighted basis patterns re-create the block of residual samples.

![[dct-transform.png]]

The output of the transform, a block of transform coefficients, is **quantized**, i.e. each coefficient is divided by an integer value. Quantization reduces the precision of the transform coefficients according to a quantization parameter (QP). The result is a block in which most or all of the coefficients are zero, with a few non-zero coefficients. The bigger the QP value is, the more coefficients are set to zero, resulting in a higher compression rate at the expense of image quality. The lower the QP, the better the quality of the decompressed image, at the expense of compression ratio.

#### Bitstream encoding

The video coding process produces a number of values that must be **encoded** to form the compressed bitstream. These values include:

- Quantized transform coefficients
- information to enable the decoder to re-create the prediction
- information about the structure of the compressed data and the compression tools used during encoding
- information about the complete video sequence.

These values and parameters (**syntax elements**) are converted into binary codes using **variable length coding** and/or **arithmetic coding**. Each of these encoding methods produces an efficient, compact binary representation of the information. The encoded bitstream can then be stored and/or transmitted.

### 2.2 Decoder processes

#### Bitstream decoding

A video decoder receives the compressed H.264 bitstream, decodes each of the syntax elements and extracts the information described above (quantized transform coefficients, prediction information, etc.). This information is then used to reverse the coding process and recreate a sequence of video images.

#### Rescaling and inverse transform

The quantized transform coefficients are **re-scaled**. Each coefficient is multiplied by an integer value to restore its original scale. An inverse transform combines the standard basis patterns, weighted by the re-scaled coefficients, to re-create each block of residual data. These blocks are combined to form a residual macroblock.

#### Reconstruction

For each macroblock, the decoder forms an identical prediction to the one created by the encoder. The decoder adds the prediction to the decoded residual to **reconstruct** a decoded macroblock which can then be displayed as part of a video frame.

## 3. The H.264/AVC Inter Prediction

From [here](https://www.vcodex.com/h264avc-inter-prediction/).

Inter prediction creates a prediction model from one or more previously encoded video frames. The model is formed by shifting samples in the reference frame(s) (motion compensated prediction). The AVC CODEC uses block-based motion compensation, the same principle adopted by every major coding standard since H.261. The key difference is that H.264 supports variable block sizes and fine sub-pixel motion vectors.

### 3.1 Tree structured motion compensation

This is a two-level splitting mode that firstly partitions the macro block into block sizes ranging from 16x16 to 4x4 luminance samples. The luminance component of each macroblock may be split up in 4 ways: 16x16, 16x8, 8x16 or 8x8. Each of these sub-regions is a macroblock partition. If the 8x8 mode is chosen, each of the four 8x8 macroblock partitions within the macroblock may be split in a further 4 ways: 8x8, 8x4, 4x8, 4x4 (known as macroblock sub-partitions). These splits into partitions of variable size gives the possibility of a variety of combinations within each macroblock. This method of partitioning macroblocks into motion compensated sub-blocks of varying size is known as **tree structured motion compensation**.

![[macroblocks.png]]

A separate motion vector is required for each partition or sub-partition. Each motion vector must be coded and transmitted. In addition, the choice of partition(s) must be encoded in the compressed bitstream. The choice of partition size has a significant impact on compression performance. In general, a large partition size is appropriate for homogeneous areas of the frame, due to its need for few bits to signal the choice, and a small partition size may be beneficial for detailed regions, which may give a lower-energy residual after motion compensation.

**Example**: The figure below shows a residual frame (without motion compensation). The AVC reference encoder selects the "best" partition size for each part of the frame, i.e. the partition size that minimizes the coded residual and motion vectors. The macroblock partitions chosen for each area are shown superimposed on the residual frame. In areas with little change between frames (in grey) a 16x16 partition is chosen; in areas of detailed motion (residual in black or white), smaller partitions are chosen.

![[residual-partitions.png]]

### 3.2 Sub-pixel motion vectors

Each partition in an inter-coded macroblock is predicted from an area of the same size in a reference picture. The offset between the two areas (the motion vector) has 1/4-pixel resolution (for the luma component). The luma and chroma samples at sub-pixel positions do not exist in the reference picture, and so it is necessary to create them using interpolation from nearby image samples.

![[sub-pixel-prediction.png]]

In the figure above, a macroblock must be predicted in the current frame (a). **The black arrow represents the motion vector**. If the horizontal and vertical components of the motion vector are integers (b), then the relevant samples in the reference block actually exist (grey dots). But if the motion vector have fractional values, then there is no match with the reference pixels, and the prediction samples are generated by interpolation between adjacent samples in the reference frame (white dots).

### 3.3 Motion vector prediction

Encoding a motion vector for each partition can take a significant number of bits, especially if small partition sizes are chosen. Motion vectors for neighbouring partitions are often highly correlated and so each motion vector is predicted from vectors of nearby, previously coded partitions. A predicted vector, MVp, is formed based on previously calculated motion vectors. MVD, the difference between the current vector and the predicted vector, is encoded and transmitted. The method of forming the prediction MVp depends on the motion compensation partition size and on the availability of nearby vectors. The "basic" predictor is the median of the motion vectors of the macroblock partitions or sub-partitions immediately above, diagonally above and to the right, and left of the current partition or sub-partition.

## 4 The H.264/AVC Intra Prediction

From [here](https://www.vcodex.com/h264avc-intra-precition/).

### 4.1 Intro

If a block or macroblock is encoded in intra mode, a prediction block is formed based on previously encoded and reconstructed (but **unfiltered**) blocks. This prediction block P is subtracted from the current block prior to encoding. There are 9 optional prediction modes for each 4x4 luma block, that the encoder can choose from. For a 16x16 luma block, there are 4 optional modes, and there is one mode that is always applied to each 4x4 chroma block.

### 4.2 4x4 luma prediction modes

Based on the available previously encoded and reconstructed samples, the encoder can choose between 9 modes for the prediction of the current luma block. The encoder may select the prediction mode for each block that minimizes the residual between P and the block to be encoded.

![[luma-predict-modes.png]]

In this example, both upper and left blocks are available, and as such they will be used to predict the current luma block.

![[luma-modes-direction.png]]

The arrows in Figure 3 indicate the direction of prediction in each mode. For modes 3-8, the predicted samples are formed from a weighted average of the prediction samples A-Q.

The Sum of Absolute Errors (SAE) for each prediction indicates the magnitude of the prediction error. The smallest SAE indicates the best match to the actual current block. In the case of Figure 4, that is mode 7 (vertical-right).

![[luma-pred-blocks.png]]

### 4.3 16x16 luma prediction modes

As an alternative to the 4x4 luma modes described above, the entire 16x16 luma components of a macroblock may be predicted. Four modes are available, shown in diagram form in Figure 5:

Mode 0 (vertical): extrapolation from upper samples (H). Mode 1 (horizontal): extrapolation from left samples (V). Mode 2 (DC): mean of upper and left-hand samples (H+V). Mode 3 (plane): a linear "plane" function is fitted to the upper and left-hand samples H and V. This works well in areas of smoothly-varying luminance.

![[intra-16x16-prediction-modes.png]]

This mode works best in homogeneous areas of an image.

### 4.4 8x8 chroma prediction mode

Each of the 8x8 chrome components of a macroblock is predicted from chroma samples above and/or to the left that have previously been encoded and reconstructed. The 4 prediction modes are very similar to the 16x16 luma prediction modes described above. The same prediction mode is always applied to both chroma blocks.

Note: if any of the 8x8 blocks in the luma component are coded in Intra mode, both chroma blocks are Intra coded.

### 4.5 Encoding intra prediction modes

The choice of intra prediction mode for each 4x4 block must be signalled to the decoder, and this could potentially require many bits. However, intra modes for neighbouring 4x4 blocks are highly correlated. For example, if previously-encoded 4x4 blocks A and B were predicted using mode 2, it is likely that the best mode for the current block C is also mode 2.

For each current block C, the encoder and decoder calculate the `most_probable_mode`. If A and B are both coded in 4x4 intra mode and are both within the current slice, `most_probable_mode` is the minimum of the prediction modes of A and B; otherwise it is set to 2 (DC prediction).

The encoder sends a flag for each 4x4 block, `use_most_probable_mode`. If the flag is `1`, the parameter `most_probable_mode` is used. If the flag is `0`, another parameter `remaining_mode_selector` is sent to indicate a change of mode.

![[adjacent-blocks.png]]

