# JPEG Compression

## What is JPEG

JPEG is a standardized image compression mechanism. It was designed for compressing either full-color or gray-scale images with a "lossy" method. This means that the decompressed image isn't quite the same as the one being compressed.

This algorithm exploits known limitations of the human eye, notably the fact that small color changes are perceived less accurately than small changes in brightness. Thus, JPEG is intended for compressing images that will be looked at by humans, so machine-analysis and evaluation must take this into account.

With JPEG, the degree of lossiness can be varied by adjusting compression parameters. This allows a trade off between file size and image quality.

## Why should we use it

This algorithm is responsible for the widespread of digital images and allowed the transmission of images over the internet at its early days.

Nowadays, it is still relevant due to its high compression rates. It can typically achieves 10:1 to 20:1 compression of full-color data with little perceptible loss in image quality. But it can be extended continuously from 30:1 to 50:1 compression with small to moderate defects, until 100:1 with very-low-quality results.

On the other hand, it takes longer to decode and view a JPEG image, compared to simpler formats, but when network transmission is involved, *the time savings from transferring a shorter file can be greater than the time needed to decompress the file*.

## How does it works

JPEG uses a lossy form of compression based on the discrete cosine transform (DCT). This operation converts frames from the spatial (2D) domain into the frequency domain. A perceptual model based loosely on the human psychovisual system discards high-frequency information, such as sharp transitions in intensity, and color hue. This high-frequency coefficients are characteristically small-values with high compressibility. So the algorithm packs them and send it to the output stream.

## About Compression Rate and Image Quality

Most JPEG compressors let you pick a file size vs. image quality tradeoff by selecting a quality setting. This quality setting is completely arbitrary and changes from one software to another, and **does not** mean rate of information preserved.

For good-quality, full-color source images, the default IJG quality setting (Q 75) is very often the best choice. If the image was less than perfect quality to begin with, there is a chance that dropping to Q 50 won´t result in objectionable degradation. Except for experimental purposes, never go above Q 95. Using Q 100 will produce a file two or three times as large as Q 95, but of hardly any better quality. If there is a need for a very small file with tolerable large defects, a Q setting in the range of 5 to 10 is about right.

## References

[JPEG image compression FAQ](http://www.faqs.org/faqs/jpeg-faq/part1/preamble.html)
