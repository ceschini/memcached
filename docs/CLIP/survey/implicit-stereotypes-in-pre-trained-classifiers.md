# Implicit Stereotypes in Pre-Trained Classifiers

**Keywords**: Algorithmic fairness, facial recognition, deep learning, zero-shot classification, CLIP.

The authors composed a **public** dataset of 10k images and six pairs of labels to classify **sensitive information** or **protected attributes** such as gender, race, attractiveness, friendliness, wealth and intelligence. They found spurious correlations between these labels with high confidence and proposed how to mitigate it at the deployment level.

The idea of fairness here is based on two statistical definitions, *anticlassification* and *statistical parity*.

Anticlassification or unawareness requires that decisions be independent of protected attributes. Statistical parity requires that positive decisions be equally likely for all groups defined by protected attributes.

A commonly noted limitation of both concepts of fairness is that correlated unprotected attributes (education level, salary, life-expectancy, etc) might act as proxies for protected attributes, effectively resulting in indirect discrimination.

The authors use GAN generated images of faces instead of real photographs to prevent any meaningful hidden information of the data. This is relevant specially since CelebA or even FairFace have faces of public figures that could have some form of underlying data about it, that could shake the decision making process.

The **goal** of the study is the association of two protected attributes, gender and ethnicity, with conventionally desirable attributes such as attractiveness, friendliness, wealth and intelligence. The deffinition of each attribute is made by a pair of opposing labels, representing a spectrum of belonging probabilities for each image.

- Male vs Female
- White Person vs Person of Color
- Attractive vs Unattractive
- Friendly vs Unfriendly
- Rich vs Poor
- Intelligent vs Unintelligent

They argue that Attractiveness and Friendliness can be decently inferred from photographs, whilst Wealth and intelligence can't, and would thus constitute an interesting way to explore implicit stereotypes in classifiers through their association with "visible" attributes.

They say that even though there is the FairFace dataset, the multiple ethnicities would reproduce wealth and other bias that are present in society.

The results show that there is no correlation between ethnicity and intelligence, a pleasant surprise given the long history of scientific racism that CLIP could have learned.

However, the results suggest that about 35% of attractiveness, according to CLIP, can be explained by the extent to which a person is white and female.

***Their debiasing method was applied only to gender bias, but could easily be extended to race.***

> Debiasing first requires geometrically identifying a gender subspace in text and image embeddings and quantifying its contribution to similarity scores. This gender subspace will later serve as a basis to the neutralization of bias.

They mention synonyms of the labels as future work.

They argue that their work has shown that associations correspond neither to bias in data nor algorithmic bias, but I couldn't understand why not.

They also say that "If all implicit stereotypes are wrong, some of them are useful to model, so that they can precisely be corrected in real life." As an example, they propose that a classification system intended at identifying persons who are the most likely to benefit from financial aid to reproduce these race - wealth associations.

> Fairness is relative and depends on the intended application, and the responsibility of testing and ensuring fairness should thus be put at the point of deployment.

They point out the lack of representation for some ethnic groups in GAN datasets.