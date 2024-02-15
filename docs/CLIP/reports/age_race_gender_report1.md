# Age, Race and Gender Prompt Ensemble
## Report 1

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

Images were loaded using python Pillow library, later sent to target device (cuda if available) and processed by calling the `clip_model.encode_image()` function, while inside a `with torch.nograd()` statement. The resulting embedding was passed along with its corresponding filename to a pandas DataFrame structure, later saved to a pickle file to preserve the embeddings in numpy arrays.

For the text prompts, a JSON file was read and later tokenized by `torch.cat()` and `clip.tokenize()` functions. The tokenized inputs were encoded using `clip_model.encode_text()` and normalized while keeping its dimensions, the same process was also applied to the images.

### 3. Evaluate similarities between prompts

With images and text embeddings saved to disc, we were able to compare similarities between every image to all available prompts by calculating `100.0 * image_features @ text_features.T`, where `@` is matrix multiplication and `.T` is the transposed form of the vector. The result of this operation is a list with a similarity score between the target image and each of the available text prompts.

### 4. Predict the final gender label

The list of similarities is then evaluated based on two functions, an average sum function and a sotmax top\[k] function. The average function sums up all the Male prompts similarities vs all the Female prompts similarities in order to choose the bigger. The top\[k] function simply grabs the maximal value between all prompts. The output of both functions is considered the final classification label for the current image.

### 5.  Measure results

The prediction for each image is saved as a new column in the original dataset csv file. This allows us to compare between both columns in order to measure the accuracy of the predictions, while also extracting some insights about the classification performance.

General accuracy is measured comparing the ground truth "gender" column with the predictions over at "gender_preds" column. Based on this general approach, we are able to measure the performance over more specific samples of the data, such as accuracy by gender, age and race.