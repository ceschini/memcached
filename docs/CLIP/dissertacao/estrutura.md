# Estrutura da Dissertação
  
foco atual: Zero-shot race classification  using VLMs
  
keywords: Facial attributes recognition, data bias, zero-shot, VLMs  
  
1. introdução  
    - motivação  
    -research questions and hypothesis  
        -- how prompt affect race bias  
        -- how can we mitigate race bias from prompt  
    - objetivos  
    - contribui coes  
        -- avaliação experimental  
    - organização dos capítulos  
  
2. fundamentação teórica  
    - Contrastive Learning  
        -- Contrastive Learning for images (ConVIRT)  
    - Visual Language Models  
        -- Mencionar Language Models  
        -- submodelos (CLIP, Laion, FairerCLIP, DeAR, etc)  
    - Zero-shot image Classification  
        -- Few-shot  
    - Prompt Learning  
        -- CoOp  
        -- CoCoOp  
    - Social Bias - atributos sensíveis (cuidar termos e detalhes sobre isso)  
  
3. trabalhos relacionados  
    - Papers sobre Bias e Model Fairness (como medir bias/fairness)  
        - Papers sobre race classification with VLMs  
  
        - (talvez mencionar gender, occupation, attractiveness)  
  
4. Metodologia  
    - modelos avaliados  
    - datasets utilizados  
        - FairFace  
        - Chicago face dataset  
    - baseline construído (detalhes de implementação) [possível apêndice]  
    - Experimentos (processo de avaliação)  
        -- data scaling  
        -- model scaling  
        -- techniques (prompt learning, prompt ensemble, etc)  
  
5. Resultados  
  
6. Conclusão  
    -- Contribuições  
    -- Limitações  
    -- Trabalhos futuros