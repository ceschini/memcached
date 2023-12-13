# Neural Video Compression with Diverse Contexts

The idea behind a video codec is to capture the context of the frames, and by doing so reduce the redundancy to save bits. This is done by tradicional codecs by using code modes, where each mode can capture a specific kind of context. Different modes are then developed and processed in order to select the best one for that specific sequence. This codes are handmade and take a lot of computational time. But they are also directly related to bit-saving ratios.

Contexts here resembles the feature kernels or filter maps of convolutional networks. There is a lot of different kernels where each kind can capture different structures.

The idea of the authors is to improve contexts of *Neural Video Compression (NVC)* algorithms while keeping computational cost at low. They propose improvements both in temporal space as in spatial space.

![[dcvc-dc-framework.png]]
## Key aspects

### Hierarchical quality patterns

Encode different frames with different quantization parameters, saving it on different layers of the hierarchical structure. When predicting the current frame context, uses these reference frames for motion estimation. The new frame with motion compensation is also of high quality, and as such leads to a smaller prediction error. By using the previous frame with these long-range reference frames, more diverse contexts are generated. This is very helpful to alleviate the **error propagation problem** that many other NVCs suffer from.

### Group-based Offset Diversity



### Quadtree Partition-Based Entropy Coding

## Workflow

The coding pipeline contains three core steps: $fmotion$, $fTcontext$, and $fframe$.

$fmotion$ receives the *current frame* + the previous *reconstructed frame*. It uses **optical flow network** it to estimate the motion vector (MV) $v_t$, which is encoded and decoded as $v_t$ hat.

$fTcontext$ receives the decoded $v_t$hat with the propagated feature $Ft-1$ from the previous frame. It then *extracts the motion-aligned temporal context feature* $Ct$.

It uses hierarchical quality patterns to train weights and assign it to each quality reference frame on the hierarchy. By adjusting these weights, it then learns what is the best one to move up or down the stack.

By referencing both the nearest frame and the farther high-quality frame, it can then achieve top performance.

$fframe$ encodes $xt$ into quantized latent representation by using $Ct$ to generate $yt$ hat. Then, By using **Channel Partition**, it uses $Ct$ to decode $yt$ hat, feeding it to a frame generator to produce $xt$ hat and $Ft$.
