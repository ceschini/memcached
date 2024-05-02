# CLIP FairFace Classification

This note aims to understand the experiment made by the openAI team in the CLIP paper. From the information gathered, the authors seems to have measured both gender, race and age classification accuracy and used a label set consisting of the intersection between genders and races (at least in one experiment, see Table 6 note below).

## What does the CLIP paper says

> We evaluated two versions of CLIP on the FairFace dataset: a zero-shot CLIP model ("ZS CLIP"), and a logistic regression classifier fitted to FF's dataset on top of CLIP's features ("LR CLIP").

No backbone specified.

> We test the performance of CLIP models across *intersectional* race and gender categories **as they are defined in the FairFace dataset**

**Table 6**: \[...] ***The label set included 7 FairFace race categories each for men and women (for a total of 14)***, \[...]

**Table 3**: Percent accuracy on Race, Gender and Age classification of images in FF category 'White'.
**Table 4**: Percent accuracy on Race, Gender and Age classification of images in FF category 'Non-White' (all other races).

**Table 5**: Gender classification performance for race categories (similar to our method).

## What does the community says

Based on [this github open issue](https://github.com/openai/CLIP/issues/157) and it's author [colab notebook](https://colab.research.google.com/drive/1OPFMZ2HQnqvUa8xV5JcSdEpI8Esu_e23?usp=sharing).

> Just like in the paper, we merge the FairFace labels for race and gender. For each combined label we define a prompt to use in the zero-shot classification. The exact prompts used for the CLIP paper experiments are unknown, so we choose them as simply as possible.

```python
labels = {

'White_Male': 'a photo of a white man',
'White_Female': 'a photo of a white woman',
'Black_Male': 'a photo of a black man',
'Black_Female': 'a photo of a black woman',
'Latino_Hispanic_Male': 'a photo of a latino man',
'Latino_Hispanic_Female': 'a photo of a latino woman',
'East Asian_Male': 'a photo of an east asian man',
'East Asian_Female': 'a photo of an east asian woman',
'Southeast Asian_Male': 'a photo of a southeast asian man',
'Southeast Asian_Female': 'a photo of a southeast asian woman',
'Indian_Male': 'a photo of an indian man',
'Indian_Female': 'a photo of an indian woman',
'Middle Eastern_Male': 'a photo of a middle eastern man',
'Middle Eastern_Female': 'a photo of a middle eastern woman',

'...crime related labels...': '...',
```

## What does the FairFace paper says

> Tables 3 and 3 show the classification results for race, gender and age on the datasets across **subpopulations**.

They predict and measure individual race, gender and age classifications.

> We measure model consistency by standard deviations of classification accuracy measured on different sub-populations, as shown in Table 5.
> More formally, one can consider conditional use accuracy equality \[6] or equalized odds \[16] as **the measure of fair classification**.

**Table 4**: Gender classification accuracy measured on external datasets across **gender-race** groups.

### Based on FairFace `predict.py` code

Source [here](https://github.com/dchen236/FairFace/tree/master).

```python
def predict_age_gender_race(...):
	
	image = load_img()
	outputs = model_fair_7(image)
	
	race_outputs = outputs[:7]
	gender_outputs = outputs[7:9]
	age_outputs = outputs[9:18]

	race_score = np.exp(race_outputs) / np.sum(np.exp(race_outputs))
	gender_score = np.exp(gender_outputs) / np.sum(np.exp(gender_outputs))
	age_score = np.exp(age_outputs) / np.sum(np.exp(age_outputs))

	race_pred = np.argmax(race_score)  # Top k=1
	gender_pred = np.argmax(gender_score)
	age_pred = np.argmax(age_score)
```

They trained a model with all FairFace's categories of races, and another model with only 4 categories (male, female and white, non-white) for compatibility with the baseline datasets.

## Reported metrics

The original CLIP paper report gender classification accuracy by race category at *Table 5*.  To better visualize some races have been abbreviated:

- ME: Middle-Eastern
- SA: South-Asian
- EA: East-Asian

**Original CLIP Paper**

| Male | Female | Black | White | Indian | Latino | ME   | SA   | EA   | Average |
| ---- | ------ | ----- | ----- | ------ | ------ | ---- | ---- | ---- | ------- |
| 96.9 | 97.0   | 96.7  | 95.9  | 98.0   | 97.5   | 98.0 | 96.3 | 96.6 | 97.0    |

This should correspond to our Top K = 1 method, and below is the ViT-L-14 results for the CLIP model and openCLIP model.

**CLIP model ViT-L-14**

| Male | Female | Black | White | Indian | Latino | ME   | SA   | EA   | Average |
| ---- | ------ | ----- | ----- | ------ | ------ | ---- | ---- | ---- | ------- |
| 95.2 | 95.0   | 93.2  | 94.4  | 96.9   | 95.0   | 97.1 | 95.3 | 94.8 | 95.1    |

**openCLIP model ViT-L-14**

| Male | Female | Black | White | Indian | Latino | ME   | SA   | EA   | Average |
| ---- | ------ | ----- | ----- | ------ | ------ | ---- | ---- | ---- | ------- |
| 95.2 | 95     | 93.1  | 94.4  | 96.9   | 94.9   | 97.1 | 95.3 | 94.9 | 95.1    |
