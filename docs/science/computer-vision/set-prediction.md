# Set Prediction

The output of many computer vision tasks, including image tagging and object detection, are naturally expressed as sets of entities rather than vectors. As opposed to a vector, the size of a set is not fixed in advance, and it is invariant to the ordering of entities within it. An example of set prediction is the problem of multi-class image classification¹.

In multi-class image classification, models are trained using as input a pair of image and label set. These models are commonly composed of an *image representation* backbone, followed by a *set prediction* module, which are stacked together and trained end-to-end. 

Image representation backbones transform an input image into a representation of extracted features. The set prediction module takes as an input the image representation and outputs set elements².

## Refs

¹ Deepsetnet: Predicting sets with deep neural networks
² Elucidating image-to-set prediction: An analysis of models, losses and datasets
