# Contrastive Learning of Medical Visual Representations from Paired Images and Text

## ConVIRT

Treinar classificadores de imagem requer normalmente exemplos muito bem anotados para o treinamento supervisionado. No meio médico isso é caro e escarço, sendo uma das soluções mais comuns utilizar modelos pré-treinados em datasets mais robustos, como, por exemplo, o ImageNet. O problema se encontra no fato de que as imagens médicas, como aquelas de raio-x, por exemplo, possuem detalhes singelos, que não se adequam aos exemplos aprendidos do ImageNet, aonde grandes partes da imagem mudam de um exemplo para o outro.

"Our work is inspired by the recent line of work on **image view-based contrastive learning\***, but differs by *exploiting contrastive learning using the text modality*."

**\* Data-efficient image recognition with contrastive predictive coding**

![[docs/img/convirt-framework.png]]

The blue and green shades represent the image and text encoding pipelines, respectively. **Our method relies on maximizing the agreement between the true image-text representation pairs with bidirectional losses $l(v-->u)$ and $l(u --> v)$.**

Eles usam uma função de projeção que joga uma representação do encoding de texto e vídeo para o mesmo espaço d-dimensional, assim possibilitando o *contrastive learning*.