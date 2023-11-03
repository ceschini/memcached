# Image Quality and Perception Metrics and Losses

Based on Ding et al. Comparison of Image Quality Models for Optimization of Image Processing Systems [1].

## Introduction

Image Quality Assessment (IQA) models are built to predict the perceptual quality of images, and can be used to optimize and evaluate image processing methods and systems. There are different approaches that excells on different tasks. Here we will broadly describe them and will focus on the best metrics and losses for super-resolution problems.

## Taxonomy

Full-reference IQA methods can be broadly classified into five categories:

**Error visibility methods**. Measure the distance between a given signal pixels and its corresponding noisy output. Error visibility methods, like MSE, have useful properties for optimization, like differentiability and convexity, but are poorly correlated with perceived image quality.

**Structural similarity (SSIM) methods**. Compute the similarity of local image structures, which are composed of three conceptually independent components - luminance, contrast and structure. They were widely used as an IQA metric and inspired many models based on feature, gradient or saliency similarities.

**Information theoretic methods**. Attempts to measure some approximation between the information present in the perceived reference and the distorted image by statistically modelling the image source, the distortion process and the HVS. The prototype is the visual information fidelity (VIF) measure.

**Learning-based methods**. Learn the relationship between input images and the perceptual distance using a large set of examples and supervised machine learning methods. Became popular due to DNNs, but are prone to overfitting due to high dimensionality of the input space.

**Fusion-based methods**. Combine existing IQA methods to build a "super-evaluator" that exploits the diversity and complementarity of the incorporated methods.

## Loss Functions

Ding et al. showed that for super resolutions tasks, learning-based models optimized with LPIPS and DISTS cost functions were able to perform better than any other method. Because of that, we will briefly introduce them here.

### DISTS

The Deep Image Structure and Texture Similarity metric [2] is designed with explicit tolerance to texture resampling (e.g., replacing one patch of grass with another). It is based on a VGG variant, and combines structure and texture similarity measures to be robust.

They showed how the most commonly used IQA metrics are over sensitive to texture resampling, when texture structures are modified to improve perceptual quality. Because most of them are pixel-wise, they wrongfully penalize this kinds of modifications, giving a higher score to low quality original texture patches than high quality resampled textures.

### LPIPS

The Learned Perceptual Image Patch Similarity model [3] computes the distance between deep representations of two images by leveraging the feature maps of different DNN architectures. They proved that, during training, convolutional networks may develop representations which transfer well to perceptual distances.

They studied these low-trained convolutional layers can better measure visual perception compared to well-established metrics such as PSNR and SSIM. By computing the cosine distance between the normalized and scaled feature stacks of a given reference and distorted patch, they were able to train a model to measure the perceptial similarity of a given reference and noise image pair.

## Metrics

Quality assessment metrics have recently focused on perceptual details rather than a more quantitized approach. The above mentioned LPIPS and DISTS are some examples of metrics that have been used in recent papers. ERQA is another metric that showed this goal of measuring details restoration over exact pixels restoration.

### ERQA

The Edge-restoration Quality Assessment for Video Super-Resolution [4] metric have been developed to improve details restoration and prevent hallucinations on super-resolution models.

Assuming a basis hypothesis that edges are significant for detail preservation, ERQA metric estimates how well a model can restore edges by applying F1 score over the Canny edge maps of the GT and the distorted frame. They compensate local and global shifts that are commonly present in generations, and calculate precision and recall as follows:

- **True Positive**: Number of pixels detected as edge in both ground-truth (GT) and distorted frames.
- **False Positive**: Number of pixels detected as edges only in distorted frame.
- **False Negative**: Number of pixels detected as edges only in ground-truth frame.

![ERQA example](../imgs/erqa-example.png)

---

## References

1. Ding, K. et al. (2021) **‘Comparison of Image Quality Models for Optimization of Image Processing Systems’**, International Journal of Computer Vision, 129(4), pp. 1258–1281. Available at: <https://doi.org/10.1007/s11263-020-01419-7>.
2. Ding, K. et al. (2020) **‘Image Quality Assessment: Unifying Structure and Texture Similarity’**, IEEE Transactions on Pattern Analysis and Machine Intelligence, pp. 1–1. Available at: <https://doi.org/10.1109/TPAMI.2020.3045810>.
3. Zhang, R. et al. (2018) **‘The Unreasonable Effectiveness of Deep Features as a Perceptual Metric’**. arXiv. Available at: <http://arxiv.org/abs/1801.03924>.
4. Kirillova, A. et al. (2022) **‘ERQA: Edge-restoration Quality Assessment for Video Super-Resolution’**:, in Proceedings of the 17th International Joint Conference on Computer Vision, Imaging and Computer Graphics Theory and Applications.SCITEPRESS - Science and Technology Publications, pp. 315–322. Available at: <https://doi.org/10.5220/0010780900003124>.
