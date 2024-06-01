# Related Work: Bias

## Topics

- Describe existing bias
- Describe gender classification bias
- Talk about gender classification papers

Following the initial experiments performed by the CLIP authors in Section 7.1 Bias, several works have focused on exploring social biases present in vision language models. These initial experiments detected racial and gender biases in image classification tasks when prompting CLIP with human and non-human labels, and gender-agnostic profession labels.

in \cite{gender_disparities} the authors explore gender bias in vision language models by probing CLIP and two other models built upon it, Detic \cite{cetic} for bounding box detection and LSeg \cite{lseg} for semantic segmentation. To compare the performance of the models in image classification, object detection and segmentation, they used the Visual Genome dataset \cite{visual_genome}, where each image contain bounding boxes for persons and gender-neutral objects like bag, necklace and sweater. All models performed better for images containing women for these three objects, while performing better for images containing men for objects like necktie, wheel and hat. They also point that a common practice of reporting the mean average precision across all concepts as a single, summary statistic of model performance can potentially mask the disparities between them, and lend a false assurance of model consistency across demographic groups (unbiased). To explore the impact of the dataset in the studied tasks, they employed both the Visual Genome and the COCO \cite{coco_dataset} datasets. In a range of concepts they showed that the bias is present in both of them, therefore it is not dataset specific. Furthermore, They found that the bias in word embeddings parallel the biases in the zero-shot vision-language models, and as such verifies the claims that biases from language models are passed to vision models learnt from it.

According to \cite{fairer_clip}, model bias can be classified based on the dependencies between the data attributes. Such attributes can be dependent or independent from each other. Correlations between dependent attributes, such as high cheekbone and sex, are called an intrinsic dependence, while the correlation between independent attributes, such as hair color and sex, are called a spurious correlation. The authors claim that debiasing methods are commonly focused on the spurious correlations, requiring or not ground-truth labels and use iterative methods computationally costly. Their method is supposed to address all these issues. They developed a method to learn the biases from text and image embeddings of CLIP and deploy their model as a post-processing stage after CLIP encoding and prior to cosine similarity classifiers.

The authors of \cite{biased_prompts} propose a calibration loss that minimizes the discrepancy between a pair of prompt embeddings that contain biases. By doing so, the embeddings of both male and female versions of a given prompt should be similar, and as such, debiased. Only focusing on the text embedding was sufficient to improve group robustness of zero-shot models, corroborating the claims of parallelism between VLMs and LLMs biases.

The ease of use and feasibility of such models could improve the work of those that lack the technical knowledge or the resources to build, deploy and use vision models in their activities. However, the unaware use of these tools with bad class design could be dangerous, specially for those oppressed demographic groups.

From these works we can better understand the impact and the presence of biased points of view regarding gender in our society, captured by these large models trained unconstrained from data scraped from every corner of the web. While this is not a definitive solution, it is of the utmost importance to mitigate these kind of bias from "real world" applications that rely on the judgment of these models for its decision making.