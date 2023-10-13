# Lista 4 - Line/curve detection, shapes

### 1. Quando fazemos o ajuste de uma reta a um conjunto de pontos aproximadamente alinhados usando o método dos mínimos quadrados, qual o efeito de um (ou alguns) ponto(s) completamente fora da reta (os chamados _****outliers****_) no resultado final? Qual seria uma possível estratégia para diminuir a influência de ********outliers********?

A estimativa do MLS acaba perseguindo os outliers, pois o seu peso em relação aos outros pontos é muito maior, quanto mais fora da reta o outlier é. Uma possível estratégia é adicionar um novo termo à métrica de erro, para penalizar outliers e impedir que eles influenciem na aproximação.

### 2. Considere um conjunto de pontos (******xi, yi******) para _i_ = 1, …, **n**. Queremos achar uma curva paramétrica na forma y = ax + be⁻^x que aproxime os pontos dados. Descreva o procedimento para achar _a_ e _b_ com base no método dos mínimos quadrados.

???

### 3. Considere a parametrização _********************d = x cosθ + y sinθ********************_ para uma reta.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c67c563b-7d0d-45f7-a03f-b4133accb251/Untitled.png)

********************a) [CMP 197] Mostre os parâmetros _d_ e _θ_ representam distância e orientação da reta, conforme a ilustração acima.**

**********************************************************************************************************************b) [CMP 197] Mostre que a menor distância entre um ponto (********x0, y0)******** qualquer à reta é dada por |x0 cos θ + y0 sin θ − d|**

******c) Para uma imagem onde o ≤ x ≤ N e 0 ≤ y ≤ M, quais valores de **d** e _θ_ geram retas com ao menos um pixel visível na imagem?**

### 4. Descreva o funcionamento básico da Transformada Hough (HT), e justifique por que a parametrização usual da reta _********y = ax + b********_ não é adequada.

A Transformada Hough (HT) baseia-se na ideia de um acumulador de pontos em que possíveis retas podem passar. Ele parte da premissa de que um número desconhecido de retas podem estar presentes em uma imagem, e com isso avalia um determinado número de ângulos em que possíveis retas podem estar presentes. Com isso ele adiciona no acumulador cada um desses ângulos em que os pontos geram, retornando assim nos picos do acumulador as mais prováveis linhas encontradas. Com isso, é possível perceber uma dificuldade para a usual parametrização, pois o coeficiente angular possui um intervalo impossível de menos infinito até mais infinito, e com isso linhas verticais não podem ser parametrizadas.

### 5. Várias modificações em torno da HT foram propostas desde sua criação. Cite algumas dessas versões alternativas, e quais as motivações para a introdução dessas modificações.

A eficácia da Transformada Hough em detectar linhas está fortemente relacionada à quantidade de ruído e ao tamanho dos bins que definem os picos. Por conta disso, diversas modificações foram propostas. Gradient Weighted HT explora o gradiente local da orientação das bordas para reduzir o número possível de linhas. Limitando-se a apenas algumas orientações, essa modificação é capaz de reduzir o tempo computacional e o ruído na imagem Hough. HT com voting kernels é uma abordagem que tenta atacar o problema do tamanho dos bins, para isso reduz as linhas capazes de votarem para somente aquelas que estão próximas de cada ponto, com isso destaca mais os picos porém sofre mais ainda com a influência de outliers.

### 6. Uma imagem em tons de cinza contém elipses alinhadas com o eixo _x_, e com excentricidade conhecida*. Descreva um procedimento para detectar tais elipses usando a HT. Qual seria a dimensão do espaço de parâmetros nesse caso?

???

### 7. A HT pode ser aplicada para formas paramétricas genéricas, criando um acumulador de votos no espaço de parâmetros. Quais potenciais problemas podem ocorrer na prática quando a forma possuir muitos parâmetros livres?

???

### 8. Queremos achar a equação da hipérbole abaixo que melhor se ajuste a um conjunto dado de pontos. A formulação do problema como a minimização do erro quadrático se reduz à solução de um sistema linear? Justifique.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ccf51199-2076-4d8f-9a4e-e2d495f479df/Untitled.png)

???

### 9. Considere um **********chain code********** codificado de acordo com a máscara de direções abaixo. Esboce e ache o comprimento das curvas cujos chain codes são dados por:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/92bca58c-3c0a-497d-920f-a3168958078a/Untitled.png)

**********************************a) 00220024446466**********************************

perdi um tempo absurdo tentando fazer isso daqui

**************b) …**************

**************c) …**************

### 10. Quais dos chain codes do exercício anterior representam um contorno fechado? Justifique apenas com base no chain code, não recorra à curva gerada.

???

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