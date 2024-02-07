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
