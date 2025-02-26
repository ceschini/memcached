# Deep Learning Super Sampling 4

Deep Learning Super Sampling (DLSS) is a suite of real-time deep learning image enhancement and upscaling technologies developed by Nvidia.

The goal of these technologies is to **allow the majority of the graphics pipeline to run at a lower resolution** for increased performance, and then infer a higher resolution image from this that approximates the same level of detail as if the image has been rendered at this higher resolution (*basically super resolution*).

DLSS 4 adds Multi Frame Generation, allowing a greater number of frames to be generated and interpolated based on a single traditionally rendered frame. There is also a new AI-model based on the transformer architecture, an improved frame stability and reduced memory usage.

Nvidia claims that using the DLSS 4 Frame Generation there was a reduction of 30% less video memory with the example of Warhammer 40k: Darktide, using 400MB less memory at 4k resolution.