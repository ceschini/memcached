# Bias Survey

## Notes

So far, the papers tend to focus on debiasing using NLP techniques. They leverage the fact that contrastive learning joins text and image embeddings on the same shared feature space, and as such they can use NLP methods to debias and measure.

| Paper                   | Type              | Datasets           | Methods                                          |
| ----------------------- | ----------------- | ------------------ | ------------------------------------------------ |
| CLIP                    | race, gender      | FairFace, congress | gender shades & fair face                        |
| Oxford                  | criminal          | FairFace, COCO     | evaluating clip                                  |
| prompt array, bias away | race, gender      | FairFace, UTK-Face | learn unbiased embeddings using adversarial nets |
| biased prompts          | race, gender, age | FairFace           | MaxSkew metric                                                 |


## Key papers

**Gender Shades**: Intersectional accuracy disparities in commercial gender classification.
**FairFace**: Face attribute dataset for balanced race, gender, and age
**Evaluating clip**: towards characterization of broader capabilities and downstream implications.
**MaxSkew metric**: Fairness-aware ranking in search & recommendation systems with application to LinkedIn talent search.

