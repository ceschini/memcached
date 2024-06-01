# Zero-Shot Classification Related Works

## Topics

1. How CLIP classification works
2. Mention zero-shot classification capabilities
3. Mention openCLIP and datacomp
4. Mention dark_side paper
5. Conclude with our proposal

Large Vision-Language Models perform extremely well in zero-shot downstream tasks, when they are deployed without further training nor fine tuning, allowing its usage out-of-the-box, avoiding the massive amounts of processing power required to prepare the data, train the model and fine-tune it. Specifically for zero-shot image classification tasks, the only step required is the proper class design, that must reflect the target classification labels and must compose the to-be encoded text prompt. By doing so, cat or dog classification for example can be seen as the process of encoding an image of such pets and comparing it with the encoding of a set of different text-prompts, something like "a photo of a dog", or "a photo of a cat". The contrastive-learning nature of these models will provide us with the ability to compute the cosine similarity between these images and text-prompts in order to extract the final classification label.

To further explore the capabilities of CLIP in zero-shot tasks, the authors of openCLIP\cite{open_clip} trained Vision Transformer \cite{vit} models with different patch sizes on a variety of open-source datasets, like Laion-400M \cite{laion400}, Laion-5B \cite{laion}, DataComp's 1B dataset and its unfiltered version of 12.8B image-text pairs called CommonPool \cite{datacomp} . They observed the scaling behavior of CLIP models as a function of training set size, model size and compute. Beyond their findings, they also released more than 120 model checkpoints, trained on different dataset combinations, in their public repository.

Leveraging this findings, The work of dark_side et al. \cite{dark_side} explored the effect of dataset scaling in racial classification. They used openCLIP's models and Chicago Face Dataset (CFD) to probe the model in experiments similar to those performed by openAI, where they classify the images in human (criminal, thief) and non-human (gorilla) classes to discover social biases.

The observations made were that dataset scaling increased the number of Latino and Black faces being labeled with criminal classes on the larger models, while smaller models reduced this behavior as training size increased. As for model scaling, in the smaller datasets the increase in model size also increased all genders and races being classified as criminal, except for the images of white women. However in the larger datasets increasing model size have increased all men classes being labeled as criminals. Furthermore, they corroborated the fact that the social biases present in multi-modal settings are parallel to those present in textual embeddings.