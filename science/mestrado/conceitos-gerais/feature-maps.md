# Feature or Activation Maps

Feature or activation map is the non-linear output of a convolution layer. It’s the resulting matrix of the convolution operation, where we see what have stimulated the layer’s neurons.

On CLIP code, “logits” means the feature map, with non-normalized features, which are then passed to softmax in order to map each class to one output value.

## Refs
- [What is the meaning of the word logits in TensorFlow?](https://stackoverflow.com/questions/41455101/what-is-the-meaning-of-the-word-logits-in-tensorflow#43577384)
- [What is the definition of a "feature map" (aka "activation map") in a convolutional neural network?](https://stats.stackexchange.com/questions/291820/what-is-the-definition-of-a-feature-map-aka-activation-map-in-a-convolutio)