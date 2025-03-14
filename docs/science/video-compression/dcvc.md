# Deep Contextual Video Compression

The authors propose a paradigm shift for video compression. From the commonly used predictive compression, to a conditional compression. Predictive compression is based on applying filters to previous frames, predict it and then subtracting their encoded residual from the current frame, in order to compress it. This approach is fast, but it is handmade and static, in which it won't work on all contexts, limiting the amount of possible entropy gains.

The authors present a condition framework where the feature domain contexts are used to compress the frames, thus leveraging high context and a learnable kernel, instead of the static and handmade kernels used in predictive compression, that not always can be relevant.

This learning aspect can result in higher quality compression ratios, when compared to the _very slow_ settings of the standard codecs of H.261 and H.265.

They say that there is a mode where the entropy coding applies only the temporal compression module instead of spatial-temporal, and this could result in faster compression rates, _which are important to us_.

The definition of a condition is anything useful to compress the current frame. This can be the predicted frame, as in the predictive compression, but not only it. In the proposed work, the authors show that the features learned by a deep learning network can be used at both the encoder, the decoder, and at the [[entropy-coding|entropy]] level.

In order to learn this condition, the authors employ Motion Estimation and Motion Compensation (MEMC) at feature domain. This can help guide the model where to extract useful features.

The main contribution of this paper is the new approach for video compression, the conditional-based paradigm, with substantial gains in compression rates.

_Maybe this suggests that it can better learn series of movements, which in case are not very interesting to our videos._

![[docs/img/dcvc-paradigm-shift.png]]

> For entropy modeling, we design a model which utilizes spatial-temporal correlation for higher compression ratio or only utilizes temporal correlation for fast speed.

## The Framework

> In this paper, we do not adopt the commonly-used residue coding but try to design a conditional coding-based framework for higher compression ratio.

Instead of using the previous predicted frame to encode the current one, they train a network to *generate context*. This context is in the feature domain of the network, with higher dimensions. On each dimension, the network will shift its attention to distinct contexts of the frame. For example, in one channel it will extract high frequency information, while on the others it will extract color information, borders or whatnot.

In residue coding, redundancy is subtracted between the predicted and the current frame, and is tied to entropy gains by the level of motion and pixel shifts between frames. Instead of using this approach, the authors propose leveraging a neural network to learn the correlation between the current and the predicted frame, removing redundancy in the process. This approach leverages the learning adaptability of the network to solve this new motions and contexts.

![[docs/img/dcvc-framework.png]]

## Entropy model

In order to encode the bit stream, the authors apply both hierarchical, spatial and temporal priors to latent codes. First, they use the [[hyperprior|hyperprior]] model to learn the hierarchical prior and use the autoregressive network to learn the spatial prior. Both are commonly-used in deep image compression. However, for video, the latent codes also have the temporal correlation. Thus, they propose using the context x⁻t to generate the temporal prior.

This model utilizes spatial prior for higher compression ratio, but the instructions of this module are non-parallel, and as such can drastically reduce efficiency. On the other hand, temporal priors are fully parallel, and so a distinct accelerated mode with only this module is also proposed.

![[docs/img/entropy-model.png]]

## Context learning

One simple way to learn the context is to train a plain CNN to predict the features. But this is complex due to motion differences between the frames. Because of that, the authors chose to apply MEMC. This way, the motion vector is encoded and warped together with the previous frame features. While the common use of MEMC is in the RGB pixel domain, the authors propose to apply MEMC over the feature domain. 

They train a network to learn how to convert the reference frame from pixel to feature domain. At the same time, an optical flow estimation network will learn the motion vector (MV) between the reference frame and the current frame. This encoded motion vector ($mhat$ $t$) will help guide the model on where to extract the content through warping operation.