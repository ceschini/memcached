## Topics

1. Explain what models and datasets we used
2. Describe how our prompts were built and formed
3. Run Experiments 

## 1. Models and datasets

In order to explore the effect of data and model scaling, we chose a constant model architecture with 5 scaling checkpoints, and 4 data sources with two or more scaling sizes, that were currently available from openCLIP. For the first three checkpoints (B-32, B-16 and L-14) we selected datasources from openai WIT's, LAION 400M  and 2B, DataComp subsets sizes medium, large and extra large, and CommonPool subsets sizes medium, large and extra large. As for the two biggest models (H and G variants) only the laion2b datasource was available. openCLIP's datasources follow a simple nomenclature, where the name of the source (such as laion400m or commonpool) is followed by its respective subset size (such as m for medium and xl for extra-large), with the optional addition of an "e", "s" or "b" flag, respectively meaning training epochs, number of samples seen and global batch size. As such, the datasource laion2b_s34b_b79k means that the model was trained on the LAION-2B subset, with 34 billion samples seen with a global batch size of nearly 79 thousand samples.

(insert arch-ds table here)

Following the initial bias probe performed by the authors of CLIP \cite{clip}, we chose the FairFace dataset \cite{fair_face} for our experiments. This dataset contains 108,501 images of faces which is balanced on 7 race groups: White, Black, Indian, East Asian, Southeast Asian, Middle Eastern and Latino. Image data was collected from the YFCC-100M Flickr dataset \cite{yfcc} and labeled with race, gender and age groups.

For the class-generation, we applied the approach from Section 7.1 Bias of the OpenAI CLIP paper, forming combinations of the binary gender and the 7 race groups present in the target FairFace dataset. Next, we created the final text prompts using the "A photo of a/an \<class>" template as advocated in the "Interacting with CLIP" Jupyter notebook shared at \cite{open_clip_repo} in the context of Zero-Shot Image Classification for CIFAR-100 dataset.

(insert prompt list here?)


### Architectures

- ViT-B-32
- ViT-B-16
- ViT-L-14
- ViT-H-14
- ViT-G-14

### DataSources

- openAI
- LAION-400M
- LAION-2B
- DataComp-M
- DataComp-L
- DataComp-XL
- CommonPool-M
- CommonPool-L
- CommonPool-XL

### About CLIP's WIT dataset

The original openAI CLIP paper briefly describes their data scraping process for their private WebImageText (WIT) dataset. This is a 400 million (image, text) pairs collected from a variety of publicly available sources on the Internet. Further exploring these raw sources they also built a text corpora of 500.000 queries based on the top 100 most occurred words from Wikipedia, enlarging it with bi-grams and WordNet synsets. The robust volume of data was later on filtered for class balance, including at most 20.000 (image, text) pairs for query.

### About LAION's 400M and 2B datasets

LAION-400M \cite{laion400} is an open-source dataset with over 400 million (text, images) pairs scraped from the Common Crawl \cite{common_crawl} web dump, filtered using CLIP models for image text similarity match. Based on this initial effort, a larger version of the dataset was constructed, consisting of 5.85 billion CLIP-filtered image-text pairs, of which 2.32B contain English language. Both 400M and the 2B subset are employed at openCLIP and used in this work.

### About Datacomp's and CommonPool's datasets

Datacomp is a testbed for dataset experiments centered around a new candidate pool of 12.8 billion image-text pairs from Common Crawl \cite{common_crawl}, which they called CommonPool. One of the benchmark tracks is the filtering track, where each participant must find the best methods to filter the CommonPool dataset in order to achieve top performance on a variety of zero-shot tasks. They showed improvements over these tasks when different filtering techniques were applied to data, such as querying captions for relevant keywords, filtering based on image embeddings and applying a threshold on CLIP scores. Based on the results of the filtered datasets, they constructed the DataComp-1B dataset, a filtered version of CommonPool based on a combination of the two most promising filtering baselines.

## 2. Prompt ensemble

In order to improve CLIP gender classification in our target dataset, we propose an ensemble of age, race and gender adjectives in a single prompt, later grouping it by the original gender labels. This process involves saving both image and text embeddings, later applying the softmax as classification and selecting the most prominent result. The steps required for this outcome are as follows: 1) Generate a list of prompts, 2) save image and prompts embeddings, 3) evaluate similarities between images and prompts, 4) predict final gender label and 5) measure results.

By combining a list of age, race and gender words, the list of prompts was generated from all their possible combinations. The prompt was built based on aforementioned prefix "a photo of a/an" and a combination of (race + gender), (age + gender) and (age + race + gender). We also added prompts with only the gender classes for the leveraging of the original prompt.

Based on the prompts created above, and a list of image file paths, the embeddings for the CLIP model were generated and saved.

Images were loaded using python Pillow library, later sent to target device (cuda if available) and processed by calling the corresponding CLIP model encode image function, while inside a loop statement with no gradient optimization. The resulting embedding was passed along with its corresponding filename to a pandas DataFrame structure, later saved to a pickle file to preserve the embeddings in numpy arrays.

For the text prompts, a JSON file was read and later tokenized. These tokenized inputs were encoded using the corresponding CLIP model encode text function and normalized while keeping its dimensions (the same process was also applied to the images).

With images and text embeddings saved to disc, we were able to compare similarities between every image to all available prompts by calculating 100.0 * image\_features @ text\_features.T, where @ is matrix multiplication and .T is the transposed form of the vector. The result of this operation is a list with a similarity score between the target image and each of the available text prompts.

The prediction for each image is saved as a new column in the original dataset file. This allows us to compare between both columns in order to measure the accuracy of the predictions, while also extracting some insights about the classification performance.

General accuracy is measured comparing the ground truth "gender" column with the predictions. Based on this general approach, we are able to measure the performance over more specific samples of the data, such as accuracy by gender, age and race.

There are 3 available sets of labels that we can choose from. The first and simplest one is the raw\_gender\_labels, with only Male and Female prompts, the second one is the original\_clip\_labels where we try to replicate the labels used by the authors of the original CLIP paper, and finally there is the set presented here, called age\_race\_gender.
## 3. Experiments

- The effect of model and data scaling
	- Initial example comparing original_clip_labels with different models and datasets, check how classification errors are divided into races.
- The effect of auxiliary adjectives
	- Explain our prompt ensemble strategy and the impact on classification. The motive is for the better alignment of image-text embedding.
- **BONUS**: The effect of synonyms
	- Check description at overleaf project

### The effects of model and data scaling

#### Model Scaling

Grab the same data-source and all model checkpoints.

For model scaling we chose a constant data-source and scaled the ViT model's architecture. The only available data-source for all model checkpoints was LAION-2B, with its individual variant, as seen in table X.

table X: model-scaling-details

### Data scaling

Grab the same model and all datasources.

For data scaling we chose a constant model architecture and scaled the data-sources. Beyond just raw dataset size, we also included filtered and curated datasets to explore the impact of their filtering techniques. These filtered datasets include the subsets of the DataComp-1B dataset, being a CommonPool dataset filtered by their two most promising approaches.

(Here we could also grab all filtering techniques of datacomp and explore their impact for gender classification, but maybe too out of scope)


## Aggregation techniques

To further explore aggregation techniques, the list of similarities is evaluated based on two functions, an average sum function and a softmax top[k] function. The average function sums up all the Male prompts similarities vs all the Female prompts similarities in order to choose the bigger. The top[k] selects the k-th highest scores of each class and sum them up into the final score. The output of both functions is considered the final classification label for the current image.

The original OpenAI CLIP paper does not says anything about their prediction method, and as such we assume that they employ a simple argmax to retrieve the most similar prompt from the list. This is equal to our method of Top K with k = 1, and so this is the used method when we try to replicate the original paper results. Beyond that, we also experimented with different k values, where the logic is that it sorts the similarity scores from highest to lowest, and sum up the first k samples, each for every category (male and female).

Both model and data scaling experiments will be replicated with the addition of the aggregation techniques, selecting the best method for each model and data-source, where it can be any from the average aggregator to the top-k aggregator, with 20 levels of K being explored.