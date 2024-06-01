# The Dark Side of Dataset Scaling: Evaluating Racial Classification in Multimodal Models

The authors explored the effect of dataset scaling in racial classification. They used openCLIP models and Chicago Face Dataset (CFD) to probe the model in experiments similar to those performed by openAI, where they classify the images in human (criminal, thief) and non-human (gorilla) classes to discover social biases. They argue that the social biases present in multi-modal settings are parallel to those present in textual embedding.

Larger models increased criminal classification of Latino and Black images as the scale of datasets increased. Smaller models reduced this behavior as the scale increased.

On the smaller dataset, increasing models have increased all genders and races being classified as criminal, except white woman. For the larger dataset, increasing models have increased all men classes being labeled criminal.

They explored the 14 Vision Transformer-based models available in openCLIP using only LAION-400M and LAION-2B.

**For the classes they copied from CLIP paper**: "... the remaining classes were verbatim extracted from *Section 7.1 Bias* of the OpenAI CLIP paper"

**For the choice of "A photo of a \<class>"**: "As advocated in the `Interacting with CLIP` Jupyter notebook shared at open_clip in the context of *Zero-Shot Image Classification for CIFAR-100 dataset*."

**IDEA**: In here, we could join this study with the Hall et al. one and explore how data scaling effects the gender-based disparities. Basically grabbing the settings of data-scaling and applying to the gender-based experiments (necktie for men, bow for woman etc).
