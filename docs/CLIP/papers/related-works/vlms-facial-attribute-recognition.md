# Exploring Vision Language Models for Facial Attribute Recognition: Emotion, Race, Gender and Age

ref 1, applications, ref 2, difficulties, ref 3 ML shortcommings, ref 4 age and emmotion intraclass variability, ref 8, 9 ethical AI, 10, 11 biased datasets, refs 37 - 42 race classification applications 

The goal of this work is to develop a multi-task classifier that leverages the capabilities of VLMs to simultaneously identify multiple human facial attributes.

**Keywords**: Facial attribute recognition methods

**Models explored**: GPT-4o, GEMINI 1.5, LLAVA-NEXT, PaliGemma and Florence-2

**Datasets used**: FairFace, AffectNet and UTKFace

> The direct applications of vision language models (VLMs) in facial attribute recognition are still unexplored.

They formulated the facial recognition application as a visual question-answer task

![[Pasted image 20250203161356.png]]

They combined East and South Asian into "Asian" group.

GPT-4o gave highest metrics with 76.4% accuracy. With South and East Asian, it dropped to 68%.

![[Pasted image 20250203155714.png]]

For gender, GPT-40 achieved 95.9% compared to CLIP's 94%.

For age, they combined age groups in 5 categories: 0-9, 10-19, 20-39, 40-59, 60+. GPT-4o achieved highest accuracy 77.4%.

For emotion, all models performed poorly, with high 49% accuracy and low 38.8%.

