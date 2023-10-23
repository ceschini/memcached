# Adding datasets from other domains

We are adding different datasets from other domains in order to improve training while reducing over fitting. Also, we want to add visual-text task specific datasets for improving restoration of frames with textual information.

Here are an up-to-date list of datasets being explored, followed by a brief info about them.

## Dataset list
- ICDAR 2019 - ST-VQA
- TextCaps
- IIIT scene text retrieval (STR)

## About datasets

In the early look at the dataset's papers we can suppose that we will not be able to use every image on them, but in fact we'll have to filter them in some way to not completely disrupt training.

### ICDAR 2019

This is a dataset for Scene Text Visual Question Answering (ST-VQA) competition. There are over 23k samples, but not all of them will be useful to us, because they are cramped or distorted, which do not correspond to our domain.

### TextCaps

This is a dataset for image captioning with reading comprehension. They argue that beyond OCR and object recognition, in order to truly understand the context of an image, it is required to comprehend texts in order to support image captioning context formation. Because of that, there are images that do not contain explicit words on in-the-wild images, but also numbers in a ruler for example, or the current time on a smartphone screen.

### IIIT scene text retrieval

This is a dataset containing labeled text classes on images, such as "Motel", "Police", "Restaurant". It is queried from Google image search and passed through an image text recognizer and manual labeling. It contains 10K images with 10–50 occurrences of each query word. The images are in a more standard and frontal view and could be useful to us.

## Tabular info

We should think about organizing info about the datasets in a tabular form, columns should include something like: Name, Size, task, resolution. And some qualitative columns as well.

## Initial image set exploration

Because of image text recognition, OCR, visual text comprehension and other related tasks, the authors do not worry about image resolution, but instead look for a large enough corpus to enable deep learning training. Because of that, we should filter images by resolution and look into more specific image datasets tasks, that provide **high resolution images**.

## OCR on super res datasets

Maybe we could download a lot of super resolution datasets and run OCR models in order to grab only images containing texts. This way we can make sure that all the images are of a high resolution while still containing text.

Jhjhjhj