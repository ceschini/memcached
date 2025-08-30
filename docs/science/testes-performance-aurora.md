# Testes de Performance do Sistema Aurora

## Introdução

Afim de avaliarmos o estado atual de otimização do sistema, e uma possível necessidade de recursos computacionais adicionais, resolvemos realizar alguns testes de performance do sistema Aurora. Estes testes serão de suma importância para providenciar uma transição mais tranquila, além de garantir o sucesso das futuras implantações em ambiente produtivo, com os usuários finais do sistema.

Iremos começar com uma breve recapitulação dos módulos do sistema, com sua carga de trabalho e os possíveis gargalos. Em seguida, iremos descrever o protocolo de testes desenvolvido e, por fim, analisaremos os resultados observados.

## Sobre os módulos do sistema

O sistema é composto por três módulos: Frontend, Backend, e Indexador. Cada um deles está sendo provisionado em um ambiente de containeres individual, e se comunicam a partir de requisições HTTPS via a rede interna da Petrobras. No atual ambiente de desenvolvimento, todos eles possuem 1 núcleo de CPU e 2GB de memória RAM. Um diagrama da arquitetura pode ser visto a seguir:

![](Pasted%20image%2020250804141755.png)

### Módulo Frontend

O módulo Frontend é composto de uma aplicação Streamlit responsável por exibir os resultados da inferência em um mapa iterativo a partir da biblioteca Leaflet, com suporte a um sistema de feedbacks. Além disso, ele também é responsável por notificar o usuário caso haja detecção de novas feições.

![](Pasted%20image%2020250804143138.png)

A exibição gráfica deste módulo pode se tornar um possível gargalo ao ser necessário servir o mapa iterativo para diversos usuários simultâneos. Mesmo sendo computações simples, o uso da CPU pode aumentar consideravelmente nestes casos.
### Módulo Backend

O módulo Backend é responsável por fazer a ponte entre a aplicação web e o servidor de arquivos, a partir de uma biblioteca chamada TiTiler. Ele recebe requisições de imagens do Frontend e solicita ao Indexador pelo pedaço necessário, utilizando requisições HTTP especiais, chamadas "range". Com isso, este módulo é capaz de servir imagens georeferenciadas gigantescas com eficiência, ao servir apenas os pedaços necessários para a exibição atual no mapa iterativo. Com o passar do tempo, mesmo os diversos pedaços dessas imagens otimizadas podem acabar enchendo a memória RAM do sistema, já que ele se utiliza de tal memória para salvar os pedaços requisitados em uma espécie de memoria "cache", evitando assim sobrecargas ao servidor de arquivos.

![](Pasted%20image%2020250804144113.png)

### Módulo Indexador

O Indexador possui duas aplicações paralelas, o servidor de arquivos rangeHTTPServer e o daemon Watchdog. O servidor fornece as imagens requisitadas para o Backend, já o daemon está constantemente observando a pasta raiz do NAS afim de detectar novas imagens sendo salvas lá, para assim agendar uma nova inferência no cluster Atena02.

![](Pasted%20image%2020250804144426.png)

## O protocolo de testes

Com o intuito de medirmos a performance dos módulos com maior precisão, elaboramos um protocolo de testes composto por diversos etapas, de onde foram feitas medições. Todos os atores seguiram esse protocolo para garantirmos a validade dos resultados. Realizamos testes de forma individual, com apenas um usuário, e de forma simultânea, com três usuários concorrentes. Iniciamos com apenas uma imagem fixa, e depois expandimos para duas imagens aleatórias e ao mesmo tempo.

O principal recurso medido foi a memória RAM do backend, sendo que os outros recursos não demonstraram nenhuma aumento significativo. Em alguns casos, também foi observado o consumo de CPU tanto do módulo backend como frontend, pois ambos demonstraram um aumento significativo.

### Testes individuais

Para os testes individuais, um protocolo com mais granularidade foi utilizado:

1. Inicial - A imagem exibida no seu zoom padrão (7);
2. Zoom 14 - Escolher um ponto central da imagem e dar zoom até o nível 14;
3. Movimentação - No zoom 14, movimentar o mapa para os lados, aguardando o carregamento dos blocos;
4. Livre - Alterações de zoom e movimentações livres, simulando um caso de uso mais realista.

Durante os testes individuais, duas imagens diferentes foram selecionadas, e dentre cada uma dessas foi feito o *redeploy* do container backend, a fim de limpar sua memória cache. Todas as imagens utilizadas são imagens no formato `.tif`, MBI, pós-processadas pelo modelo de inferência em uma versão otimizada chamada de *MBI_light*.
### Testes Simultâneos

De acordo com os resultados dos testes individuais, para os testes simultâneos com três usuários concorrentes foi simplificado o protocolo de testes, visto que o zoom 14 e a movimentação não resultaram em aumentos expressivos na carga computacional. O novo protocolo foi definido como segue:

1. Inicial - A imagem exibida no seu zoom padrão (7);
2. Livre - Alterações de zoom e movimentações livres, simulando um caso de uso mais realista.
3. Reanálise - Selecionando outra imagem aleatória e repetindo o passo 2.

Por fim, um último teste foi realizado onde cada usuário selecionou duas imagens para serem exibidas de forma concomitantes.

## Resultados

A seguir vamos detalhar os resultados dos testes e discutir um pouco sobre o que eles significam.

### Testes individuais

#### Teste 1 

**Imagem selecionada**: CSKS1_GEC_B_HR_00_VV_RA_FF_20230314085543_20230314085612.MBI_light

| Teste        | RAM do Backend |
| ------------ | -------------- |
| Inicial      | 141 MB         |
| Zoom 14      | 180 MB         |
| Movimentação | 181 MB         |
| Livre        | 196 MB         |

#### Teste 2

**Imagem selecionada**:
CSKS1_GEC_B_HR_00_VV_RD_SF_20230614200423_20230614200451.MBI_light

| Teste        | RAM do Backend |
| ------------ | -------------- |
| Inicial      | 150 MB         |
| Zoom 14      | 191 MB         |
| Movimentação | 197 MB         |
| Livre        | 208 MB         |

### Testes simultâneos

#### Teste 1 - Imagem fixa

**Imagem selecionada**:
CSKS1_GEC_B_HR_00_VV_RA_FF_20230314085543_20230314085612.MBI_light

| Teste   | RAM do Backend |
| ------- | -------------- |
| Inicial | 190 MB         |
| Livre   | 252 MB         |

#### Teste 2 - Reanálise

Em seguida, sem reiniciar o container do backend, cada um dos usuários selecionou uma outra imagem aleatória para análise. Medimos tanto a memória RAM do Backend como também o consumo de CPU dele, e também do Frontend.

**Backend**: 310 MB de RAM, 0.30 de CPU
**Frontend**: *0.82 de CPU*

Por fim, depois de reiniciar o container do backend, cada um dos usuários selecionou duas imagens aleatórias para serem exibidas simultâneamente.

**Backend**: 352 MB de RAM, 0.40 de CPU
**Frontend**: *0.94 de CPU*

## Discussão

A partir dos testes realizados pudemos perceber algumas características dos módulos em execução. O Backend utiliza uma espécie de *cache* para guardar os blocos solicitados a fim de aliviar a carga do servidor de arquivos. Isso faz com que o consumo de memória RAM aumente de acordo com  o número de novas imagens sendo requisitadas. Felizmente, o sistema é robusto a ponto de conseguir servir mais de uma imagem ao mesmo tempo, com um aumento mínimo na carga de processamento. Por outro lado, o módulo do Frontend quase ocupou sua carga máxima de processamento, ao colocarmos três usuários para analisarem duas imagens ao mesmo tempo. 