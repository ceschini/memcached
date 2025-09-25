# Introduction

## Topics

- About VLMs: How they work, what are good, about zero-shot and its problems
- About Bias Related Works: What did they discover about bias, how they tackle it
- About Gender Classification Related Works: What they evaluate and how. Bias?
- About our work
- About our results
- About our findings

### About VLMs

Large Vision Language Models  such as CLIP \ref{clip} and ALIGN \ref{align}, proved overwhelming zero-shot and few-shot capabilities in a variety of downstream tasks such as image classification, image and text retrieving, action recognition, geo-localization and many more, while providing a better starting point for fine-tuning models than the previous standard of Imagenet's pre-trained weights \ref{clip}.

This is due to its contrastive learning approach \ref{contrastive_learning} and the development of large scale datasets comprising billions of image-text pairs automatically scraped from the publicly available sources of the web \ref{laion}.

The large scale of such models makes its use in a zero and few shot manner interesting, due to its prohibitive computational and informational costs for training and scaling. Recent works have explored these capabilities for image classification \ref{tai-dpt} \cite{meta_learning} \ref{lens} \ref{train_free_adaption}, primarily using textual prompt manipulation techniques.

But the shortcomings of such unsupervised learning approach can be critical when evaluated over the lens of its societal impacts in real-world tasks \ref{robots_malignant}. These Large VLMs are trained without supervision from the images and texts contained over the web, and are prone to learn the spurious correlations between them. The unlimited flexibility of the class design of such models could result on its misguided used, justifying and amplifying stereotypical and unfair social relations \ref{dark_side} \ref{gender_disparities}.

The nature and the details about the dataset that these models are trained are treasured as business secrets of the highest value by the private companies that built them, leaving researchers and the society in the dark. To shed some light in these issues, the work of openCLIP \ref{open_clip} is substantial due to the open release of large scale models and datasets, extending the initial settings of its owners.

As an intersectional study, this work explores the effects of prompt ensemble in gender classification with model and data scaling as a background. We show the power of the addition of semantic context and synonyms to the textual-prompt to improve zero-shot gender classification.