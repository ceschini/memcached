# Learning Transferable Visual Models From Natural Language Supervision

## First pass

### Context

They explore how computer vision models can leverage the capabilities of NLP to train task-agnostic and zero-shot learning using text-to-image and web crawling. The related papers include those that explore how captions and labels can improve computer vision models in classification tasks. That is, papers that use *natural language supervision for image representation learning*.

### Contributions

They trained a simplified version of the *ConVIRT* from scratch using 400 million image-text pairs and showed how it can zero-shot classify more specific tasks.

Basically they grabbed an existing technique and explored its limited performance due to lack of computation, leveraging openAI processing capabilities.

## Second pass

There are a lot of concepts and papers that I should look into before grasping what the paper is about. But as far as I could tell, they trained a massive model and then spent the rest of the time evaluating it, the same thing that we are doing. They explored their model capabilities on different tasks and pointed out some possible future works.

The key aspect that I should try to understand is the contrastive learning technique that allowed the image and text embeddings to share a common space.

