# Lista 1 - Introdução, Modelos de Câmeras

### 1) Cite alguns sensores e aplicações que capturam informação fora do espectro visível. Como acabamos visualizando tal informação?

Existem sensores capazes de emitir e absorver raios fora do espectro visível, como raios-X, raios gama e raios ultravioleta. Podem ser utilizados para capturar estruturas ósseas, cerebrais e genéticas, por exemplo. As intensidades captadas por estes sensores são mapeadas para o espectro visível, em cores ou tons de cinza capazes de simbolizar os valores observados fora do espectro.

### 2) No modelo de câmera _********pinhole********_, qual o efeito do diâmetro do orifício (abertura) na imagem formada?

A abertura da câmera pinhole influencia na quantidade de raios de luz que serão captados e projetados na imagem. O diâmetro de abertura faz com que mais ou menos raios sejam bloqueados, sendo que um diâmetro muito grande pode fazer com que mais de um raio de luz seja refratado pelo mesmo objeto, causando um efeito de borramento. Por outro lado, ao diminuir o diâmetro de abertura, o efeito contrário ocorre, reduzindo o número de raios de luz recebidos, é possível que alguma superfície não seja detectada, causando um borramento parecido.

### 3) No modelo de lentes finas (**********thin lens**********), um objeto a uma distância _do_ de uma lente com distância focal _f_ estará em foco a uma distância **di** da lente (do outro lado da mesma). Se uma câmera fotográfica segue esse modelo, como se faz para focar um objeto em uma dada distância? O que ocorre com **di** quando ************do************ tende ao infinito?

“Para focar um objeto em uma dada distância com uma câmera digital, é necessário mover a câmera ou ajustar a distância focal da lente. Quando do → ∞, di = f e tem-se o foco em raios paralelos; sendo assim, todos os objetos a uma distância infinita ou superior estão em foco.” - Heloisa.

### 4) Explique o que é aberração cromática.

A aberração cromática ocorre devido ao fato da lente fotográfica ter diferentes graus de refração para cada comprimento de onda, consequentemente havendo um ponto focal diferente para cada um deles. Sensores capazes de captar a luz visível seguem o padrão do olho humano, captando comprimentos de onda de luz que indetificamos como vermelho, verde e azul. Como cada uma dessas cores é captada por comprimentos de onda diferentes, o ponto focal da lente não consegue capturar todas elas simultâneamente, acontecendo assim uma distorção colorida nas bordas dos objetos.

### 5) Explique o que é um CFA - ******************Color Filter Array****************** em uma câmera digital, e qual sua funcionalidade.

Color filter arrays são uma malha de blocos coloridos RGB intercalados. São aplicados nos sensores para que os mesmo possam capturar informação colorida. É necessário interpolar cada um dos blocos de cor para estimar a cor real.

### 6) Explique o que é _************depth of field************_, e qual a relação disso com a abertura da câmera.

Depth of field é a distância entre o mais próximo e o mais distante objeto que é capturado com uma definição e foco aceitável. A relação com a abertura de câmera está no fato de que quanto menor a abertura, menos luz irá passar, e com isso mais em foco estará toda a cena observada, aumentando assim o depth of field.

### 7) Considere dois pontos no plano cujas coordenadas homogêneas são ****x1**** = (x1, y1, s1) e x2 = (x2, y2, s2). Pense em um critério para determinar quando esses vetores representam o mesmo ponto no espaço.

Os vetores irão representar o mesmo ponto no espaço quando a divisão de xW e yW por sW resultar no mesmos valores para x e y. Isso é **x1/s1 = x2/s2** e **y1/s1 = y2/s2.**

### 8) Mostre que a rotação em torno de um ponto x0 no espaço NÃO É uma transformação linear de R³ em R³. Mostre que, usando coordenadas homogêneas, tal transformação é linear.

?????

### 9) Considere uma câmera na origem do sistema de coordenadas de mundo, e com orientação canônica (ou seja, R = I). Se a matriz K com os parâmetros intrínsecos é dada por:

![intrinsecs.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2d09f9ef-abc8-47ae-8da7-2f7f7452a967/intrinsecs.png)

### mostre que duas retas no espaço ortogonais ao plano de projeção (ou seja, retas paralelas ao eixo z) se interceptam no plano no ponto (uc, vc).

??????

### O resto é tudo horrívelmente difícil e vai me travar então vou ignorar e foda-se 🙂