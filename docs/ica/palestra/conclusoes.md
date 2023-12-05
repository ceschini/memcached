# Conclusões

O treinamento a partir da supervisão de linguagem natural pode ser a resposta para os problemas principais do treinamento supervisionado de modelos de deep learning: disponibilidade de dados e rotulagens de qualidade.

Avanços recentes na intersecção das áreas de visão computacional e NLP propiciaram o casamento de classificadores de imagens e textos em um mesmo modelo capaz de compartilhar o espaço dos embeddings, e assim convergir concorrentemente.

CLIP demonstrou capacidades competitivas em diversas tarefas de visão computacional, sem nenhum tipo de treinamento adicional. O modelo foi treinado em um novo dataset contendo 400 milhões de pares de imagens e texto, e pode ser considerado um marco para a visão computacional, desbancando os modelos pré-treinados com ImageNet. Esse avanço é significativo por propiciar um método mais eficiente de treinamento de modelos para tarefas mais específicas, além de sugerir um novo paradigma de treinamento a partir da vasta quantidade de dados atualmente disponíveis de forma pública na web.

Porém, esse tipo de treinamento sem uma supervisão ativa e sem filtros pode aprender conceitos deturpados e pejorativos, resultando em propagar e acentuar vieses sociais. Um cuidado muito grande deve ser feito, com medidas para mitigar os danos possíveis ao utilizar ferramentas como o CLIP de forma mal intencionada.

Atualmente não existem formas de medir e testar os modelos de linguagem e visão para descobrir e corrigir esses vieses.

Com isso, podemos concluir que, mesmo que o CLIP seja inovador como um método eficiente de pré-treino a partir da supervisão de linguagem natural, ele ainda deve ser explorado e melhorado.