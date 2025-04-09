# Results

## Data scaling

datacomp and commonpool had really poor results model wise, this could be due to the fact that these datasets were tailored for specific downstream tasks, and didn´t generalized well for off scope scenarios, such as race prediction.

General accuracy was not consistent with scaling of dataset sizes. Even though the best model were the biggest one, the second and third best were the openai and laion 400m respectively, one of the smallest ones. This could mean that, at least for vit-b-16, dataset size is not the only thing important here, but actually the kind of data being trained with.

datacomp models had poor results in all ethnicities except Black, *but i'm not sure why*.

**DataComp tem face blurring, então n faz sentido (appendix G)**

## Model scaling

ViT-B-16 even though is smallest model had the best results in Latino_Hispanic predictions, 62% vs 47% of ViT-L-14. On the other hand, White predictions were from 57% accuracy with B-16 to 78% with H-14, a massive improvement. Same thing with Middle Eastern, from 46% to 61%.