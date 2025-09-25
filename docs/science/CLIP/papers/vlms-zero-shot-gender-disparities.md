# Vision-Language Models Performing Zero-Shot Tasks Exhibit Gender-based Disparities

The authors explore gender bias in vision language models by probing CLIP and two other models built upon it. They provide an image and a set of "concepts" or ground-truth labels that are present in the image. Then, they ask CLIP to do multi-label classification, Detic to detect objects and LSeg to semantic segmentate the image. These two auxiliary models are employed to support the given CLIP classification.

They argue that the bias present in word embeddings are parallel to the biases in zero-shot vision-language models.

> We use Detic's bounding box object predictions and LSeg's pixel-by-pixel classifications as **binary indicators** of the model's recognition of the concept in the image.

They create two sets of labels based on WordNet concepts that refer to women and men.

> "A fair model should have generally comparable performance across demographic groups."

So, they use the difference of average precision (AP) between images containing men and images containing women for every object label. In their example, a positive AP difference signal means a better measurement for images containing men, and negative for images containing women. For example, A high AP diff in the concept *necktie* means that the model performs better for images containing men over images containing women.

They also argue that a common practice of reporting the mean average precision across all concepts as a single, summary statistic of model performance can potentially mask the disparities between them, and lend a false assurance of model consistency across demographic groups (unbiased).

To explore the impact of the dataset in the studied tasks, they employed both the Visual Genome and the COCO datasets. And in a range of concepts they showed that the bias is present in both of them, therefore it is not dataset specific.

The calculation of cosine similarity is done as follows: First, the synset embeddings are averaged with the canonical gender term. Then, the difference between each gender group embeddings is calculated and the final number describes the kind of gender bias, where positive could mean a "man" concept and negative a "woman" concept.

> **For each gender group, we *average the cosine similarity between the synset embeddings and each gender term*. We then use the difference in the cosine similarities between the "man" and "woman" gender groups as an indicator of the social bias for that concept**

but this synset embedding is a synonym of the gender, or is it each of the concepts present, such as "umbrella", "baseball bat", etc?

In the Visual Genome dataset, each bounding box is a WordNet synset, and they represent both objects and people. The authors created two sets of labels, one for each gender group, using WordNet concepts that refer to them. Then, they use the list of synsets for each group to filter the images.

"we retain only the images that are annotated with a single gender label and assign them to the binary gender groups"
