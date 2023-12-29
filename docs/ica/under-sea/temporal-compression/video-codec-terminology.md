# Standard Hybrid Video Codec Terminology

Based on the sidebar inset article of [[rate-distortion-optim|Rate-distortion optimization]] paper.

**Prediction mode**: A method which is selected for use in approximating a picture region. 

**Block**: A rectangular region (normally of size 8 x 8) in a picture. The Discrete Cosine Transform (DCT) in standard video coders always operates on 8 x 8 block regions.

**Macroblock**: A region of size 16 x 16 in the luminance picture which is associated with a prediction mode.

**Motion vector (MV)**: A spatial displacement offset for use in the prediction of an image region. In the INTER prediction mode, an MV affects a macroblock region, while in the INTER+4V prediction mode, an individual MV is sent for each of the four 8 x 8 blocks in a macroblock.

**INTRA mode**: A prediction mode in which the picture content of a macroblock region is represented without reference to a region in any previously decoded picture.

**SKIP mode**: A prediction mode in which the picture content of a macroblock region is represented as a copy of the macroblock in the same location in a previously decoded picture.

**INTER mode**: A prediction mode in which the picture content of a macroblock region is represented as the sum of a motion-compensated prediction using a motion vector, plus (optionally) a decoded residual difference signal representation.

**INTER+4V mode**: A prediction mode in which the picture content of a macroblock region is represented as in the INTER mode, but using four motion vectors (one for each 8 x 8 block in the macroblock).

**INTER+Q mode**: A prediction mode in which the picture content of a macroblock is represented as in the INTER mode, and a change is indicated for the inverse quantization scaling of the decoded residual signal representation.

