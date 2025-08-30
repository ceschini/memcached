# Leveraging Visual Embeddings from Instagram for Credit Scoring of Informal Microbusinesses in Latin America

## Reading notes

Access to formal credit for informal microbusinesses in Latin America is limited, and several fintech startups have sought to address this challenge by developing alternative credit scoring methodologies leveraging AI and non-traditional data sources.

The paper proposes to leverage Instagram images and videos of microbusinesses accounts to extract visual features capable of improving predictive models of creditworthiness.

The proposed method starts with CLIP to obtain visual embeddings. Then, they reduce dimensionality and cluster it to derive **discriminative features**. They also introduce two distinct architectures: A Fully Connected Neural Network (FCNN) processing CLIP embeddings, and a separate CNN directly analyzing image data, each generating predictive visual scores.

The dataset used in the study comprises Instagram account data from 570 microbusinesses who received microcredit financing from a fintech company operating in Colombia. Users were categorized as either good or poor payers based on their loan repayment behavior, specifically, whether they experienced overdue payments (default) or consistently met repayment deadlines.

Multiple data modalities were extracted from each user's Instagram account, including:

**User metadata**, such as biography text, number of followers, total number of posts, etc. **Post information**, such as number of highlights, post type (reel, carousel, or single image), geolocation tags, date of posting, number of likes, comments, views, tagged collaborators, and textual descriptions. **Visual data**, images and videos from user posts, analyzed through computer vision methodologies described in the paper.

The study evaluates whether visual features can improve credit scoring with XGBoost. They test the impact of adding visual embeddings in the structured data baseline model.

They also implemented a custom loss function that balances multiple objectives critical for credit risk assessment.

(I'm worried about a simplistic view about this. By simply mimicing the behavior and social media footprint of good payers one could get an impactful boost in credit risk scoring, but without any real relationship between good payers)

why reduce dimensionality? why clusterization? why CLIP in the first place? Couldn´t you use ViT directly? If so, why not leverage textual encoding of CLIP?

visual examples from dataset?

repeated paragraphs, lines 152 and 156.

did all clusters related to a specific business type as the two in the example?

details about the analysis of feature importance?
## Summary and contribution

Access to formal credit for informal microbusinesses in Latin America is limited, and several fintech startups have sought to address this challenge by developing alternative credit scoring methodologies leveraging AI and non-traditional data sources. The paper proposes to leverage Instagram images and videos of microbusiness accounts to extract visual features capable of improving predictive models of creditworthiness. The study evaluates whether visual features can improve credit scoring with XGBoost, testing the impact of adding visual embeddings in the structured data baseline model. The proposed method employs a CLIP model to obtain visual embeddings, reducing dimensionality and clustering it to derive discriminative features. These features are passed to a Fully Connected Neural Network (FCNN) that will generate a predictive visual score. The dataset used in the study comprises Instagram account data from 570 microbusinesses who received microcredit financing from a fintech company operating in Colombia. Users were categorized as good or poor payers based on their loan repayment behavior and whether they experienced overdue payments (default) or consistently met repayment deadlines. Multiple data modalities were extracted from each user's Instagram account, including user metadata, post information, and visual data. Experiments showed that visual embeddings improved AUC by 2.16 and F1-score by 9.86 percentage points over metadata-only models.
## Strengths

The problem addressed is socially relevant. The proposed method is based on emerging technologies such as CLIP, and it contains a solid baseline. The proposed method performed better than the standard metadata approach, effectively answering its intrinsic research question.

## Weaknesses

The proposed method lacked explanations about its architectural decisions. In line 44, the paper states that CLIP models can capture signals related to business operations, product quality, and customer interaction, but it doesn't provide any reference. If using only the vision module of CLIP, why not use a Vision Transformer? Also, the reason for dimensionality reduction and clustering is not in the paper. There is a lack of qualitative analysis of the results, with no image samples from the dataset to help the reader understand the visual characteristics relevant to credit scores. Finally, in the conclusion section, it is stated that "Feature importance analysis revealed that visual features contributed 25.52% of the model's predictive power, but no detail about such analysis is in the paper.

## Justifying the rating

The paper is promising, and the problem is relevant. However, the proposed methodology is not sufficiently detailed and fails to justify the reasoning behind its architecture. It is also unclear how effective the resulting model is in predictive creditworthiness since there is no qualitative analysis of its predictions.

## Comments to authors

Include a list of contributions in the introduction, mentioning the custom loss function, and remove duplicate paragraphs in lines 152 and 156. Describe why dimensionality reduction and clustering are relevant for the pipeline, and explore some prediction results, preferably with visual examples of hits and misses.
