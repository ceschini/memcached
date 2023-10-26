## Avanços

- modelo yuv, ycbcr
	- Qual melhor? Só testando
	- modelo yuv "killed" na época 10
		- mensagem de shuffle buffer
		- 50 min por epoch
		- problema com datatypes? - compartilhar notebook
- Datasets com texto
	- Os mais famosos são de OCR ou VQA - Visual Question Answering
	- low-res ou highres variado.
	- Pensei em pegar datasets de super-resolution e rodar um detector OCR para filtrar imagens com texto.

## Propostas

A gente deveria padronizar algumas coisas no desenvolvimento. Principalmente os seguintes passos:
- load dataset
- preprocessing
- model class
- **scales**
	- Quando é necessário um scaling específico, adaptar diretamente na função que precisa.
	- Modelos de ML não são melhores com range \[0-1]?

Na verdade, eu posso fazer essas coisas, porém, apenas após propor adicionar isso no Trello como uma tarefa.

## Novas tasks

- **Filtrar por resolução esses datasets** +1000 pixeis w ou h
- **Reduzir batch size no parâmetro do shuffle.**
