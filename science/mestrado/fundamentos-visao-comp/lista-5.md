# Lista 5 - Estéreo, SfM e fluxo ótico

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