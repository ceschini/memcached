# Introdução

## Classificadores de imagens

>"Classificação de imagens é o processo de categorizar e rotular grupos de píxeis ou vetores numa imagem com base em regras específicas. A lei de categorização pode ser concebida usando uma ou mais características espectrais, ou textuais."
>
> -- *Synthetic aperture radar and remote sensing technologies for structural health monitoring of civil infrastructure systems*

É uma tarefa fundamental na área de visão computacional, cujo objetivo é entender e categorizar uma imagem sob um rótulo específico.

## Principais tarefas

Reconhecimento de dígitos, rotular imagens automaticamente com texto alternativo, busca visual, reconhecimento facial, diagnóstico médico, etc.

## Como funciona

[ref](https://levity.ai/blog/image-classification-in-ai-how-it-works)

Primeiramente, é realizado uma etapa de pré-processamento da imagem de entrada. Nessa etapa as transformações necessárias para alimentar um modelo de deep learning é aplicado na imagem, como, por exemplo, normalização, recorte, realce de brilho ou contraste.

Em seguida, a imagem processada é fornecida para o algoritmo de rede neural, que irá aprender a detectar os objetos da imagem. Essa etapa envolve contornar o objeto em questão, na sua região de interesse (ou ROI) com uma estrutura conhecida como _bounding box_. Essa imagem pode ser recortada ou não, a partir das coordenadas de pixeis da bbox.

Essa imagem resultante é então classificada com um dos rótulos de interesse, pelo classificador de objetos. Por fim, o processo de aprendizado será aplicado, caso o modelo tenha acertado ou não a classificação.

## Dificuldades

Existem diversas dificuldades inerentes do uso de aprendizado supervisionado. O modelo precisa de dados para aprender a classificar imagens de contextos específicos, e uma arquitetura eficaz para um tipo de problema deve ser retreinada para ser utilizada em outro.

Generalizar esses modelos para outras tarefas requer dados rotulados adicionais, e nem sempre eles estão disponíveis na quantidade e qualidade necessária.

## Solução

A área de processamento de linguagem natural foi revolucionada recentemente pelo advento do chatGPT, um sistema que tira vantagem da vasta quantidade de informação textual disponível na internet para modelar uma interface de texto-para-texto, assim propiciando uma arquitetura genérica capaz de ser aplicada aos mais diversos contextos sem a necessidade de treinamento ou customização adicional. 

Esse tipo de aprendizado a partir de linguagem natural pode ser explorado também na área de visão computacional para resolver alguns dos problemas inerentes do aprendizado supervisionado, como, por exemplo, a necessidade de dados rotulados e treinamento e customização adicional.

