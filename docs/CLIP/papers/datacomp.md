# DataComp: In search of the next generation of multimodal datasets

Datacomp is a testbed for dataset experiments centered around a new candidate pool of 12.8 billion image-text pairs from **Common Crawl**. They state that their best baseline outperformed CLIP's using the same training procedure and compute.

They criticize how LAION-2B is unclear on how design choices such as the data source or filtering techniques affect the resulting models. And how there are thousands of ablation studies for algorithmic choices such as loss function and model architecture, but there are none about dataset composition.

Datacomp focuses on two key challenges when assembling large training datasets:

1. What data sources to train on,
2. How to filter a given data source.

Their second contribution is **CommonPool**, a dataset of 12.8B image-text pairs collected from Common Crawl. The benchmark *filtering track* is about finding the best methods to filter this CommonPool.

Their third contribution is the baseline experiments where they showed improvements in zero-shot tasks when different filtering techniques were applied to data, these techniques span from querying captions for relevant keywords, filtering based on image embeddings and applying a threshold on CLIP scores.

Their final contribution is the DataComp-1B dataset, a filtered version of CommonPool based on a combination of the two most promising filtering baselines.

