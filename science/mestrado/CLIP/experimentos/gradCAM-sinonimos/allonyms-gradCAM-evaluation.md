# Evaluation script for the allonyms-gradCAM tests

**What will we evaluate**

The activation map of each synonym, it’s similarity and gradCAM scores.

**What will we run**

The standard clip script to get **scores** and **output labels**, the gradCAM script and the activation map image generator.

**How will we run**

We must first grab the required scripts and merge into a single app. Then we create a main script that reads an allonyms JSON file and run it grabbing each word at a time.

**How will we register results**

We will use pandas to register the word, scores, gradCAM, output label and all textual metrics. We will also save the output image of the heatmap with its corresponding B&W version.