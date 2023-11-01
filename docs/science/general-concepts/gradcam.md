# GradCAM

The Class Activation Mapping (CAM) technique highlights the regions of the image that were most important for a given neuron. They use the last layer before global pooling to extract the excited neurons that are responsible for detecting regions of an image, by highlighting the pixels that caused this last layer to activate, they can show where the network is "looking" when it outputs a given classification.

Grad-CAM is a generalization of this technique, enabling it to be applied to any kind of CNN.

## Snowballing

-  **automatically naming neurons**
	- D. Bau, B. Zhou, A. Khosla, A. Oliva, and A. Torralba. **Network dissection: Quantifying interpretability of deep visual representations.** In Computer Vision and Pattern Recognition, 2017. Selvaraju et al., 2020, p. 22
- **CAM paper**
	- B. Zhou, A. Khosla, L. A., A. Oliva, and A. Torralba. **Learning Deep Features for Discriminative Localization**
