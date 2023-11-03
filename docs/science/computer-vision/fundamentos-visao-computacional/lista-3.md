# Lista 3 - Segmentação, cor e textura

### 1) Explique brevemente as características principais que um algoritmo de segmentação de imagens deve possuir.

As principais características de um algoritmo de segmentação são o critério de agrupamento e as medidas de similaridade. O critério de agrupamento irá definir para qual grupo o pixel pertencerá de acordo com a sua medida de similaridade, isso é, sua intensidade, cor, textura, etc.

### 2) Quais as limitações das técnicas de segmentação que usam limiares múltiplos? As regiões provenientes de cada faixa de limiar são necessariamente conexas? Justifique.

A principal limitação está no fato de que iluminação não uniforme irá gerar sombras nos objetos e com isso os histogramas de intensidade não terão limiares bem definidos, sendo que a definição exata do limiar algo que irá impactar drasticamente nos resultados. As regiões serão conexas entre si porém não em relação às outras regiões.

### 3) [CMP 197] O histograma de uma certa imagem em tons de cinza é bimodal, e os modos são dados por curvas Gaussianas da forma:

![lista_3_exercicio_3.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6b2a4d70-3a64-4fed-be04-b69421eaa416/lista_3_exercicio_3.png)

### A escolha de _T = (m1 + m2)/2_ é adequada? Caso negativo, como se poderia escolher _T_?

???

### 4) Qual a essência das técnicas de segmentação baseadas em **********region growing**********? Cite algumas métricas que podem ser usadas para agregar pixels no processo de crescimento de regiões.

A essência é definir alguns pixels como pontos iniciais de grupos, e ir aumentando o seu tamanho enquanto a métrica de similaridade entre eles se mantém constante. A métrica de similaridade pode ser cor, textura, intensidade, etc.

### 5) Explique brevemente o funcionamento básico do algoritmo **********watersheds**********. Explique como a transformada _********watershed********_ pode ser usada para segmentar imagens.

O algoritmo de watersheds itera sobre os mínimos locais e vai agrupando valores enquanto os grupos não se encontram. Assim que ocorre uma intersecção entre os grupos um limiar é definido naquele ponto, sendo considerado um watershed. A transformada pode ser utilizada para segmentar imagens a partir de uma mapa de bordas, com isso cada um dos watersheds coincidirá com uma região de um objeto de interesse.

### 6) Um problema conhecido da transformada _********watershed********_ na segmentação de imagens é a obtenção de um número demasiado de regiões (em sua maioria, pequenas). Explique por que isso ocorre, e como esse problema pode ser atenuado.

Isso ocorre devido ao fato da transformada watershed identificar regiões a partir dos locais mínimos de uma imagem. A segmentação a partir disso pode resultar em um número demasiado de regiões devido ao fato de ruídos gerarem um número grande de locais mínimos. Para atenuar esse problema podemos aplicar filtros passa-baixa para suavizar a imagem, binarizar o mapa de bordas a partir de algum limiar ou até definir manualmente os pontos de interesse.

### 7) Explique brevemente (sem detalhes matemáticos) os conceitos básicos que fundamentam a segmentação por contornos ativos (******snakes******).

A segmentação por contornos ativos utiliza uma função de aproximação que tem como objetivo minimizar a distância entre um contorno inicial e uma função de energia. As repetidas iterações tem como objetivo moldar o formato da curva e aproximá-la da região de interesse a partir de uma função parametrizada por valores que controlam rigidez e tensão, penalizando quando o contorno é muito distendido ou encurvado.

### 8) Como podemos realizar a segmentação de imagens coloridas usando técnicas de **********clustering**********? Em geral, esse tipo de abordagem gera regiões conexas?

Podemos definir um número K de grupos, um para cada cor chave. Definimos os pontos centrais para cada um desses grupos, um pixel aleatório que melhor se encaixa em cada um dos grupos. A partir daí, iremos analisar os vizinhos de cada um desses pixels centrais, o pixel mais próximo de um determinado centróide agora faz parte desse grupo. Depois, calculamos a média dos pontos de cada grupo para definir o novo ponto central, repetindo assim até todos os pixels fazerem parte de um grupo. Em geral, esse tipo de abordagem não irá gerar regiões conexas, pois o mesmo valor de pixel pode ocorrer em locais esparsos e serem definidos para o mesmo cluster.

### 9) Explique o conceito básico sobre _**********super-pixels**********_, e indique possíveis aplicações.

Super-pixels são grupos de pixels com características similares. Aderem aos limites das imagens e devem representar regiões semanticamente úteis. São obtidos por agrupamento local de pixels similares. Podem ser utilizados para segmentação de alto nível ao agrupar super-pixels, mas também podem ser utilizados em outras aplicações como reconhecimento de objetos, compressão etc.

### 10) O que é a teoria da tricromaticidade na visão humana?

Essa teoria diz que para qualquer cor do espectro visível, existe uma combinação linear de três primárias colorimetricamente independentes capazes de se ajustar à essa cor.

### 11) O que é metamerismo?

Dependendo de uma intensidade de luz específica, dois espectros de cor podem ser assimilados como sendo a mesma cor. Pares metaméricos são tonalidades que parecem idênticas sob uma única combinação de iluminação específica, mas que, na verdade, têm refletância diferente.

### 12) O que significa um espaço de cores uniforme? O espaço RGB é uniforme?

Um espaço de cor uniforme significa que a distância entre a cor observada e a sua magnitude no espaço de cor são comparáveis e significativas. O espaço de cor RGB não é uniforme pois as diferenças de cores não correspondem à sua distância em magnitude.

### 13) Explique o que são ************************color matching functions************************ em um sistema de cores. O que ocorre se uma dessas funções possui valores negativos? Cite um sistema de cores onde isso ocorre.

Color matching functions são as funções necessárias para reproduzir uma cor específica utilizando cores primárias com diferentes intensidades individuais. Para que isso seja possível as leis de Grassman devem se aplicar. Se uma dessas funções possui valores negativos, a cor em questão não pode ser representada nesse espaço de cor.

### 14) Explique o que é uma superfície Lambertiana.

Uma superfície Lambertiana possui um modelo de reflexão difusa, que significa que terá o mesmo sinal de reflexão independente do ponto de vista, desde que se mantenha o mesmo ponto de iluminação.

### 15) Bla