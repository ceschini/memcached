# H.264 Prediction

## 6.1 Introduction

Prediction is the most important aspect of H.264, due to its high performance and efficiency, being considered the primary cause of high compression gains.

For every macroblock, a prediction is created, an attempt to duplicate the information contained in the macroblock using previously coded data, and subtracted from the macroblock to form a residual. *This means that the better the prediction, the less information must be encoded and sent to the decoder.* The efficiency or accuracy of this prediction process has a significant impact on compression performance. An accurate prediction means that the residual contains very little data, and this in turns leads to good compression performance.

There are a wide range of options for prediction available for the encoder to choose from, and by selecting the optimal prediction options for an individual macroblock, an encoder can minimize the residual size to produce a highly compressed bitstream.

## 6.2 Macroblock prediction

A Macroblock can be predicted by one of three methods. An I Macroblock will be predicted based on previously coded samples within the same frame, using Intra-prediction. A P Macroblock will be predicted based on one previously coded frame by using Inter-prediction. A B Macroblock will be predicted based on two previously coded frame by using Inter-prediction. Both P and B macroblock may use future frames that have been already coded.

![[macroblock-types-predictions.png]]

## 6.3 Intra prediction

An intra (I) macroblock is coded without referring to any data outside the current slice. I macroblocks may occur in any slice type, but every macroblock in an I slice is an I macroblock. Intra prediction is based on the fact that for a typical block of luma or chroma samples, there is high correlation between samples in the block and samples that are immediately adjacent to the block. As such, Intra prediction uses samples from adjacent, *previously coded blocks* to predict the values in the current block.

In the image below, a 4x4 luma blocks in a macroblock is being intra-predicted. The current block number 10 have the option to choose between blocks 1, 3, 5 and 8 to use as reference.

![[intra-pred-4x4.png]]

In an intra macroblock, there are three choices of intra prediction block size for the luma component, namely 16 x 16, 8 x 8 or 4 x 4. 

When using any mode other than the default 16 x 16, the intra prediction is created from samples located between the 'northeast' and 'west' of the current block. Luma and chroma components based on the default may only choose between the 'nort' or 'west' adjacent blocks. This is better illustrated on the figures below.

![[luma-4-or-8-size.png]]

![[intra-16-size.png]]

The encoder chooses an intra mode for the current block from the available prediction modes. The choice of intra mode is communicated to the decoder as part of the coded macroblock. Choosing between the different block sizes for luma and chroma tends to be a trade-off between prediction efficiency and cost of signalling the prediction mode.

(a) Smaller blocks: A smaller prediction block size (4 x 4) tends to give a more accurate prediction, leading to a smaller coded residual. However, the choice of prediction for every 4 x 4 block must be signalled to the decoder, which means that more bits tend to be required to code the prediction choices.

(b) Larger blocks: A larger prediction block size (16 x 16) tends to give a less accurate prediction, hence more residual data, but fewer bits are required to code the prediction choice itself.

An encoder will typically choose the appropriate intra prediction mode to minimize the total number of bits in the prediction and the residual.

In homogeneous regions of the frame, where the texture is largely uniform, 16 x 16 prediction mode tends to be more efficient since the prediction is reasonably accurate and the prediction mode overhead is low. In more complex regions of the frame, 4 x 4 mode is often selected because the increased rate required to signal the prediction mode is offset by the reduced residual size.

### 6.3.1 4 x 4 luma prediction modes

A 4 x 4 block of luma samples is predicted using nine different prediction modes, each mode leverages the available reference samples in a singular way, and after processing them all, the encoder can choose the one that produces the smaller Sum of Absolute Errors (SAE) as the best match.

There are some backup modes that are always available, such as mode 2, DC, and each of the other modes may only be used if all the required prediction samples are available. If a group of samples is not available, the closest one can be used to copy its value to the missing ones.

For example, let's take a 4 x 4 luma block and predict it. Here the target block is highlighted and represents the edge of a raster scanning order.

![[4x4-luma-prediction.png]]

The samples above and to the left, labeled A-M in the figure below, have previously been encoded and reconstructed, and as such are available in the encoder and decoder to form a prediction reference.

![[Pasted image 20240207153917.png]]

The samples a, b, c, ..., p of the prediction block are calculated based on the samples A-M as follows.

| Mode # (Direction)           | Procedure                                                                                                 |
| ---------------------------- | --------------------------------------------------------------------------------------------------------- |
| Mode 0 (Vertical)            | The upper samples A, B, C, D are extrapolated vertically.                                                 |
| Mode 1 (Horizontal)          | The left samples I, J, K, L are extrapolated horizontally.                                                |
| Mode 2 (DC)                  | All samples in P are predicted by the mean of samples A..D and I..L.                                      |
| Mode 3 (Diagonal Down-Left)  | The samples are interpolated at a 45$^o$ angle between lower-left and upper-right.                        |
| Mode 4 (Diagonal Down-Right) | The samples are extrapolated at a 45$^o$ angle down and to the right.                                     |
| Mode 5 (Vertical-Left)       | Extrapolation at an angle of approximately 26.6$^o$ to the left of vertical, i.e. width/height = $^1/_2$. |
| Mode 6 (Horizontal-Down)     | Extrapolation at an angle of approximately 26.6$^o$ below horizontal.                                     |
| Mode 7 (Vertical-Right)      | Extrapolation or interpolation at an angle of approximately 26.6$^o$ to the right of vertical.            |
| Mode 8 (Horizontal-Up)       | Interpolation at an angle of approximately 26.6$^o$ above horizontal.                                     |

![[intra-pred-mode-directions.png]]

The arrows in the figure above indicate the direction of prediction in each mode. For modes 3-8, the predicted samples are formed from a weighted average of the prediction samples A-M. For example, if mode 4 is selected, the top-right sample of P, labelled 'd', is predicted by: d = round(B/4 + C/2 + D/4).

### 6.3.2 16 x 16 luma prediction modes

As an alternative to the 4 x 4 luma modes described above, the entire 16 x 16 luma component of a macroblock may be predicted in one operation. Four modes are available.

| Mode # (Direction)  | Description                                                                                                                            |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Mode 0 (vertical)   | Extrapolation from upper samples (H)                                                                                                   |
| Mode 1 (horizontal) | Extrapolation from left samples (V)                                                                                                    |
| Mode 2 (DC)         | Mean of upper and left-hand samples (H+V)                                                                                              |
| Mode 4 (Plane)      | A linear 'plane' function is fitted to the upper and left-hand samples H and V. This works well in areas of smoothly-varying luminance |


![[luma-16x16-modes.png]]

### 6.3.3 Chroma prediction modes

Each chroma macroblock is predicted from previously encoded chroma samples above and/or to the left, with both chromas using the same mode. It is very similar to the 16 x 16 luma modes described above, except that the order of modes is different. The modes are DC (mode 0), horizontal (mode 1), vertical (mode 2), and plane (mode 3).

### 6.3.4 8 x 8 luma prediction, High profiles

This mode is very similar to the 4 x 4 luma modes describe before, yet this mode is only available in the High profiles.

### 6.3.5 Signalling intra prediction modes

#### 6.3.5.1 4 x 4 or 8 x 8 luma prediction

The choice of intra prediction mode for each 4 x 4 or 8 x 8 luma block must be signalled to the decoder, and this could potentially require a large number of bits. However, intra modes for neighboring blocks are highly correlated. To leverage this information, predictive coding is used to signal the prediction mode based on previously coded neighboring modes. For example, if A and B are the upper and left macroblocks, and E is the current macroblock, the encoder and decoder calculate the most probable prediction mode, defined as the smaller of the prediction modes of A and B. If one of them is not available, it is set to mode 2 (DC). This happens if `prev_intra4x4_pred_mode` flag is set to 1. If it is set to 0, another mode is signalled by the `rem_intra4x4_pred_mode`.

## 6.4 Inter prediction

Inter prediction is the process of predicting a block of luma and chroma samples from a picture that has previously been coded and transmitted, a reference picture. This involves selecting a prediction region, generating a prediction block and subtracting this from the original block of samples to form a residual that is then coded and transmitted. The block of samples to be predicted, a macroblock partition or a sub-macroblock partition, can range in size from the hole macroblock, 16 x 16, to 4 x 4 sub-macroblocks.

The reference picture is chosen from a list of previously coded pictures, stored in a *Decoded Picture Buffer*, which may include pictures from before or after the current displayed frame. The offset between the position of the current partition and the prediction region in the reference picture is a *motion vector*. This motion vector may have integer, half or quarter-sample positions, with interpolated values in these last two cases. Each motion vector is differentially coded from the motion vectors of neighboring blocks.

The prediction block may be generated from a single prediction region in a reference picture, for a P or B macroblock, or from two prediction regions in reference pictures, for a B macroblock.

Optionally, the prediction block may be weighted according to the temporal distance between the current and reference picture, known as *weighted prediction*. In a B macroblock, a block may be predicted in *direct mode*, in which case no residual samples or motion vectors are sent and the decoder infers the motion vector from previously received vectors.

To summarize the process of coding an inter-predicted macroblock (note that the steps need not occur in this exact order):

1. Interpolate the picture(s) in the Decoded Picture Buffer to generate $¹/_4$-sample positions in the luma component and $¹/_8$-sample positions in the chroma components. (see 6.4.2)
2. Choose an inter prediction mode from the following options:
	1. Choice of reference picture (see 6.4.1)
	2. Choice of sub or macroblock partitions sizes (see 6.4.3)
	3. Choice of prediction types:
		1. from one reference picture in list 0 for P or B macroblocks or list 1 for B macroblocks only
		2. bi-prediction from two reference pictures, one in list 0 and one in list 1, B macroblock only
3. Choose motion vector(s) for each macroblock partition or sub-macroblock partition, one or two vectors depending on whether one or two reference pictures are used.
4. Predict the motion vector(s) from previously-transmitted vector(s) and generate motion vector difference(s)
5. Code the macroblock type, choice of prediction reference(s), motion vector difference(s) and residual
6. Apply a *deblocking filter* prior to storing the reconstructed picture as a prediction reference for further coded pictures. (see 6.5)

### 6.4.1 Reference pictures

Inter prediction makes use of previously coded pictures that are available to the decoder. Slices are received, decoded to produce pictures and displayed. They are also stored in the Decoded Picture Buffer (DPB). The pictures in the DPB are indexed (listed in a particular order), in the following Lists, depending on the type of slice, P or B.

**List 0 (P slice):** A single list of all the reference pictures. Default order is most recently decoded picture first (stack).

**List 0 (B slice):** A list of all the reference pictures. Default order is display order, first in the list is the picture being displayed before the current one.

**List 1 (B slice):** A list of all the reference pictures. The default order is the reversed display order, first in the list is the picture being displayed after the current one.

The construction of List 0 is different depending on whether the current MB occurs in a P or B slice.

### 6.4.2 Interpolating reference pictures

Each partition in an inter-coded macroblock is predicted from an area of the same size in a reference picture. The offset between the two areas, the motion vector, has 1/4-pixel resolution for the luma component and 1/8-pixel resolution for the chroma components. Samples at sub-pixel positions do not exist in the reference picture, and as such must be created by using interpolation from nearby image samples.

In the figure below, a 4 x 4 block in the current frame (a) is predicted from a neighboring region of the reference picture. If the horizontal and vertical components are integers (b), the relevant samples in the reference block actually exist, shown as gray dots. If one or both vector components are fractional values (c), the prediction samples, shown as gray dots, are generated by interpolation between adjacent samples in the reference frame, shown as white dots.

![[motion-vector-sub-pixel.png]]

#### 6.4.2.1 Generating interpolated sub-pixel samples

##### Luma Component

The half-pel samples in the luma component of the reference picture are generated first (the single-lined blocks in the figure below). Each half-pel sample that is adjacent to two integer samples, such as *b, h, m,* and *s*, is interpolated from integer-pel samples using a 6-tap *Finite Impulse Response* (FIR) filter with weights (1/32, -5/32, -5/32, 5/8, 5/8, -5/32, 1/32). For example, half-pel sample **b** is calculated from the 6 horizontal integer samples E, F, G, H, I and J using a process equivalent to:

$b = round((E - 5F + 20G + 20H -5I + J)/32)$

Similarly, **h** is interpolated by filtering A, C, G, M, R and T.

Once all of the samples adjacent to integer samples have been calculated, the remaining half-pel positions are calculated by interpolating between six horizontal or vertical *half-pel samples* from the first set of operations. For example, **j** is generated by filtering cc, dd, h, m, ee and ff. Note that the result is the same whether j is interpolated horizontally or vertically.

![[half-pel-interpolation.png]]

The 6-tap interpolation filter is relatively complex but produces an accurate fit to the integer-sample data and hence good motion compensation performance.

Once all the half-pixel samples are available, the quarter-pixel positions are produced by linear interpolation. Quarter-pixel positions with two horizontally or vertically adjacent half- or integer-pixel samples, for example **a, c, i, k** and **d, f, n, q** in the figure below, are *linearly interpolated* between these adjacent samples, for example:

$a = round((G + b)/2)$

![[quartel-pel-interpolation.png]]

The remaining quarter-pixel positions, **e, g, p** and **r** in the figure, are linearly interpolated between a pair of diagonally opposite half-pixel samples. For example, **e** is interpolated between b and h.

An example of a quarter-pixel resolution interpolation is shown below.

![[interpolation-result.png]]

##### Chroma components

Quarter-pel resolution motion vectors in the luma component require eighth-pel resolution vectors in the chroma components, assuming 4:2:0 sampling. Interpolated samples are generated at 1/8-pel intervals between integer samples in each chroma component using linear interpolation.

![[chroma-interpolation.png]]

Each sub-pixel position **a** is a linear combination of neighboring integer pixel positions A, B, C and D.

### 6.4.3 Macroblock partitions

Each 16 x 16 P or B macroblock may be predicted using a range of block sizes. The macroblock is split into one, two or four **macroblock partitions**: either

- (a) one 16 x 16 macroblock partition (covering the whole MB),
- (b) two 8 x 16 partitions,
- (c) two 16 x 8 partitions,
- (d) four 8 x 8 partitions.

If 8 x 8 partition size is chosen, then each 8 x 8 block of luma samples and associated chroma samples, a **sub-macroblock**, is split into one, two or four **sub-macroblock partitions**: one 8 x 8, two 4 x 8, two 8 x 4, or four 4 x 4 sub-MB partitions.

![[sub-and-macroblock-partitions.png]]

Each sub or macroblock partition has one or two motion vectors (x, y), each pointing to an area of the same size in a reference frame that is used to predict the current partition. Each macroblock partition may be predicted from different reference frames. However, the sub-macroblock partitions within an 8 x 8 sub-macroblock share the same reference frame(s).

### 6.4.4 Motion vector prediction

Encoding a motion vector for each partition can cost a significant number of bits, especially if small partition sizes are chosen. Motion vectors for neighboring partitions are often highly correlated and so each motion vector is predicted from vectors of nearby, previously coded partitions. A predicted vector, MVp, is formed based on previously calculated motion vectors and MVD, the difference between the current vector and the predicted vector, is encoded and transmitted. The method of forming the prediction MVp depends on the motion compensation partition size and on the availability of nearby vectors.

### 6.4.5 Motion compensated prediction

An H.264 encoder or decoder creates a motion-compensated prediction from reference picture(s) in the DPB using the motion vectors and reference list indices signalled in a P or B macroblock. Reference list indices identify the reference picture(s) used to form the prediction; motion vectors indicate the offset from the current macroblock or partition to corresponding reference area(s) in the reference picture(s).

![[motion-compensated-prediction.png]]

#### 6.4.5.3 Weighted prediction

Weighted prediction is a method of scaling the samples of motion-compensated prediction data in a P or B slice macroblock. There are three types of weighted prediction in H.264, 'explicit' weighted prediction for P or B slice macroblock, and 'implicit' weighted prediction for B slice.

Each prediction sample is scaled by a weighting factor prior to motion-compensated prediction. In the 'explicit' types, the weighting factor(s) are determined by the encoder and transmitted in the slice header. If 'implicit' prediction is used, both weighting factors are calculated based on the relative temporal positions of the list 0 and list 1 reference frames.

A larger weighting factor is applied if the reference frame is temporally close to the current frame, and a smaller factor if the reference frame is temporally further away from the current frame.

One application of weighted prediction is to allow explicit or implicit control of the relative contributions of reference frames to the motion-compensated prediction process. For example, this approach may be particularly effective in coding of 'fade' transitions where one scene fades into another.

***Example:***

![[example-P-slice-partitions.png]]

Motion compensated prediction tends to be more accurate when small partition sizes are used, especially when motion is relatively complex. However, more partitions in a macroblock mean that more bits must be sent to indicate the motion vectors and choice of partitions. Typically, an encoder will choose larger partitions in homogeneous areas of a frame with less texture / movement and smaller partitions in areas of more complex motion, for example near the mouth of the man in the image above.

Making the 'best' choice of motion compensation partitions can have a significant impact on the compression efficiency of an H.264 sequence. The trade-off between partition choices and residual compression can be optimized using *Rate Distortion Optimized (RDO) mode selection* (Chapter 9).

### 6.4.7 Prediction structures

H.264 offers many different options for choosing reference pictures for inter prediction. An encoder will typically use reference pictures in a structured way. Some examples of reference picture structures are presented here.

#### 6.4.7.1 Low delay, minimal storage

