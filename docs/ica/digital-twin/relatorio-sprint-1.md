# Servidor API REST

## Roteiro

- O que é o servidor
	- Objetivo
	- Características
	- Tecnologias
- API
	- Documentação
	- Rotas
	- Respostas
- FastAPI
- SQLModel
- PostgreSQL
- Próximos passos

## O que é o servidor

O objetivo deste servidor é fornecer uma camada de abstração capaz de gerenciar as requisições do cliente que deseja de forma simplificada realizar procedimentos de computação e treinamento de algoritmos de aprendizado de máquina em um provedor de recursos em núvem. A partir do fornecimento de informações como o conjuntos de dados, as arquiteturas de machine learning, os scripts de treinamento, e a plataforma ou cluster alvo, o sistema se encarregará da comunicação com o serviço de destino, fornecendo uma espécie de ponte entre o usuário e os mais variados serviços de cloud computing.

Para que isso seja possível, e buscando sempre a generalização, foi proposto um servidor API do tipo RESTful, capaz de receber requisições HTTP em um formato conhecido, padronizado e robustamente documentado. Nessa requisição será possível informar os caminhos para o consumo dos dados, o tipo de arquitetura do modelo, qual o destino do seu treinamento e qual o script de treinamento que deverá se executado pelo cluster. Além disso, serviços de autenticação poderão tratar com segurança o autor da requisição, como também identificá-lo para o serviço de destino, propiciando assim uma camada completa de segurança, com integridade, autenticidade e confidencialidade.

## Sobre a API

Uma API (do inglês Application Programming Interface) pode ser descrita como uma interface de comunicação entre um sistema ou serviço, e um cliente. Ela é responsável por abstrair a complexidade de um sistema e expôr os serviços disponíveis para aqueles que desejam utilizá-lo sem que sejam especialistas na tecnologia de sua fundação. Com isso, clientes são responsáveis em especificar o que deve ser feito, deixando para a API a tarefa de traduzir isso em procedimentos de baixo nível que definam como isso deve ser feito.

Seguindo as instruções de uma arquitetura REST (Representational State Transfer), o serviço proposto recebera requisições do tipo HTTP com métodos que definem o tipo de operação a ser realizada. Todas as informações necessárias, assim como as características esperadas e as respostas possíveis dessa operação estarão disponíveis para consulta em um endereço contendo a documentação da API. Lá será possível inclusive testar e manipular as rotas do serviço para entender exatamente o que será consumido pelos endpoints, facilitando assim a utilização do serviços por outros clientes.

## Documentação

Afim de fornecer a melhor experiência para os usuários e clientes que desenvolverão suas soluções consumindo a nossa API, é de suma importância que se mantenha atualizado e bem estruturado a sua documentação. Para isso, uma interface web para a exploração e consulta interativa da API será disponibilizada a partir de ferramentas geradoras de documentação, como a Swagger UI e a ReDoc. Essas ferramentas são baseadas em padrões popularmente estabelecidos, como a especificação OpenAPI e o vocabulário JSON schema, capazes de gerar de forma detalhada um ambiente intuitivo para consultas e experimentações.

Esses são os espaços onde normalmente é feito o primeiro contato entre a API e os agentes interessados, e será de suma importância ao expandirmos darmos manutenção ao serviço, propiciando a rápida integração de novos colaboradores no projeto. A fim de exemplo, apresentamos na figura X a documentação referente à primeira versão da API, e na figura Y demonstramos o modelo schema de um objeto da API.

## Fluxo de operação

O cliente poderá solicitar a criação de uma nova tarefa a ser realizada no cluster a partir de uma requisição HTTP do tipo POST, composta por um pacote de dados de formulário HTML contendo os dados e os arquivos necessários para a realização dessa tarefa. O servidor então irá preparar e encaminhar a tarefa para o destino correspondente retornando para o cliente um código de acesso que identifica a sua tarefa, salvando a mesma em um banco de dados interno para consulta e auditoria. Em seguida, o cliente pode solicitar uma requisição HTTP do tipo GET onde ele informará o código de acesso da sua tarefa junto com os dados de segurança do hand-shake previamente realizado, e assim que o sistema confirmar a autenticidade da requisição ele poderá consultar com o cluster o andamento do procedimento e responder para o cliente com o estado atualizado da sua tarefa. Para os casos em que o processamento já estiver finalizado, a resposta do sistema conterá o endpoint disponível para o consumo do modelo processado, finalizando assim o ciclo de vida daquela tarefa e da sua comunicação correspondente.

## Pilha de Tecnologias

Seguindo as tecnologias já abordadas pela equipe, foi decidido que uma primeira versão desse dispositivo será desenvolvida utilizando tecnologias de alto nível para as requisições da API, o armazenamento dos dados e a segurança das informações, e com isso abaixo será listado a pilha de tecnologias atualmente em desenvolvimento:

- FastAPI
- SQLModel
- Pydantic
- Starlette
- PostgreSQL
- Docker
- JWT Token
- Traefik

FastAPI é o framework Python para o desenvolvimento do serviço de API, é conhecido como um dos mais rápidos frameworks atualmente disponíveis, contendo uma intuitiva documentação e uma ampla comunidade que propiciam o eficiente desenvolvimento de soluções. SQLModel é uma biblioteca para interações com bancos de dados relacionais SQL a partir de códigos Python, e faz parte da mesma família de sistemas do FastAPI, sendo ambos baseados na biblioteca Pydantic e Starlette, sendo a primeira para validação de dados, documentação, tipificação e integração com outras ferramentas, e a segunda para a comunicação e processamento assíncrono. PostgreSQL é um sistema de banco de dados relacional de código aberto que utiliza e extende a linguagem SQL de forma segura e escalável, com uma vasta comunidade por trás e com abrangente suporte, facilitando assim a sua implantação em um ambiente conteinerizado a partir da ferramenta Docker, responsável por desacoplar a nossa solução do sistema operacional hospedeiro, isolando assim o sistema em um ambiente particular, seguro e leve. Por fim, a comunicação HTTP será garantida com segurança a partir de tecnologias de tokenização e proxy reverso disponíveis pelo padrão JSON Web Token (JWT) e a biblioteca Traefik.