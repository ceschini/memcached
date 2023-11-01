# Anotações Prova 2

### 1. Quando fazemos o ajuste de uma reta a um conjunto de pontos aproximadamente alinhados usando o método dos mínimos quadrados, qual o efeito de um (ou alguns) ponto(s) completamente fora da reta (os chamados _****outliers****_) no resultado final? Qual seria uma possível estratégia para diminuir a influência de ********outliers********?

A estimativa do MLS acaba perseguindo os outliers, pois o seu peso em relação aos outros pontos é muito maior, quanto mais fora da reta o outlier é. Uma possível estratégia é adicionar um novo termo à métrica de erro, para penalizar outliers e impedir que eles influenciem na aproximação.

### 4. Descreva o funcionamento básico da Transformada Hough (HT), e justifique por que a parametrização usual da reta _********y = ax + b********_ não é adequada.

A Transformada Hough (HT) baseia-se na ideia de um acumulador de pontos em que possíveis retas podem passar. Ele parte da premissa de que um número desconhecido de retas podem estar presentes em uma imagem, e com isso avalia um determinado número de ângulos em que possíveis retas podem estar presentes. Com isso ele adiciona no acumulador cada um desses ângulos em que os pontos geram, retornando assim nos picos do acumulador as mais prováveis linhas encontradas. Com isso, é possível perceber uma dificuldade para a usual parametrização, pois o coeficiente angular possui um intervalo impossível de menos infinito até mais infinito, e com isso linhas verticais não podem ser parametrizadas.

### 5. Várias modificações em torno da HT foram propostas desde sua criação. Cite algumas dessas versões alternativas, e quais as motivações para a introdução dessas modificações.

A eficácia da Transformada Hough em detectar linhas está fortemente relacionada à quantidade de ruído e ao tamanho dos bins que definem os picos. Por conta disso, diversas modificações foram propostas. Gradient Weighted HT explora o gradiente local da orientação das bordas para reduzir o número possível de linhas. Limitando-se a apenas algumas orientações, essa modificação é capaz de reduzir o tempo computacional e o ruído na imagem Hough. HT com voting kernels é uma abordagem que tenta atacar o problema do tamanho dos bins, para isso reduz as linhas capazes de votarem para somente aquelas que estão próximas de cada ponto, com isso destaca mais os picos porém sofre mais ainda com a influência de outliers.

### 12. Considere os descritores de contorno ******length****** (comprimento), diameter (diâmetro) e eccentricity (excentricidade), além dos descritores de regiões compactness (compacidade) e Euler number, conforme visto em aula. Considere os objetos dados abaixo:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2baf0b4b-c920-4019-9c5c-1884c1abe3ca/Untitled.png)

********************************************************************************************a) Se considerarmos que os objetos acima são******************************************************************************************** similares **************************************************************entre si, qual(is) descritor(es) seriam adequados? Justifique**************************************************************

Diâmetro, pois a distância máxima entre dois pontos extremos são correspondentes, e os eixos máximos e mínimos são parecidos.

******b) Se considerarmos que os objetos acima são****** diferentes ****************************************************************************************************************************entre si, qual(is) descritor(es) seriam adequados? Justifique****************************************************************************************************************************

Comprimento, pois o número de pixels nas bordas varia muito. Excentricidade, as razões entre os eixos difere muito.

### 13. Explique o que é a Transformada Distância (************************Distance Transform - DT)************************ aplicada a uma imagem binária.

A Transformada Distância em um determinado pixel representa a distância mínima daquele pixel até o ponto mais próximo da imagem. Pode ser medido por distância euclidiana, Citiblock ou Chessboard

### 14. Explique o que é o ********Skeleton******** (************Medial Axis)************ de um objeto.

Skeleton pode ser usado para representar o formato de regiões de um objeto. Ele é aplicado ao se expandir circularmente os limites de um determinado ponto até que ele não consiga mais permanecer contido, assim o seu centro é considerado um ponto do medial axis. Em outras palavras, os pontos no medial axis são os centros da vizinhança máxima circular que esteja totalmente contida na forma.

### 15. Descreva matematicamente o índice de Jaccard, e indique uma aplicação concreta onde ele pode ser utilizado.

O índice Jaccard é uma medida de similaridade, e pode ser utilizado como uma métrica de avaliação de modelos de segmentação, ao comparar a intersecção da união dos conjuntos de duas imagens binárias, por exemplo.

Ele pode ser descrito como a razão entre a intersecção e a união de dois conjuntos, normalizado pela área total. Quanto mais próximo de 1, mais similar os dois conjuntos são. Sendo 1 apenas quando ambos são iguais, e 0 quando os conjuntos são disjuntos.

### 16. Explique as principais diferenças entre as distâncias de Haussdorff e de Frèchet para medir a dissimilaridade entre duas curvas. Dê exemplos de situações onde uma ou outra são mais adequadas.

A principal diferença está no fato de que Frechet leva em conta tanto a localização mas também a ordem dos pontos ao longo dos conjuntos. Haussdorff é mais simples e é mais adequado à situações onde não ocorre rotação, e o objeto de interesse tem compacidade definida. Já Frechet é mais adequado ao comparar a trajetória de objetos, ou ajuste de superfícies.

### 1. Explique o que são linhas epipolares no contexto de geometria de duas vistas.

Linhas epipolares são as linhas originadas da projeção de um ponto na câmera vizinha. A partir da linha epipolar é possível restringir o espaço paramétrico da segunda câmera e assim encontrar os pontos correspondentes dentre as duas imagens.

### 2. Explique o que significa o processo de “retificação” de imagens no contexto de casamento estéreo, e por que isso é feito. O que se pode dizer sobre as linhas epipolares após a retificação?

O processo de retificação envolve a re-projeção de duas imagens em um plano paralelo comum, garantindo assim que um ponto da imagem A cai exatamente no mesmo ponto na imagem B, no que diz respeito ai eixo vertical. Isso é necessário pois o casamento estéreo é feito com janelas deslizantes apenas na horizontal. Com isso é possível afirmarmos que as linhas epipolares serão horizontais e colineares.

### 3. No casamento de pontos estéreo ao longo da linha epipolar, é comum analisarmos o erro entre regiões em torno do pixel (e não apenas o pixel em consideração). Por que isso é feito? Qual o efeito de aumentarmos o tamanho dessa janela?

Isso é feito devido ao fato de aplicarmos o Sum of Squared Differences (SSD) para medir a distância entre os pixels, e apenas dois pontos não são o suficiente para medir com acurácia devido à alta influência de ruído. Ao aumentarmos a janela, ficamos menos suscetíveis aos ruídos, porém perdemos informação devido ao efeito de borramento.

### 4. No pipeline estéreo tradicional, explique brevemente o processo de agregação

O processo de agregação envolve o cálculo da correlação entre os mapas estéreos, acumulando-os e gerando um volume de custo tridimensional.

### 6. Descreva ao menos duas técnicas atuais para projeção e visualização de dados estereoscópicos.

Existem duas técnicas principais quando o assunto é televisões com tecnologia 3D. A primeira é o 3D Polarizado, onde as imagens de ambas as câmeras são projetadas ao mesmo tempo, e um óculos polarizado faz com que cada olho visualize apenas uma imagem. A segunda abordagem é o 3D Obstruído, onde apenas uma imagem é mostrada em um determinado momento, e um óculos sincronizado é responsável por bloquear a visão do olho correspondente.

### 7. Explique o que são as matrizes fundamental e essencial, e qual a principal diferença entre elas.

A matriz fundamental é a representação algébrica da geometria epipolar, e está relacionada ao mapeamento entre um ponto em uma imagem e a sua linha epipolar na segunda. Esta matriz é usada para descobrir as linhas epipolares quando os parâmetros intrínsecos não são conhecidos. Já a matriz essencial é um caso particular da matriz fundamental quando os parâmetros intrínsecos são conhecidos, e pode ser utilizada para obter os parâmetros extrínsecos da câmera.

### 11. Explique o problema da abertura (****************aperture problem****************) no contexto de fluxo ótico.

O problema da abertura refere-se à ambiguidade em determinar a real velocidade utilizando um detector de movimento local, ocasionado normalmente por regiões homogêneas e bordas.

### 13. No rastreamento de pontos, quais são as vantagens/desvantagens de técnicas iterativas baseadas no gradiente (como Lucas Kanade) em comparação com busca direta (exaustiva)?

Técnicas iterativas baseadas no gradiente combinam a informação de diversos pixels próximos, e com isso é capaz de resolver a ambiguidade inerente da busca direta, além de ser menos sensível à ruídos. Por outro lado, o fato desta técnica ser puramente local, ela não é capaz de resolver o rastreamento de pontos no interior de regiões uniformes e homogêneas da imagem.