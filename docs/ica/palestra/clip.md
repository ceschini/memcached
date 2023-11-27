# CLIP

## O que é

A proposta dos autores é avaliar o desempenho de classificadores de imagens treinados com supervisão de linguagem natural **em grande escala**, e a sua principal contribuição é o CLIP, um método para pré-treinar tais classificadores.

Contrastive Language-Image Pre-training, ou CLIP, é um ConVIRT simplificado, treinado do zero em um dataset de 400 milhões de pares de imagem-texto, e posteriormente avaliado em diversas tarefas de visão computacional.

## Como funciona

### Dataset

Para viabilizar o treinamento do modelo, os autores desenvolveram um novo dataset com 400 milhões de pares (imagem, texto) a partir de fontes públicas da web. Além de explorar essas fontes cruas, eles também coletaram as palavras com mais de 100 ocorrências na Wikipedia em inglês, aumentando com bi-grams e WordNet synsets, assim criando um conjunto de textos com 500.000 _queries_. Em seguida, eles realizam um balanceamento de classe, incluindo até 20.000 pares (imagem, text) por query.

### Método de Pré-Treino

Os autores detalham a dificuldade de unir os classificadores de imagem e texto a partir dos modelos mais comuns dessas duas arquiteturas. Os classificadores de texto do estado-da-arte atual são muito lentos em comparação com o seu parceiro de imagem. Isso se deve ao fato desses classificadores tentarem predizer exatamente cada uma das palavras presentes no texto. Com isso, para o CLIP foi definido uma tarefa aproximada, onde o modelo de texto tenta definir qual texto na totalidade está relacionado com a imagem. Essa definição, em conjunto com a abordagem de Bag-Of-Words (BOW), foi a escolha otimizada.

> Contrastive objectives can learn better representations than their equivalent predictive objective

— - **Contrastive multiview coding.** Tian, Y., Krishnan, D., and Isola, P.

Dado um batch de $N$ pares de (imagem, texto), CLIP é treinado para predizer quais dos $N X N$ possíveis pares de (imagem, texto) ocorreram durante o batch. Para fazer isso, CLIP treina um encoder de imagem e de texto em um espaço compartilhado e multimodal, onde tenta maximizar a _cosine similarity_ entre os dois embeddings, o da imagem e o de texto, daqueles pares reais que ocorrem no batch, enquanto tentam minimizar a similaridade dos pares incorretos.

## Destaques

