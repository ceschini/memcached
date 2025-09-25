## Topics

- Facial attribute recognition and ethnicity prediction
	- overview of the problem
	- problem formulation
	- common approaches
- Vision Language Models
	- Contrastive learning
	- ConVirt
	- CLIP
	- Zero-shot learning and prediction
- Visual Prompt Tuning with CoOp

## Facial attribute recognition and ethnicity prediction

 Facial attribute recognition is a computer vision task often related to the problem of soft biometrics and the identification of an individual based on their physiological and behavioral characteristics [1], and it was introduced to the computer vision community in 2008 in the field of image search [2]. Predicting age, gender, race and emotion using an image of a face is at the core of facial attribute recognition, and could be employed in several realworld applications such as surveillance, security, and demographic studies. Among these traits, ethnicity is a major aspect of society with profound linkages to a variety of environmental and social concerns [3]. Over the years, many approaches and developments have been made on the subject of ethnicity prediction (ER), ranging from mathematical and statistical approaches, to the use of specific features such as skin and hair color, focusing on  particular regions of the face, to the recent popularization of deep learning models.

## Vision Language Models

Recent advances in Large Language Models (LLMs) and Multimodal Vision-Language Models (VLMs)  showed good performance on various image recognition benchmarks, thanks to its ability to correctly align visual and textual features in a shared space [4]. Contrastive Language-Image Pre-training, or CLIP, is a VLM based on contrastive learning, a strategy that leverages pairs of positive and negative examples to learn how to predict similarities between positive pairs, and differences between the negative ones.

CLIP is based on a cross-entropy loss function called InfoNCE, in which the goal is to learn how to calculate a distance metric, such as cosine similarity, to predict the most likely pair of examples that is closest to the real distribution, while distancing all negative examples from it [5]. In CLIP, positive pairs are defined as one image and its corresponding ground truth caption while the negative examples are defined as the same image but with all the other possible captions. It then trains a vision and a text encoder to map the representation of an input image and a given textual label to similar embedding vectors using a contrastive loss [4]. See Figure REF for a summary of the CLIP approach.

![](docs/img/clip-architecture.png)

CLIP models were trained in a dataset containing 400 million image-text pairs, and come in two available architectures, one based in the original ResNet-50 [6] and the other based on Vision Transformer (ViT) [7]. ResNets leverage residual learning to address the degradation problem of deep neural networks by letting the stacked layers fit a residual mapping instead of the original, unreferenced one. Formally, by denoting the underlying mapping as H(x), the authors let the stacked nonlinear layers fit another mapping of F(x) := H(x) - x. The original mapping is recast into F(x) + x [6]. This is better described in Figure REF.

![](docs/img/residual-building-block.png)

ResNets were the state-of-the-art in machine translation (the automatic translation of written text from one natural language to another [8]) but they are highly sequential and suffer from long sentences [9]. The idea of Transformers is to ditch recurrence and convolutions, instead relying entirely on attention mechanisms to map the relationships between input and output sequences.

Vision Transformers applies the standard Transformer architecture directly to images by splitting it into patches and providing the sequence of linear embeddings of these patches as an input to the model. By doing so, image patches are treated the same way as tokens (words) in an NLP application. When trained on mid-sized datasets, their performance is sub-optimal, due to a lack of inductive biases inherent to CNNs, such as translation equivariance and locality. However, when trained on larger datasets (14M-300M images), they were able to trump inductive bias and achieve performances comparable or better than the current state of the art [7]. The standard Transformer receives as input a 1D sequence of tokens embeddings. To handle 2D images, the image is reshaped into a sequence of flattened 2D patches, that are flattened and mapped to the transformer dimensions with a *trainable linear projection*. A class token is prepended to the sequence of embeddings, this is a learnable embedding whose state at the output of the Transformer encoder serves as the representation of image features. Along with each patch embedding is a standard learnable 1D position embedding to retain positional information. See Figure REF for more details.

![](docs/img/vit-architecture.png)

## Visual Prompt Tuning and Context Optimization

According to Radford et al. [4], CLIP's features outperform the features of the best ImageNet model on a wide variety of datasets, and showed good robustness to Natural Distribution Shifts. This paved the way of a new foundation model for vision, replacing the standard ImageNet pre-trained fine-tuning. Now, for a variety of different recognition tasks, the most accurate models are adapted by fully fine-tuning the pre-trained foundation models such as CLIP [10]. However, the sheer amount of computational power required to store and deploy a separate copy of the backbone of these large models is infeasible, especially for modern Transformer-based architectures comprised of 632 million parameters, such as ViT-Huge [7].

Visual Prompt Tuning was proposed in 2022 by Jia et al. [10] to solve this problem. By freezing the pre-trained Transformer backbone and introducing a few learnable parameters to the input, they were able to efficiently fine-tune models to several downstream tasks, even surpassing full fine-tuning in some cases. According to the authors, "[..] these additional parameters are simply prepended into the input sequence of each Transformer layer and learned together with a linear head during fine-tuning."

Prompting [11] originally refers to the ability of prepending a template text to the inputs of a language model in order to make it better understand the task, or even adapt in to unseen tasks in a zero-shot setting [12]. Prompt tuning is the idea that the prompt can be treated as a task-specific learnable feature, in order to correctly "ask a question" to a language model [13].


## References

1. Can soft biometric traits assist user recognition? 2004 Jain et al.
2. Attribute and simile classifiers for face verification. 2009 Kumar et al.
3. Intelligent deep learning based ethnicity recognition and classification using facial images
4. Learning Transferable Visual Models From Natural Language Supervision (CLIP). Radford et al. 2021
5. An introduction to vision-language modeling. 2024 Bordes et al
6. Deep residual learning for image recognition He et al. 2016
7. An image is worth 16x16 words: Transformers for image recognition at scale Dosovitski et al. 2021
8. Neural Machine Translation: A Review
9. Attention is All you Need
10. Visual Prompt Tuning. Jia et al. 2022
11. Pre-train, prompt, and predict: A systematic survey of prompting methods in natural language processing.
12. Language models are few-shot learners. Brown et al. 2020
13. Prefix-tuning: Optimizing continuous prompts for generation. Li et al. 2021
14. Learning to Prompt for Vision-Language Models (CoOp)
