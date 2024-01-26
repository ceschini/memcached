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