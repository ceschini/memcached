# Variational Image Compression With a Scale Hyperprior

## Introduction

Like all lossy compression methods, they operate on a simple principle: an image, typically modeled as a vector of pixel intensities $x$, is **quantized**, reducing the amount of information required to store or transmit it, but introducing error at the same time. Typically, it is not the pixel intensities that are quantized directly. Rather, an **alternative (latent)** representation of the image is found, a vector in some other space $y$, and quantization takes place in this representation, yielding a discrete-valued vector $yhat$. Because it is discrete, it can be losslessly compressed using _entropy coding_ methods, such as arithmetic coding, to create a bitstream which is sent over the channel. Entropy coding relies on a **prior** probability model of the quantized representation, which is known to both encoder and decoder (the _entropy model_).

The smallest average code length an encoder-decoder pair can achieve, using a shared entropy model, is given by the Shannon cross entropy between the two distributions. 

Note that this entropy is minimized if the model distribution is identical to the marginal. This implies that a fully factorized entropy model with statistical dependencies will lead to suboptimal compression performance.

One way conventional compression methods increase their compression performance is by transmitting _side information_: additional bits of information sent from the encoder to the decoder, which signal modifications to the entropy model intended to reduce the mismatch. In this scheme, the hope is that the amount of side information sent is smaller, on average, than the reduction of code length achieved by matching the entropy model more closely to the marginal for a particular image.

For example, JPEG models images as independent fixed-size blocks of 8 x 8 pixels. However, some image structure, such as large homogeneous regions, can be more efficient represented by considering larger blocks at a time. For this reason, recent works such as HEVC dynamically adapt the size of those blocks. For HEVC to work, the decoder needs to decode the side information first, so that it can use the correct entropy model to decode the block representations. This can be efficient due to the ability to choose a partitioning that optimizes the entropy model for each image.

But this is usually hand-designed. In contrast, the authors present a model that essentially learns a latent representation of the entropy model, in the same way that the underlying compression model learns a presentation of the image. **This model is optimized by learning to balance the amount of side information with the expected improvement of the entropy model**, by expressing the problem formally in terms of variational autoencoders (VAEs).

They use this formalism to show that side information can be viewed as a prior on the parameters of the entropy model, making them *hyperpriors* of the latent representation.





