# Reunião de Acompanhamento Mensal com Petrobras

## Apresentação

1. correlação temporal
3. optical flow
4. non-local priors
5. RNNs
6. convLSTM
8. papers

## Textos por slide

### TextCaps

Não basta apenas reconhecer caracteres e ler o que está escrito, precisa também compreender o que está escrito e qual o contexto disso.

Não é só um placar de um jogo de futebol, é um jogo onde o RICE tá ganhando. O outro não é só 17,88 no wallmart coisa de emagrecer, é uma **promoção**. O outro não é só customer information, informação para o cliente, é uma piada com os vegeratianos.

**ECCV**: European Conference on Computer Vision

### RAE-RPM Intro

Dois modelos, um auto-encoder e um de probabilidade. O auto-encoder gera representações latentes da imagem de entrada, e o de probabilidade codifica a imagem para gerar o bit stream.

### RAE-RPM Visual

bpp = bits per pixel

### RAE-RPM Metrics

#### BDBR - Bjontegaard Delta Bit-Rate

Calculates the average bit-rate difference in comparison with the anchor (x265). Lower BDBR value indicates better performance, and negative BDBR indicates **saving bit-rate** in comparison with the anchor, _i.e._, outperforming the anchor.

Calcula a diferença média de bit-rate vs anchor. Quanto menor melhor. Negativo indica que está salvando bit-rate em comparação com o anchor.

### Joint Spatial-Temporal Intro

Eles realizaram uma fusão de correlação espacial e temporal. Para correlação temporal exploraram estatísticas de primeira e segunda ordem. Primeira ordem foca em intensidade e orientação, gerando optical flows. Segunda ordem foca em aceleração, comparando os optical flows dos frames.

### Joint Spatial-Temporal Visual



### Joint Spatial-Temporal Métricas



### Kalman Intro



### Kalman methods



### Kalman visual



### Kalman metrics


