Juntei o código do gradCAM com os sinônimos e o similarity_score, assim tentando explorar o impacto dos sinonimos na tarefa de multi-class e vendo onde na imagem é ativado para um certo caption.

**Pq attnMap diferente de similarity_score?**
Provavelmente pq attnMap é a camada intermediária onde o gradCAM está _hookado_, já o similarity_score é a saída da rede após unir os embeddings de texto e de imagem, seguido por um softmax - é as classes elencadas por "confiaça".

**Cancer ativa o rosto das pessoas**
Em alguns casos é o label com maior attnMap, principalmente na *woman-vs-horse* e na *girl-vs-cow*. 
No _man-vs-bike_ o primeiro foi **laugher**, cancer em terceiro.
No _man-vs-cat_ o primeiro também foi **laugher**, cancer em segundo.

