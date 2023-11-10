# Learning for Video Compression With Recurrent Auto-Encoder and Recurrent Probability Model

They explored the use of [[RNNs]] for video compression. By using a wide range of video frames, they could explore temporal correlation in order to improve compression. A framework was designed and is built upon a Recurrent Auto-Encoder (RAE) and a Recurrent Probability Model (RPM). The former can explore all frames in order to compress the current one, and the latter is able to achieve lower bit-rate to compress a frame by leveraging [[entropy-coding | entropy encoding]]. This approach exploited both pixel and latent domains for compressing upcoming frames, and was called Recurrent Learned Video Compression (RLVC).

Instead of loading frames on memory, the recurrent network is able to save the states of previous frames in memory and recurrently move forward, exploring a larger range of temporal correlation with less memory fingerprint.

## Framework

First, the current frame and the *previously compressed frame* are compared in order to estimate temporal motion, by applying a [[pyramid-optical-flow-net|pyramid optical flow network]]. Then, the estimated motion is compressed by the proposed Recurrent Auto-Encoder (RAE). This compressed motion vector is then used to warp the previous frame, feeding it all to a CNN, that in turn predicts the final motion compensated frame. The next step is to obtain the *residual* between the current frame and the motion compensated one, followed by feeding it to another RAE module, and finally generating the current compressed frame by adding together the residual and the motion compensated frame.

The **RAE** is responsible to generate latent representations of the motion and the residual frame, while **RPM** is responsible to compress them into a bit stream by recurrently predicting the temporally conditional PMF.

![[Pasted image 20231106150109.png | RLVC framework]]

