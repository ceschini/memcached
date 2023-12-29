# Standard Hybrid Video Codec Terminology

Based on the sidebar inset article of [[rate-distortion-optim|Rate-distortion optimization]] paper.

**Prediction mode**: A method which is selected for use in approximating a picture region. 

**Block**: A rectangular region (normally of size 8 x 8) in a picture. The Discrete Cosine Transform (DCT) in standard video coders always operates on 8 x 8 block regions.

**Macroblock**: A region of size 16 x 16 in the luminance picture which is associated with a prediction mode.

**Motion vector (MV)**: A spatial displacement offset for use in the prediction of an image region.