# Lista 2 - Filtragem de Imagens e Multi resolução

### 1) No filtro linear da média, qual o efeito de aumentarmos o tamanho da máscara na imagem filtrada? Em termos de complexidade computacional, qual estratégia é interessante quando usamos o filtro da média, em particular para janelas maiores?

Ao aumentarmos o tamanho da máscara, a imagem fica com menos ruídos, porém também fica com mais borramento. A estratégia mais interessante quando usamos filtro da média em janelas maiores é as integrais de imagens, aplicando essa técnica para realizar a soma dos pixels e assim reduzir a complexidade final.

### 2) Explique brevemente como funciona um filtro bilateral. Qual a principal diferença entre esse filtro e um filtro passa-baixas (como o filtro Gaussiano)? O filtro bilateral é linear?

O filtro bilateral aplica um filtro gaussiano em conjunto com um kernel de pesos que aumenta ou diminui a intensidade do filtro de acordo com a vizinhança. Isso quer dizer que pixels similares aos seus vizinhos serão filtrados por uma mascara maior. Com isso o filtro bilateral possibilita o realce de bordas e a preservação de características mais finas. Essa constitui a principal diferença em relação a um filtro passa-baixas, pois ele é capaz de preservar detalhes de alta frequência. Não é linear pois a saída não é uma combinação linear dos vizinhos, justamente por ser adaptativo.

### 3) CMP 197 - Mostre que o filtro da mediana não é um operador linear.

???

### 4) A filtragem linear usando máscaras de convolução pode ser realizada diretamente no domínio espacial ou no domínio de frequência. Explique quando uma ou outra opção é mais vantajosa do ponto de vista computacional.

Do ponto de vista computacional, a filtragem no domínio espacial é vantajosa nos casos de múltiplas filtragens utilizando kernels pequenos. A complexidade é impactante no momento em que o kernel cresce e se aproxima do tamanho original da imagem. Com isso, uma convolução com um kernel grande será mais rápida no domínio da frequência devido ao fato de que será uma convolução global.

### 5) Calcule a convolução linear entre os vetores _f_ e _g_ dados abaixo:

![lista_2_exercicio_5.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5fc30e54-8412-489e-8649-79a4ce781923/lista_2_exercicio_5.png)

![photo_2023-01-28_18-17-18.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5c4962dd-409e-417e-8e72-720e1d18d0df/photo_2023-01-28_18-17-18.jpg)

![photo_2023-01-28_18-27-30.jpg](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/034cc542-ae95-45f6-a443-aa77d2c9435b/photo_2023-01-28_18-27-30.jpg)

### 6) [CMP 197] Mostre matematicamente que _************f * g = g * f************_

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e0b9314b-b51c-46ad-8178-eb5d657e6234/Untitled.png)

[[mais aqui]](https://thewolfsound.com/mathematical-properties-of-convolution/)

### 7) [CMP 197] Mostre que se Hlp(w) é a transformada de Fourier de um filtro passa-baixas, então Hhp(w) = 1 - Hlp(w) é a transformada de Fourier de um filtro passa-altas.

Se Hlp(w) é um filtro passa-baixa, isso quer dizer que os valores acima do limiar definido serão zerados, mantendo assim apenas os valores menores. Com isso, podemos concluir que um filtro passa-alta é justamente o contrário, isso é, ele mantém todos aqueles valores acima do limiar, podendo ser descrito como 1 - Hlp(w).

### 8) Queremos aplicar um filtro da média 5x5 para reduzir o ruído em uma imagem, e após um filtro ************unsharp mask************ para realçar os detalhes. Se trocarmos a ordem de aplicação desses filtros, a imagem resultante seria afetada? Justifique.

Sim, pois os ruídos atuais da imagem seriam atenuados pelo unsharp, reduzindo assim a eficácia do filtro da média. [[ref]](https://masteryournikon.com/2018/09/25/should-i-sharpen-an-image-before-or-after-noise-reduction/)

### 9) Descreva um procedimento para achar um mapa de bordas binário em uma imagem monocromática usando máscaras de Sobel. Se a imagem for colorida, o que podemos fazer?

Ao aplicar filtros que realça diferenças entre pixels em uma direção e suaviza na outra direção, conseguimos obter um mapa de bordas, com linhas de pixels em diferentes ângulos. Se a imagem for colorida é possível calcular os mapas de bordas para cada um dos três canais de cor, unindo as matrizes e preservando as maiores magnitudes.

### 10) O detector de bordas de Canny é bastante popular, e um dos parâmetros é o desvio padrão de um ******kernel****** Gaussiano 2D. Qual o efeito desse parâmetro no mapa de bordas resultante?

O kernel Gaussiano 2D tem o efeito de reduzir o número de bordas ao ir aumentando. O filtro suaviza a imagem e remove ruídos, porém acaba removendo também bordas “fracas”.

### 11) A figura abaixo mostra o resultado da _****************Discrete Fourier Transform****************_ - DFT (em módulo) de dois sinais unidimensionais com 256 amostras cada. Caracterize cada um deles como contendo predominantemente baixas frequências ou altas frequências, justificando sua resposta.

![lista_2_exercicio_11.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7299d638-8957-417d-bcfd-7da03f2b604b/lista_2_exercicio_11.png)

Ao repetirmos as frequências indefinidamente para entendermos o ciclo, fica claro que a figura da esquerda representa predominantemente frequências baixas, pois o pico de altas frequências ocorre apenas no intervalo 75 - 175, contendo assim apenas 100 das 256 amostras em uma alta frequência. Por outro lado, o sinal da direita possui um vale entre 75 e 175, sendo perpendicularmente oposto ao sinal da esquerda. Com isso podemos afirmar que neste sinal predomina frequências altas, indo do 0 ao 75 e depois do 175 ao 250.

### 12) [CMP 197] Se f[n, m] representa uma imagem N x M e g[n, m] uma máscara de convolução n x m, mostre que a convolução pode ser expressa na forma matricial h = Gf, onde f é um vetor com MN elementos contendo uma rasterização da imagem f, G é uma matriz de dimensões (N + n -1) (M + m - 1) x MN, e h é um vetor com (N + n - 1)(M + m - 1) elementos que representa uma rasterização da convolução h = f * g.

Fiz no tablet mas é basicamente essa figura aí:

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0011c9e2-ff2a-4e59-a367-16488386dc28/Untitled.png)

### 13) Cite algumas semelhanças e diferenças entre a transformada de Fourier e a transformada Wavelet.

Ambas transformadas conseguem aplicar convoluções e filtros passa baixa ou alta para extrair as frequências das imagens de uma forma global. A principal diferença é que a transformada de Fourier não especifica onde estão localizados, no domínio do tempo, os pontos de altas e baixas frequências (SALOMON, 2000).

### 14) Considere uma imagem monocromática com resolução 2^n x 2^n, onde n é um número inteiro positivo. Até quantos níveis podemos realizar a transformada wavelet? Em termos de consumo de memória, quantos elementos devem ser armazenados na transformada para permitir a reconstrução da imagem original?

??????

### 15) Quando fazemos a representação piramidal de imagens de múltiplas resoluções fazendo o ************downsampling************ pela metade das linhas e colunas a cada nível da pirâmide, é comum suavizarmos a imagem antes do _**********downsapling**********_. Por que isso é necessário? Qual a relação entre essa estratégia piramidal e a essência da transformada Wavelet?

Isso é necessário pois ao aplicarmos o downsampling o nível de detalhes necessário não é alcançado pelo grau de amostragem, ocorrendo o efeito de aliasing, isso é geração de ruídos e inconsistências na imagem reduzida. Esta estratégia piramidal é um precursor da transformada wavelet, que vai reduzindo a imagem para detectar estruturas em diferentes graus de resolução e detalhes.