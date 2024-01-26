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

![[encoder-drawing.jpg]]

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

![[motion-estimation.png]]

There are a lot of variations of the motion estimation and motion compensation (MEMC) explained above. For example, future frames might be more meaningful to the prediction of a macroblock, and then should be encoded before the current one, giving an out-of-order transmission. Sometimes there is a significant change in motion between two frames, and as such there is no gain in using motion compensation, in such cases the CODEC might use intra coding instead. Not every moving object will be encompassed in a 16 x 16 pixel size block, and dynamic sizes might be applied to better envelop objects on different scenes. 

#### 3.3.1.5 Motion compensation block size

Smaller motion compensation block sizes can produce better motion compensation results. However, a smaller block size leads to increased complexity, with more search operations to be carried out, and an increase in the number of motion vectors that need to be transmitted. Sending more vectors means sending more bits of information, and this overhead might not be beneficial compared to the residual size gains. Because of that, the best approach is to adapt the block size to the image characteristics. In large homogeneous regions, a big macroblock can capture all the information contained within, on the other hand, in small edgy and complex regions a small macroblock size might be necessary in order to attend to the complex shapes and as such reduce the residual energy while not overheading it too much.

#### 3.3.1.6 Sub-pixel motion compensation

Objects might not always move by integer valued pixels. And as such, sometimes it is better to explore regions with _sub-pixel_ values, by interpolating half-pixel positions. Sub-pixel motion estimation and compensation involves searching sub-pixel interpolated positions as well as integer-pixel positions and choosing the position that gives the best match and minimizes the residual energy.

On the example below, the first stage of the motion estimation is to find the best match on the integer pixel grid (circles). The encoder searches the half-pixel positions immediately next to this best match (squares) to see whether the match can be improved and, if required, the quarter-pixel positions next to the best half-pixel position (triangles) are then search. The final match, at an integer, half-pixel or quarter-pixel position, is subtracted from the current block or macroblock.

![[sub-pixel-motion-estim.png]]

In general, 'finer' interpolation provides better motion compensation performance, producing a smaller residual at the expense of increased complexity. The performance gain tends to diminish as the interpolation steps increase.

There is a trade-off in compression efficiency associated with more complex motion compensation schemes.

### 3.3.2 Spatial model: intra prediction

In the intra prediction mode, macroblocks of the current frame are predicted based on the already encoded macroblocks within the same frame. By doing so, it guarantees that only information already available for the decoder is used. This approach leverages the assumption that neighboring pixels are commonly correlated, and as such it will use this neighboring blocks of pixels in order to perform the prediction. Once the prediction has been generated, it is subtracted from the current block, similarly of that of inter prediction. The residual is transformed and encoded, together with an indication of how the intra coding was performed.

![[intra-preds-available-samples.png]]

![[intra-preds-process.png]]

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

![[dct-A-matrix.png]]

**_Example: N = 4_**

The transform matrix $A$ for a 4 x 4 DCT is:

![[dct-A-4x4-matrix.png]]

