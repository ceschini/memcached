# Anotações Prova 1

### 2) No modelo de câmera _********pinhole********_, qual o efeito do diâmetro do orifício (abertura) na imagem formada?

A abertura da câmera pinhole influencia na quantidade de raios de luz que serão captados e projetados na imagem. O diâmetro de abertura faz com que mais ou menos raios sejam bloqueados, sendo que um diâmetro muito grande pode fazer com que mais de um raio de luz seja refratado pelo mesmo objeto, causando um efeito de borramento. Por outro lado, ao diminuir o diâmetro de abertura, o efeito contrário ocorre, reduzindo o número de raios de luz recebidos, é possível que alguma superfície não seja detectada, causando um borramento parecido.

### 4) Explique o que é aberração cromática.

A aberração cromática ocorre devido ao fato da lente fotográfica ter diferentes graus de refração para cada comprimento de onda, consequentemente havendo um ponto focal diferente para cada um deles. Sensores capazes de captar a luz visível seguem o padrão do olho humano, captando comprimentos de onda de luz que identificamos como vermelho, verde e azul. Como cada uma dessas cores é captada por comprimentos de onda diferentes, o ponto focal da lente não consegue capturar todas elas simultaneamente, acontecendo assim uma distorção colorida nas bordas dos objetos.

### 5) Explique o que é um CFA - ******************Color Filter Array****************** em uma câmera digital, e qual sua funcionalidade.

Color filter arrays são uma malha de blocos coloridos RGB intercalados. São aplicados nos sensores para que os mesmo possam capturar informação colorida. É necessário interpolar cada um dos blocos de cor para estimar a cor real.

### 6) Explique o que é _************depth of field************_, e qual a relação disso com a abertura da câmera.

Depth of field é a distância entre o mais próximo e o mais distante objeto que é capturado com uma definição e foco aceitável. A relação com a abertura de câmera está no fato de que quanto menor a abertura, menos luz irá passar, e com isso mais em foco estará toda a cena observada, aumentando assim o depth of field.

### 1) No filtro linear da média, qual o efeito de aumentarmos o tamanho da máscara na imagem filtrada? Em termos de complexidade computacional, qual estratégia é interessante quando usamos o filtro da média, em particular para janelas maiores?

Ao aumentarmos o tamanho da máscara, a imagem fica com menos ruídos, porém também fica com mais borramento. A estratégia mais interessante quando usamos filtro da média em janelas maiores é as integrais de imagens, aplicando essa técnica para realizar a soma dos pixels e assim reduzir a complexidade final.

### 2) Explique brevemente como funciona um filtro bilateral. Qual a principal diferença entre esse filtro e um filtro passa-baixas (como o filtro Gaussiano)? O filtro bilateral é linear?

O filtro bilateral aplica um filtro gaussiano em conjunto com um kernel de pesos que aumenta ou diminui a intensidade do filtro de acordo com a vizinhança. Isso quer dizer que pixels similares aos seus vizinhos serão filtrados por uma mascara maior. Com isso o filtro bilateral possibilita o realce de bordas e a preservação de características mais finas. Essa constitui a principal diferença em relação a um filtro passa-baixas, pois ele é capaz de preservar detalhes de alta frequência. Não é linear pois a saída não é uma combinação linear dos vizinhos, justamente por ser adaptativo.

### 7) [CMP 197] Mostre que se Hlp(w) é a transformada de Fourier de um filtro passa-baixas, então Hhp(w) = 1 - Hlp(w) é a transformada de Fourier de um filtro passa-altas.

Se Hlp(w) é um filtro passa-baixa, isso quer dizer que os valores acima do limiar definido serão zerados, mantendo assim apenas os valores menores. Com isso, podemos concluir que um filtro passa-alta é justamente o contrário, isso é, ele mantém todos aqueles valores acima do limiar, podendo ser descrito como 1 - Hlp(w).

### 9) Descreva um procedimento para achar um mapa de bordas binário em uma imagem monocromática usando máscaras de Sobel. Se a imagem for colorida, o que podemos fazer?

Ao aplicar filtros que realça diferenças entre pixels em uma direção e suaviza na outra direção, conseguimos obter um mapa de bordas, com linhas de pixels em diferentes ângulos. Se a imagem for colorida é possível calcular os mapas de bordas para cada um dos três canais de cor, unindo as matrizes e preservando as maiores magnitudes.

### 10) O detector de bordas de Canny é bastante popular, e um dos parâmetros é o desvio padrão de um ******kernel****** Gaussiano 2D. Qual o efeito desse parâmetro no mapa de bordas resultante?

O kernel Gaussiano 2D tem o efeito de reduzir o número de bordas ao ir aumentando. O filtro suaviza a imagem e remove ruídos, porém acaba removendo também bordas “fracas”.

### 13) Cite algumas semelhanças e diferenças entre a transformada de Fourier e a transformada Wavelet.

Ambas transformadas conseguem aplicar convoluções e filtros passa baixa ou alta para extrair as frequências das imagens de uma forma global. A principal diferença é que a transformada de Fourier não especifica onde estão localizados, no domínio do tempo, os pontos de altas e baixas frequências (SALOMON, 2000).

### 15) Quando fazemos a representação piramidal de imagens de múltiplas resoluções fazendo o ************downsampling************ pela metade das linhas e colunas a cada nível da pirâmide, é comum suavizarmos a imagem antes do _**********downsapling**********_. Por que isso é necessário? Qual a relação entre essa estratégia piramidal e a essência da transformada Wavelet?

Isso é necessário pois ao aplicarmos o downsampling o nível de detalhes necessário não é alcançado pelo grau de amostragem, ocorrendo o efeito de aliasing, isso é geração de ruídos e inconsistências na imagem reduzida. Esta estratégia piramidal é um precursor da transformada wavelet, que vai reduzindo a imagem para detectar estruturas em diferentes graus de resolução e detalhes.

### 1) Explique brevemente as características principais que um algoritmo de segmentação de imagens deve possuir.

As principais características de um algoritmo de segmentação são o critério de agrupamento e as medidas de similaridade. O critério de agrupamento irá definir para qual grupo o pixel pertencerá de acordo com a sua medida de similaridade, isso é, sua intensidade, cor, textura, etc.

### 2) Quais as limitações das técnicas de segmentação que usam limiares múltiplos? As regiões provenientes de cada faixa de limiar são necessariamente conexas? Justifique.

A principal limitação está no fato de que iluminação não uniforme irá gerar sombras nos objetos e com isso os histogramas de intensidade não terão limiares bem definidos, sendo que a definição exata do limiar algo que irá impactar drasticamente nos resultados. As regiões serão conexas entre si porém não em relação às outras regiões.

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