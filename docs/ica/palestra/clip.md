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

### Robusto a variações de distribuição

CLIP aprende um conceito de objeto e consegue reconhecê-lo em diferentes estilos e formatos. (Por exemplo, uma foto de uma banana, e um desenho de uma banana)

### Transferência de aprendizado para múltiplas tarefas

Durante seu treinamento, CLIP aprende a discernir diferentes tarefas e conceitos que podem ser utilizados posteriormente para fine-tuning. Essas tarefas incluem, busca de texto e imagens, reconhecimento de caracteres _digitais_, reconhecimento de ações e geolocalização.

### Representações melhores e mais eficientes que ImageNet

Por muito tempo, modelos foram pre treinados a partir do conjunto de dados ImageNet, sendo posteriormente aplicado algum conceito mais específico da tarefa ou problema a ser resolvido. Porém, os autores comprovaram a capacidade do CLIP de ultrapassar os modelos pré-treinados com ImageNet em uma variedade de tarefas, consistindo assim em um novo padrão de pré-treino.

## Limitações

### Eficiência computacional

Mesmo sendo inovador no seu aspecto de pré-treino com supervisão de linguagem natural, CLIP continua abaixo se comparado com o estado da arte, nos datasets com conjuntos de treino. Uma das abordagens dos autores foi escalar o modelo e os dados, porém para alcançar as métricas do estado da arte, seria necessário um aumento computacional de aproximadamente mil vezes.

### Tarefas de Classificação Refinadas

CLIP consegue reconhecer o que é uma flor, um carro ou um avião, mas ele não sabe dizer que tipo de flor é, qual o modelo do carro, nem se o avião é comercial ou militar.

### Tarefas Abstratas e Sistemáticas

Contar o número de objetos em uma imagem, por exemplo.

### Vieses

Pares de imagem e texto não são filtrados, o que resulta no aprendizado de diferentes vieses sociais.

## Vieses

CLIP tem uma grande variedade de tarefas em que pode ser aplicado, e o design das classes feito de uma forma tão livre pode ser perigoso. O modelo pode dizer o que é um gato ou um cachorro, mas pode dizer também quem é um bandido num mercado, um terrorista em um aeroporto ou uma garota de programa em uma festa. Como qualquer ferramenta, é o nosso uso dela que distingue algo útil para o nosso trabalho, de uma arma capaz de ferir outra pessoa.

CLIP demonstrou vieses que infelizmente são comuns em nossa sociedade. Fotos de homens são mais frequentemente classificadas como "doutor", do que fotos de mulheres. O inverso acontece ao classificar imagens com o rótulo de "garçom", ou "empregada". Além disso, vieses de raça também estão presentes, classificando com maior frequência pessoas de cor parda e negra com rótulos não-humanos ou pejorativos. 

Essa questão está ainda muito em aberto e não existem esquemas de testes robustos para os desenvolvedores melhor caracterizar os vieses dos seus modelos de IA. Para tal, a exploração da comunidade será crucial, de modo a contextualizar e ampliar esses esquemas de testes.

Estudos foram propostos para corrigir esses vieses a partir de prompt engineering ou feature engineering, e mesmo que sejam capazes de resolver os casos específicos em que foram testados, não temos como saber quais outros vieses irão surgir, e nem se esses métodos serão capazes de mitigá-los.
