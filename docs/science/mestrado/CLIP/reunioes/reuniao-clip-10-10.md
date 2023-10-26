**Pq attnMap diferente de similarity_score?**
Provavelmente pq attnMap é a camada intermediária onde o gradCAM está _hookado_, já o similarity_score é a saída da rede após unir os embeddings de texto e de imagem, seguido por um softmax - é as classes elencadas por "confiaça".

esse attnMap nao parece muito util.

## TODOS

- Ler os papers que o claudio linkou
	- Zotero marquei varios como "lido", preciso achar os resumos
- Explorar bias
	- Horse e Rider
	- ...
