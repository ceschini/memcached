# Background

## Treinamento do GPT-3

Para treinar grandes modelos de linguagem natural, como o GPT-3, é utilizado um conceito de treinamento não supervisionado, com aprendizado de contexto. Nesse caso, o modelo é alimentado com dados não rotulados dos mais variados tipos, e com o decorrer dos passos vai reconhecendo padrões e tarefas, que posteriormente se recorda ao ser abordado com alguma necessidade específica.

## Supervisão com Linguagem Natural

A grande vantagem de utilizar um modelo de supervisão mais "relaxado" está no fato de que não é necessário gastar recursos com anotação. Ao invés disso, basta utilizar os textos alternativos e os rótulos que os próprios usuários atribuíram para a imagem em questão. Isso já foi explorado anteriormente, porém os últimos avanços em NLP propiciaram o seu uso em grande escala (deep contextual representation learning).

### Vantagens

- Fácil de escalar
- Conecta a representação com a linguagem e assim possibilita "flexible zero-shot transfer".

## ConVIRT

## Contrastive Learning of Medical Visual Representations from Paired Images and Text

Treinar classificadores de imagem requer normalmente exemplos muito bem anotados para o treinamento supervisionado. No meio médico isso é caro e escarço, sendo uma das soluções mais comuns utilizar modelos pré-treinados em datasets mais robustos, como, por exemplo, o ImageNet. O problema se encontra no fato de que as imagens médicas, como aquelas de raio-x, por exemplo, possuem detalhes singelos, que não se adequam aos exemplos aprendidos do ImageNet, aonde grandes partes da imagem mudam de um exemplo para o outro.

"Our work is inspired by the recent line of work on **image view-based contrastive learning\***, but differs by *exploiting contrastive learning using the text modality*."

**\* Data-efficient image recognition with contrastive predictive coding**

![[convirt-framework.png]]

The blue and green shades represent the image and text encoding pipelines, respectively. **Our method relies on maximizing the agreement between the true image-text representation pairs with bidirectional losses $l(v-->u)$ and $l(u --> v)$.**

Eles usam uma função de projeção que joga uma representação do encoding de texto e vídeo para o mesmo espaço d-dimensional, assim possibilitando o *contrastive learning*.