# Swin Transformer: Hierarchical Vision Transformer using Shifted Windows

In this paper, the author's goal is to expand the applicability of Transformer such that it can serve as a general-purpose backbone for computer vision, as it does for NLP, taking the role from CNNs.

The two main problems on using Transformers in vision tasks is the scale and the resolution of images. 

Visual elements can vary substantially in scale, a problem that receives attention in tasks such as object detection. In existing Transformer-based models, tokens are all of a fixed scale, a property unsuitable for these vision applications.

In tasks such as semantic segmentation, that require dense prediction at the pixel level, high resolution images would be intractable for Transformers, as the computation complexity of its self-attention is quadratic to image size.

To overcome this issues the authors propose a general-purpose Transformer backbone called Swin Transformer, which constructs hierarchical feature maps and has linear computational complexity to image size.

![[docs/img/swin-transformer-hierarchical.png]]

Swin Transformer constructs a hierarchical representation by starting from small-sized patches and gradually merging neighboring patches in deeper Transformer layers. With these hierarchical feature maps, the Swin Transformer model can conveniently leverage advanced techniques for dense prediction.

The linear computational complexity is achieved by computing self-attention locally within non-overlapping windows that partition an image. The number of patches in each window is fixed, and thus the complexity becomes linear to image size.

The Swin Transformer achieves strong performance on the recognition tasks of image classification, object detection and semantic segmentation, outperforming both ViT and DeiT.

## Overall Architecture

Swin Transformer first splits an input RGB image into non-overlapping patches by a patch splitting module, like ViT. Each patch is treated as a "token" and its feature is set as a concatenation of the raw pixel RGB values.

*A linear embedding layer is applied on this raw-valued feature to project it to an arbitrary dimension (denoted as C).*

To produce a hierarchical representation, the number of tokens is reduced by patch merging layers as the network gets deeper. The first patch merging layer concatenates the features of each group of 2 x 2 neighboring patches, and applies a linear layer on the 4C-dimensional concatenated features. **This reduces the number of tokens by a multiple of 2 x 2 = 4 (2x downsampling of resolution)**, and the output dimension is set to 2C.

![[docs/img/swin-transformer-architecture.png]]

### Swin Transformer block

Swin Transformer is built by replacing the standard multi-head self attention (MSA) module in a Transformer block by a module based on shifted windows, with other layers kept the same. A Swin Transformer block consists of a shifted window based MSA module, followed by a 2-layer MLP with GELU non-linearity in between. A LayerNorm (LN) layer is applied before each MSA and MLP module, and a residual connection is applied after each one of those.

![[docs/img/swin-transformer-blocks.png]]

Two successive Swin Transformer Blocks. W-MSA and SW-MSA are multi-head self attention modules with regular and shifted windowing (SW) configurations, respectively.

## Shifted Window based Self-Attention

The standard Transformer architecture and its adaptation for image classification both conduct global self-attention, where the relationships between a token and all other tokens are computed. This global computation is unsuitable for dense prediction or to represent a high-resolution image.

For efficient modeling, the authors propose to compute **self-attention within local windows**. The windows are arranged to evenly partition the image in a non-overlapping manner. However, the window-based self-attention module lacks connections across windows, which limits its modeling power.

To introduce cross-window connections while maintaining the efficient computation of non-overlapping windows, the authors propose a **shifted window partinioning approach** which alternates between two partinioning configurations in consecutive Swin Transformer blocks.

![[docs/img/shifted-window-approach.png]]
