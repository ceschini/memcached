# Deep Contextual Video Compression

The authors propose a paradigm shift for video compression. From the commonly used predictive compression, to a conditional compression. Predictive compression is based on applying filters to previous frames, predict it and then subtracting their encoded residual from the current frame, in order to compress it. The authors present a condition framework where the feature domain contexts are used to compress the frames, thus leveraging high context and a learnable kernel, instead of the static and handmade kernels used in predictive compression.

This learning aspect can result in higher quality compression ratios, when compared to the _very slow_ settings of the standard codecs of H.261 and H.265.

They say that there is a mode where the entropy coding applies only the temporal compression module instead of spatial-temporal, and this could result in faster compression rates, which are important to us.

The definition of a condition is anything useful to compress the current frame. This can be the predicted frame, as in the predictive compression, but not only it. In the proposed work, the authors show that the features learned by a deep learning network can be used at both the encoder, the decoder, and at the [[entropy-coding|entropy]] level.

In order to learn this condition, the authors employ Motion Estimation and Motion Compensation (MEMC) at feature domain. This can help guide the model where to extract useful features.

_Maybe this suggests that it can better learn series of movements, which in case are not very interesting to our videos._

![[dcvc-paradigm-shift.png]]

