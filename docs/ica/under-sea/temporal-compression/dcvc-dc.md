# Neural Video Compression with Diverse Contexts

The idea behind a video codec is to capture the context of the frames, and by doing so reduce the redundancy to save bits. This is done by tradicional codecs by using code modes, where each mode can capture a specific kind of context. Different modes are then developped and processed in order to select the best one for that specific sequence. This codes are handamade and take a lot of computational time. But they are also directly related to bit-saving ratios.

Contexts here resembles the feature kernels or filter maps of convolutional networks. There is a lot of different kernels where each kind can capture different structures.

The idea of the authors is to improve contexts of Neural Video Compression algorithms while keeping computational cost at low. They propose improvements both in temporal space as in spatial space.

## Key aspects

Hierarchical quality patterns.

Group-based offset diversity.

Context mining.

quadtree-based partition