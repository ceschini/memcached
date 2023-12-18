# Deep Contextual Video Compression

The authors propose a paradigm shift for video compression. From the commonly used residual compression, to a conditional compression. Residual compression is based on applying filters to previous frames, encoding it and then subtracting their residual from the current frame, in order to compress it. The authors present a condition framework where the feature maps are used to compress the frames, thus leveraging high context and a learnable kernel, instead of the static and handmade kernels used in residual compression.

This learning aspect can result in higher quality compression ratios, when compared to the _very slow_ settings of the standard codecs of H.261 and H.265.

They say that there is a mode where the entropy coding applies only the temporal compression module instead of spatial-temporal, and this could result in faster compression rates, which are important to us.

