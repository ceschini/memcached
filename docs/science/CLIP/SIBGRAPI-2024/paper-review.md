# SIBGRAPI Paper Reviews

## Review 1

### Paper Weaknesses and Tips for Improving:

- Better justify the novelty of the paper
- Improve discussion of related work
- Deepen experiments
- Conclusion is not succinct

#### Minor issues

- Page 1: GDif is mentioned without having been explained before.
- Gap column undefined in Tables III and IV.
- Table VII with top k should present a graph, k values seem arbitrary.

## Review 2

### Paper Weaknesses and Tips for Improving:

- Lack of novelty
- "The 4.4% improvement in accuracy is true but does not tell the majority case story."
- "The aggregate score, currently a simple mean, could be elevated to a more advanced level by employing sophisticated aggregator functions, inspiring research in this area."

## Review 4

### Paper Weaknesses and Tips for Improving:

- The authors fail to position their work in relation to the existing literature.
- Choice of related work should have been based on gender classification on FairFace.
- The paper fails to convey to the reader the current state of the art.
- Methodology imprecise in describing dataset evaluation protocol (entire dataset or test set?)
- Introduction fails to define the research problem.
- FairFace dataset is absent from abstract and introduction.
- Conclusion overly lengthy
- Explore whether FairFace was used to train WIT or LAION. 

### Other issues:

- "Gadre and colleagues \[14] introduced CommonPool" should be "Gadre et al".
- Introduction should formalize the problem simillarly to section III.A
- Research questions should be stated more explicitly.
- Incoherent "For all models, the \[...] achieved the highest accuracy for some models"

### Suggestions for Improvement:

- Broaden the scope of related works to include studies that specifically address gender classification using the FairFace dataset and CLIP-based models.
- Provide more detailed explanations about the use of the FairFace dataset, particularly whether the entire dataset or only the test partition was used.
- Introduction should clearly formalize the problem we are addressing and explicitly state the research questions and objectives.
- Clarify overlaps between training datasets and FairFace dataset, to address potential data leakage concerns.
- Condense conclusion to focus on key findings, implications and future research directions.
- Add tables or figures that compare our results with those from previous studies on similar tasks.
- Provide public repo.
