# Related Work

## Estrutura

- Ethnicity classification (check [[ethnicity-classification-lit-review]])
- Face datasets
- Zero-shot prediction
- Classification bias
- Hook to our motivation and research question

## Ethnicity classification

Several works have been produced about ethnicity classification over the years. Starting from classic mathematical and statistical approaches, some based on soft biometric features such as skin and hair color, focused on a specific part of the face, until more recent deep learning and multitask learning approaches. The following sections will summarize these works, based on the Lyle et al. [0] literature review on the subject.

### Mathematical and statistical

Lu et al. (2004) [4] classified Asian and Non-asian using Linear Discriminative Analysis (LDA) to gray-scale facial images in different scales. At each scale, a classic appearance-based face recognizer based on the LDA representation is developed under the Bayesian statistical decision framework. LDA is a well-known statistical method to project a given multidimensional data to a lower dimension, in a way that efficiently distinct two or more classes [5].

Manesh et al. (2010) [6] adpoted a method that uses the Golden ratio mask by employing decision-making rules on different face zones, and then using SVM to classify the extracted Gober features into Asian and non-asian.

Guo et al. (2014) [7] used Canonical Correlation Analysis (CCA), and partial least squares (PLS) to broadcast ethnicity, gender and age classification. They found that these methods can do dimensionality reduction and jointly estimate age, gender and ethnicity within a single step.

### Skin and hair color

Wu et al. (2004) [8] proposed a Look-Up-Table depending on boosting method to extract demographic features such as race and gender from human faces. The proposed method composes three modules, including facial region detection, facial feature extraction, and demographic classification.

Demirkus, Garg and Guler (2010) [9] proposed two algorithms, which are pixel intensity values and the Biologically Inspired Model (BIM) to classify race based on hair and face color. Also using an SVM classifier into three ethnic groups: Asian, African and Caucasian.

Muhammad et al. (2012) [10] proposed a race classification method based on two local descriptors: Weber Local Descriptor (WLD) and Local Binary Pattern (LBP). It also used different distance classifiers, such as Chi-square, Euclidean, and City-block. Fusing WLD with LBP and using City-block as a distance classifier performed better than the rest.

Becerra-Riera et al. (2018) [11] proposed a method to codify information about different face regions, with appearance and geometric features such as color and shape. They classified face images from the FERET dataset into three groups: white, black and asian.

### Focusing on particular region in the face

Lyle et al. (2012) [12] proposed a method that selected texture features from the periocular region by applying LBP feature-based technique to distinguish between asians and non-asians.

Sun et al. [13] proposed a classification technique dependent on a hierarchical visual codebook to classify iris images.

Xie et al. (2012) [14] focused on the upper part of the face and incorporated facial color-dependent features and Kernel class-based features to identify race.

Hosoi et al. [15] proposed a new method that cooperates retina sampling and the Gabor wavelet features to be classified using SVMs.

Roomi et al. [16] suggested a face detection method that used the Viola-Jones algorithm. The authors focused on extracting features from different regions, including lip color, skin color, and forehead area.

Marzoog et al. [17] proposed a model based on the geodesic distance technique as a feature extractor. Principal Component Analysis (PCA) has been used as dimension reduction. SVM and KNN have been used as classifiers. This method achieved 100% accuracy in FERET dataset.

### Deep learning

Zhang et al. (2016) [18], Wang et al. (2016) [19], Wei et al. (2015) [20], Khan et al. (2013) [21] all used CNNs to classify. Vo et al. (2018) [22] suggested two race recognition frameworks, with CNN and VGG.

(More about CNNs here?)

### Multitask learning

From Facial Attribute Recognition: A Survey Thom and Hand 2020

Rud et al. [3] developed MOON: Mixed Objective Optimization Network, capable of learning all attributes at once while at the same time adjusting for label imbalance. They utilized the popular VGG-16 network architecture [1] and trained from random initialitzation on CelebA.

Ehrlich et al. [2] used Restricted Boltzmann Machine (RBM) rather than a CNN, and trained their model with both CelebA face images and facial landmark points as inputs. They extended RBMs to handle multiple tasks and multiple inputs, creating a Multitask Multimodal RBM (MTM-RBM).

## Context Learning

To efficiently adapt a pre-trained CLIP model to different downstream tasks, one must build the correct prompt, a technique commonly called prompt engineering [ref?]. A standard prompt for vision language models is "A photo of a \<class>", but if trying to describe textures, for example, adding the word "texture" in the end, or even removing "a photo of" could drastically improve results. This often requires domain expertise and time to do it correctly. The authors of [14] propose a method to *learn* the better prompt used in such tasks. They called it *Context Optimization* (CoOp), a simple approach specifically adapting CLIP-like vision-language models.

CoOp models a prompt's ***context words*** with learnable vectors while the entire pre-trained parameters are kept fixed. There are two implementations, an unified context and class-specific context. Unified context is better to classes that share a common context, while class-specific context will learn a specific set of context tokens for each class, more suitable for fine-grained categories.

In CoOp, the prompt is designed as a sequence of learnable vectors, followed by a target class. This target class is a word token that will be replaced from its corresponding word embedding vector by the text encoder. The goal is to learn the best combination of such vectors in order to predict the correct class name.

This class token can be placed at the end of the sequence, or in the middle. The advantage of a middle class token is the ability to fill the latter word cells with supplementary descriptions, such as "a type of food", or cut off the sentence earlier by using termination signal such as full stop.

**Training** is performed by minimizing the standard classification loss based on the cross-entropy, *and the gradients can be back-propagated all the way through the text encoder, making use of the rich knowledge encoded in the parameters to optimize the context*.

Though the performance is better than manual prompting, the results of CoOp are relatively difficult to interpret, as the resulting prompt is normally written gibberish.
### References 

0. Ethnicity Classification From Face Images, Literature Review
1. Very deep convolutional networks for large-scale image recognition. Simonyan K 2014
2. Facial attributes classification using multi-task representation learning.
3. Moon: a mixed objective optimization network for the recognition of facial attributes
4. Ethnicity Identification from Face Images. Lu et al. (2004)
5. Pattern Classification Duda et al. (2000)
6. Facial Part Displacement Effect on Template-Based Gender and Ethnicity Classification. Manesh et al. (2010)
7. A Framework for Joint Estimation of Age, Gender and Ethnicity on a Large Database. Guo et al. (2014)
8. Facial Image Retrieval Based on Demographic Classification. Wu et al. (2004)
9. Automated Person Categorization for Video Surveillance Using Soft Biometrics Demirkus et al. (2010)
10. Race Classification from Face Images Using Local Descriptors. Muhammad et al. (2010)
11. On Combining Face Local Appearance and Geometrical Features for Race Classification. Becerra-Cierra et al. (2018)
12. Soft Biometric Classification Using Periocular Region Features. Lyle et al. (2012)
13. Iris Image Classification Based on Hierarchical Visual Codebook. Sun et al. (2014)
14. A Robust Approach to Facial Ethnicity Classification on Large Scale Face Databases
15. Ethnicity estimation with facial images. Hosoi et al. (2004)
16. Race Classification Based on Facial Features. Roomi et al. (2011)
17. Gender and Race Classification Using Geodesic Distance Measurement. Marzoog et al. (2022)
18. Deep Neural Network for Halftone Image Classification Based on Sparse Auto-Encoder. Zhang et al. (2016)
19. Facial Ethnicity Classification with Deep Convolutional Neural Networks. Wang et al. (2016)
20. HCP: A Flexible CNN Framework for Multi-Label Image Classification. Wei et al. (2015)
21. A Comparative Analysis of Gender Classification Techniques. Khan et al. (2013)
22. Race Recognition Using Deep Convolutional Neural Networks. Vo et al. (2018)

## Face datasets

In 2008, Kumar et al. [1] built a face search engine called FaceTracer, and released a dataset of the same name. At the time of publication, the dataset consisted of over 3.1 million face images, 17k of which were manually labeled with 10 attributes: age, gender, race, hair color, eyewear, mustache, expression, blurry, lighting and environment. A year later, the same group developed a face verification method and introduced a new dataset, called PubFig [2], with 60 million images of 200 people collected from the internet. Labels for race were present in binary form, such as Caucasian, Asian, Indian and Black, but the vast majority of them were Caucasian.

In 2015 and the advent of deep learning, Liu et al. [3] introduced two large-scale benchmark datasets for facial attribute recognition: CelebFaces Attributes (CelebA) and Labeled Faces in the Wild Attributes (LFWA). CelebA contains over 200k images each labeled with 40 binary attributes, which are a subset of those used in [4]. LFWA is the popular LFW dataset but with the same binary attribute labels of CelebA. These two datasets were the first large-scale datasets introduced for the problem of facial attribute recognition from images.

## The FairFace Dataset

Despite the robust volumes of data available, the vast majority of public, large-scaled face image datasets used in these studies, tends to be strongly biased toward Caucasian faces, underrepresenting key demographic groups and limiting the applicability of the models trained with them. To this end, Karkkainen and Joo [5] developed the FairFace dataset, a novel face image dataset containing 108,501 images of 7 race groups, with labels related to race, gender and age. The images were sourced from the YFCC-100M Flickr dataset [6]. Labeled racial groups are: White, Black, Indian, East Asian, Southeast Asian, Middle Eastern, and Hispanic Latino. For gender they only provide a binary classification of Male or Female. For age, there are several interval groups, more specifically 0-2, 3-9, 10-19, 20-29, 30-39, 40-49, 50-59, 60-69, and 70+. Images have a fixed resolution of 224 x 224 pixels, with two available crop padding options, 0.25 and 1.25. Figure REF shows some examples of images from the dataset. The number of samples for each race category can be seen in Table REF. In Table REF we show the number of samples for each age, and in Table REF, the number of samples for each gender.

(Taken from vlms-facial-attr-recon paper)

![](docs/img/fairface-examples.png)

### References 

1. Facetracer: a search engine for large collections of images with faces.
2. Attribute and simile classifiers for face verification
3. Deep learning face attributes in the wild
4. Describable visual attributes for face verification and image search.
5. Fairface: Face attribute dataset for balanced race, gender, and age for bias measurement and mitigation.
6. Yfcc100m: The new data in multimedia research

## Zero-shot prediction

Large Vision-Language Models perform extremely well in zero-shot downstream tasks \cite{clip}, when they are deployed without further training or fine-tuning. Hence, they can be used out-of-the-box, avoiding the massive amounts of processing power required to prepare the data, train the model, and fine-tune it. Specifically for zero-shot image classification tasks, the only step required is the proper class design, which must reflect the target classification labels and compose the text prompt. For example, a two-class problem involving cats and dogs can be seen as the process of encoding an image of such pets and comparing it with the encoding of a set of different text-prompts, such as ``a photo of a dog'' and ''a photo of a cat''. The contrastive-learning nature of these models will provide us with the ability to compute the cosine similarity between these images and the text-prompts to extract the final classification label.

To further explore the capabilities of CLIP in zero-shot tasks, the authors of openCLIP\cite{open_clip} trained several ViT models with different patch sizes and number of trainable parameters on a variety of open-source datasets, like Laion-400M \cite{laion400}, Laion-5B \cite{laion}, DataComp-1B and its unfiltered version with 12.8B image-text pairs called CommonPool \cite{datacomp}. They observed the scaling behavior of CLIP models as a function of training set size, model size and compute. Beyond their findings, they also released more than 120 model checkpoints, trained on different dataset combinations, in their public repository \cite{open_clip_repo}. Gadre et al.~\cite{datacomp} showed that scaling is important, but the quality of the dataset also plays an important role. In fact, their results indicated that using a ``good'' subset of massive datasets can yield better results than training using all data.

In \cite{learn_name_classes} the authors draw attention to the fact that VLMs suffer from high sensitivity to class-prompt textual content and from the complexity to adapt to new, unseen data. They propose to leverage the concept of textual inversion \cite{textual_inversion} to adapt models to new data and thus address suboptimal, and potentially wrong, labels. To do so they describe categories of interest by applying available data to learn optimal word embeddings, as a function fo the visual content.

In the work of \cite{tai-dpt} the contrastive-learning nature of image and text encoders found in VLMs is explored to treat texts as images for prompt tuning, alleviating the need of visual data to learn prompts and reducing the impact of data-limited and label-limited settings.

The authors of \cite{robustification} observe how language models contain actionable insights that can be exploited to improve themselves or other models, and showed how it is possible to improve model's zero-shot performance without labeled data, training or fine-tuning or manual identification.

According to AlDahoul et al. [1] the direct applications of VLMs in facial attribute recognition are still unexplored. As such, they developped a multi-task classifier that leverages the capabilities of VLMs to simultaneously identify multiple human facial attributes, specifically race, gender, age and emotion. They explored commercially available Multimodal Large Language Models (MLLMs) such as GPT [2], GEMINI [3], LLAVA [4], PaliGemma [5] and Florence2 [6] to classify facial attributes using various datasets, such as FairFace, AffectNet [7], and UTKFace [8]. The facial recognition application was formulated as a visual question-answer task, see Figure REF for more detail. The authors combined East and South Asian into "Asian" group, and GPT-4o gave highest metrics with 76.4% accuracy. With South and East Asian, it dropped to 68%. Age groups were also combined into 5 categories: 0-9, 10-19, 20-39, 40-59, 60+. GPT-4o achieved highest accuracy 77.4%. For emotion, all models performed poorly, with high 49% accuracy and low 38.8%.

![[docs/img/vlm-far-prompt-example.png]]

### References

1. Exploring Vision Language Models for Facial Attribute Recognition: Emotion, Race, Gender, and Age. AlDahoul et al. 2024
2. Language models are few-shot learners
3. Gemini: a family of highly capable multimodal models
4. Visual instruction tuning
5. Paligemma: A versatile 3b vlm for transfer
6. Florence-2: Advancing a unified representation for a variety of vision tasks.
7. Affectnet: a new database for facial expression, valence, and arousal computation in the wild.
8. Utkface dataset. https://susanqq.github.io/UTKFace/.

## Classification Bias

Following the initial experiments performed by the CLIP authors in Section 7.1 Bias, several works have focused on exploring social biases present in vision language models. These initial experiments detected racial and gender biases in image classification tasks when prompting CLIP with human and non-human labels, and gender-agnostic profession labels.

According to \cite{fairer_clip}, model bias can be classified based on the dependencies between the data attributes. Such attributes can be dependent or independent from each other. Correlations between dependent attributes, such as high cheekbone and sex, are called an intrinsic dependence, while the correlation between independent attributes, such as hair color and sex, are called a spurious correlation. The authors claim that debiasing methods are commonly focused on the spurious correlations, requiring or not ground-truth labels and use iterative methods computationally costly. Their method is supposed to address all these issues. They developed a method to learn the biases from text and image embeddings of CLIP and deploy their model as a post-processing stage after CLIP encoding and prior to cosine similarity classifiers.

The authors of \cite{biased_prompts} propose a calibration loss that minimizes the discrepancy between a pair of prompt embeddings that contain biases. By doing so, the embeddings of both male and female versions of a given prompt should be similar, and as such, debiased. Only focusing on the text embedding was sufficient to improve group robustness of zero-shot models, corroborating the claims of parallelism between VLMs and LLMs biases.

Leveraging this findings, The work of \citeonline{dark_side} explored the effect of dataset scaling in racial classification. They used openCLIP's models and Chicago Face Dataset (CFD) to probe the model in experiments similar to those performed by openAI, where they classify the images in human (criminal, thief) and non-human (gorilla) classes to discover social biases.

The observations made were that dataset scaling increased the number of Latino and Black faces being labeled with criminal classes on the larger models, while smaller models reduced this behavior as training size increased. As for model scaling, in the smaller datasets the increase in model size also increased all genders and races being classified as criminal, except for the images of white women. However in the larger datasets increasing model size have increased all men classes being labeled as criminals. Furthermore, they corroborated the fact that the social biases present in multi-modal settings are parallel to those present in textual embeddings.

The ease of use and feasibility of such models could improve the work of those that lack the technical knowledge or the resources to build, deploy and use vision models in their activities. However, the unaware use of these tools with bad class design could be dangerous, specially for those oppressed demographic groups.

Based on these findings, our work aims to further explore the impact of model and data scaling of CLIP models for the zero-shot race recognition task, while also observing the power of prompt tuning and ensemble in improving classification and fairness (TODO) measures.
