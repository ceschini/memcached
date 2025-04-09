# Conditional Prompt Learning for Vision-Language Models

The authors observed that CoOp overfits base classes observed during training. To address this issue they propose Conditional Context Optimization (CoCoOp), which extends CoOp by further training a lightweight neural network to generate for each image an input-conditional token (vector). This dynamic prompt is more robust against class shifts than the static prompt of CoOp.

The learnable, input-conditional token is **combined** with the learnable context vectors.

![[docs/img/cocoop-approach.png]]

Two learnable components: a set of context vectors (CoOp) and a lightweight neural network (Meta-Net) that generates for each image an input-conditional token.

The performance of both methods are almost the same for the base classes observed during training, but CoCoOp generalizes way better than its former counterpart.

During training, both context vectors and the Meta-Net's parameters are updated. The Meta-Net is built with a two-layer bottleneck structure (Linear-ReLU-Linear), with the hidden layer reducing the input dimension by 16x. The input to the Meta-Net is simply the output features produced by the image encoder.