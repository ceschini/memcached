# Age, Race and Gender Prompt Ensemble

## General Process

In order to improve CLIP classification of genders "Male" or "Female" in the FairFace dataset, we are trying to combine age, race and gender in a single prompt, later grouping it by the original gender labels. This process involves saving both image and text embeddings, later applying the softmax as classification and selecting the most prominent result. The steps required for this outcome are as follows:

1. Generate a list of prompts
2. Save image and prompts embeddings
3. Evaluate similarities between images and prompts
4. Predict final gender label
5. Measure results

### 1. Generate a list of prompts

By combining a list of age, race and gender words, we generated a JSON file with all the possible combinations of these words. The list can be seen below, and the prompt was built based on the prefix *"a photo of a"* and a combination of (race + gender), (age + gender) and (age + race + gender). We also added only gender for the original prompt.

```
age_list = ['young', 'old', 'middle-aged']

race_list = ['black', 'white', 'hispanic', 'latino', 'indian', 'asian', 'arabic']

gender_list = ['man', 'woman']
```

Examples of the final prompts are:

```
{
	'man': 'a photo of a man', 
	'black_man': 'a photo of a black man',
	'young_black_man': 'a photo of a young black man',
}
```

### 2. Save image and prompts embeddings

Based on the prompts created above, and a list of image filepaths, the embeddings for the CLIP model were generated and saved. 

Images were loaded using python Pillow library, later sent to target device (cuda if available) and processed by calling the `clip_model.encode_image()` function, while inside a `with torch.nograd()` statement. The resulting embedding was passed along with its corresponding filename to a pandas DataFrame structure, later saved to a *pickle* file to preserve the embeddings in numpy arrays.

For the text prompts, a JSON file was read and later tokenized by `torch.cat()` and `clip.tokenize()` functions. The tokenized inputs were encoded using `clip_model.encode_text()` and normalized while keeping its dimensions (the same process was also applied to the images).

### 3. Evaluate similarities between prompts

With images and text embeddings saved to disc, we were able to compare similarities between every image to all available prompts by calculating `100.0 * image_features @ text_features.T`, where `@` is matrix multiplication and `.T` is the transposed form of the vector. The result of this operation is a list with a similarity score between the target image and each of the available text prompts.

### 4. Predict the final gender label

The list of similarities is then evaluated based on two functions, an average sum function and a sotmax top\[k] function. The average function sums up all the Male prompts similarities vs all the Female prompts similarities in order to choose the bigger. The top\[k] selects the k-th highest scores of each class and sum them up into the final score. The output of both functions is considered the final classification label for the current image.

### 5.  Measure results

The prediction for each image is saved as a new column in the original dataset csv file. This allows us to compare between both columns in order to measure the accuracy of the predictions, while also extracting some insights about the classification performance.

General accuracy is measured comparing the ground truth "gender" column with the predictions over at "gender_preds" column. Based on this general approach, we are able to measure the performance over more specific samples of the data, such as accuracy by gender, age and race.

## About the available labels

There are 3 available sets of labels that we can choose from. The first and simplest one is the `raw_gender_labels`, with only Male and Female prompts, the second one is the `original_clip_labels` where we try to replicate the labels supposedly used by the authors, and finally there is the set presented here, called `age_race_gender_labels`.


## About Top K prediction

The original paper does not says anything about the prediction method, and as such we assume that they employ a simple `argmax` to retrieve the most similar prompt from the list. This is equal to our method of Top K with `k = 1`, and as such this is the used method when we try to replicate the original paper results. Beyond that, we also experimented with different `k` values, where the logic is that it sorts the similarity scores from highest to lowest, and sum up the first `k` samples, each for every category (male and female).

## About Backbones and DataSources

We are currently focusing on **ViT-L-14** as our main backbone, due to the fact that it is the largest one present in the original CLIP pre-trained models, and also available at the openCLIP models. As for datasources, we cherry-picked a list that could encompass the largest option of each data source available to the current ViT-L-14 backbone, and they are:

- openai
- laion400m_e31
- laion2b_s32b_b82k
- datacomp_xl_s13b_b90k
- commonpool_xl_s13b_b90k

By calling the corresponding `.list_pretrained` function we can check all the available backbones and datasources for the CLIP and openCLIP models.

**CLIP pre-trained**:

```json
[
	"RN50",
	"RN101",
	"RN50x4",
	"RN50x16",
	"RN50x64",
	"ViT-B/32",
	"ViT-B/16",
	"ViT-L/14",
	"ViT-L/14@336px"
]
```

**openCLIP pre-trained**:

`["backbone", "datasource"]`

```json
[
	["RN50", "openai"],
	["RN50", "yfcc15m"],
	["RN50", "cc12m"],
	["RN50-quickgelu", "openai"],
	["RN50-quickgelu", "yfcc15m"],
	["RN50-quickgelu", "cc12m"],
	["RN101", "openai"],
	["RN101", "yfcc15m"],
	["RN101-quickgelu", "openai"],
	["RN101-quickgelu", "yfcc15m"],
	["RN50x4", "openai"],
	["RN50x16", "openai"],
	["RN50x64", "openai"],
	["ViT-B-32", "openai"],
	["ViT-B-32", "laion400m_e31"],
	["ViT-B-32", "laion400m_e32"],
	["ViT-B-32", "laion2b_e16"],
	["ViT-B-32", "laion2b_s34b_b79k"],
	["ViT-B-32", "datacomp_xl_s13b_b90k"],
	["ViT-B-32", "datacomp_m_s128m_b4k"],
	["ViT-B-32", "commonpool_m_clip_s128m_b4k"],
	["ViT-B-32", "commonpool_m_laion_s128m_b4k"],
	["ViT-B-32", "commonpool_m_image_s128m_b4k"],
	["ViT-B-32", "commonpool_m_text_s128m_b4k"],
	["ViT-B-32", "commonpool_m_basic_s128m_b4k"],
	["ViT-B-32", "commonpool_m_s128m_b4k"],
	["ViT-B-32", "datacomp_s_s13m_b4k"],
	["ViT-B-32", "commonpool_s_clip_s13m_b4k"],
	["ViT-B-32", "commonpool_s_laion_s13m_b4k"],
	["ViT-B-32", "commonpool_s_image_s13m_b4k"],
	["ViT-B-32", "commonpool_s_text_s13m_b4k"],
	["ViT-B-32", "commonpool_s_basic_s13m_b4k"],
	["ViT-B-32", "commonpool_s_s13m_b4k"],
	["ViT-B-32-256", "datacomp_s34b_b86k"],
	["ViT-B-32-quickgelu", "openai"],
	["ViT-B-32-quickgelu", "laion400m_e31"],
	["ViT-B-32-quickgelu", "laion400m_e32"],
	["ViT-B-32-quickgelu", "metaclip_400m"],
	["ViT-B-32-quickgelu", "metaclip_fullcc"],
	["ViT-B-16", "openai"],
	["ViT-B-16", "laion400m_e31"],
	["ViT-B-16", "laion400m_e32"],
	["ViT-B-16", "laion2b_s34b_b88k"],
	["ViT-B-16", "datacomp_xl_s13b_b90k"],
	["ViT-B-16", "datacomp_l_s1b_b8k"],
	["ViT-B-16", "commonpool_l_clip_s1b_b8k"],
	["ViT-B-16", "commonpool_l_laion_s1b_b8k"],
	["ViT-B-16", "commonpool_l_image_s1b_b8k"],
	["ViT-B-16", "commonpool_l_text_s1b_b8k"],
	["ViT-B-16", "commonpool_l_basic_s1b_b8k"],
	["ViT-B-16", "commonpool_l_s1b_b8k"],
	["ViT-B-16", "dfn2b"],
	["ViT-B-16-quickgelu", "metaclip_400m"],
	["ViT-B-16-quickgelu", "metaclip_fullcc"],
	["ViT-B-16-plus-240", "laion400m_e31"],
	["ViT-B-16-plus-240", "laion400m_e32"],
	["ViT-L-14", "openai"],
	["ViT-L-14", "laion400m_e31"],
	["ViT-L-14", "laion400m_e32"],
	["ViT-L-14", "laion2b_s32b_b82k"],
	["ViT-L-14", "datacomp_xl_s13b_b90k"],
	["ViT-L-14", "commonpool_xl_clip_s13b_b90k"],
	["ViT-L-14", "commonpool_xl_laion_s13b_b90k"],
	["ViT-L-14", "commonpool_xl_s13b_b90k"],
	["ViT-L-14-quickgelu", "metaclip_400m"],
	["ViT-L-14-quickgelu", "metaclip_fullcc"],
	["ViT-L-14-quickgelu", "dfn2b"],
	["ViT-L-14-336", "openai"],
	["ViT-H-14", "laion2b_s32b_b79k"],
	["ViT-H-14-quickgelu", "metaclip_fullcc"],
	["ViT-H-14-quickgelu", "dfn5b"],
	["ViT-H-14-378-quickgelu", "dfn5b"],
	["ViT-g-14", "laion2b_s12b_b42k"],
	["ViT-g-14", "laion2b_s34b_b88k"],
	["ViT-bigG-14", "laion2b_s39b_b160k"],
	["roberta-ViT-B-32", "laion2b_s12b_b32k"],
	["xlm-roberta-base-ViT-B-32", "laion5b_s13b_b90k"],
	["xlm-roberta-large-ViT-H-14", "frozen_laion5b_s13b_b90k"],
	["convnext_base", "laion400m_s13b_b51k"],
	["convnext_base_w", "laion2b_s13b_b82k"],
	["convnext_base_w", "laion2b_s13b_b82k_augreg"],
	["convnext_base_w", "laion_aesthetic_s13b_b82k"],
	["convnext_base_w_320", "laion_aesthetic_s13b_b82k"],
	["convnext_base_w_320", "laion_aesthetic_s13b_b82k_augreg"],
	["convnext_large_d", "laion2b_s26b_b102k_augreg"],
	["convnext_large_d_320", "laion2b_s29b_b131k_ft"],
	["convnext_large_d_320", "laion2b_s29b_b131k_ft_soup"],
	["convnext_xxlarge", "laion2b_s34b_b82k_augreg"],
	["convnext_xxlarge", "laion2b_s34b_b82k_augreg_rewind"],
	["convnext_xxlarge", "laion2b_s34b_b82k_augreg_soup"],
	["coca_ViT-B-32", "laion2b_s13b_b90k"],
	["coca_ViT-B-32", "mscoco_finetuned_laion2b_s13b_b90k"],
	["coca_ViT-L-14", "laion2b_s13b_b90k"],
	["coca_ViT-L-14", "mscoco_finetuned_laion2b_s13b_b90k"],
	["EVA01-g-14", "laion400m_s11b_b41k"],
	["EVA01-g-14-plus", "merged2b_s11b_b114k"],
	["EVA02-B-16", "merged2b_s8b_b131k"],
	["EVA02-L-14", "merged2b_s4b_b131k"],
	["EVA02-L-14-336", "merged2b_s6b_b61k"],
	["EVA02-E-14", "laion2b_s4b_b115k"],
	["EVA02-E-14-plus", "laion2b_s9b_b144k"],
	["ViT-B-16-SigLIP", "webli"],
	["ViT-B-16-SigLIP-256", "webli"],
	["ViT-B-16-SigLIP-i18n-256", "webli"],
	["ViT-B-16-SigLIP-384", "webli"],
	["ViT-B-16-SigLIP-512", "webli"],
	["ViT-L-16-SigLIP-256", "webli"],
	["ViT-L-16-SigLIP-384", "webli"],
	["ViT-SO400M-14-SigLIP", "webli"],
	["ViT-SO400M-14-SigLIP-384", "webli"],
	["ViT-L-14-CLIPA", "datacomp1b"],
	["ViT-L-14-CLIPA-336", "datacomp1b"],
	["ViT-H-14-CLIPA", "datacomp1b"],
	["ViT-H-14-CLIPA-336", "laion2b"],
	["ViT-H-14-CLIPA-336", "datacomp1b"],
	["ViT-bigG-14-CLIPA", "datacomp1b"],
	["ViT-bigG-14-CLIPA-336", "datacomp1b"],
	["nllb-clip-base", "v1"],
	["nllb-clip-large", "v1"],
	["nllb-clip-base-siglip", "v1"],
	["nllb-clip-large-siglip", "v1"]
]
```

## About balanced accuracy

There is no discrepancy between genders, we should check other metrics but they will make more sense when we predict races and ages.

### Gender distribution

- Male:      5792
- Female:    5162

#### Example

**backbone**: laion2b_s32b_b82k 

**best method**: Top 11

**accuracy**: 0.9682

**balanced_accuracy**: 0.9688
