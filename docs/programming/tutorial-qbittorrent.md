# Como instalar e configurar o qBittorrent

Etse passo-a-passo irá demonstrar como instalar o cliente de Bittorrent gratuito chamado qBittorrent, além de como ativar sua poderosa funcionalidade de busca, que permite encontrar o melhor repositório para o arquivo que se deseja baixar, independente de onde ele esteja hospedado.

## Instalando pré-requisitos: Python

A funcionalidade de busca é desbloqueada em computadores onde o programa Python está instalado. Por isso, primeiro vamos instalá-lo em nossa máquina.

Vá para [o site oficial do Python](https://www.python.org) e clique em Downloads. Baixe o Python install manager e execute-o, a instalação é extremamente simples e pode ser feita com apenas um clique.

![download python](docs/img/python-download.png)

desmarque a opção "Iniciar quando pronto", e clique em "Instalar Python".

![instalando python](docs/img/install-python-manager.png)


## Instalando qBittorrent

Vá para  [o site oficial do qBittorrent](https://qbittorrent.org) e clique na aba Download, desca a página e clique no primeiro link abaixo do Windows 10 / 11, "Download qBittorrent v5.1.4" (a versão pode ter mudado desde a publicação do tutorial).

![download qbittorrent](docs/img/download-qbittorrent.png)

Na página do sourceforge que abrir, clique na última versão estável (de preferência a que não seja a beta).

![sourceforge qbittorrent](sourceforge-qbittorrent.png)

Em seguida, clique no primeiro link, o `qbittorrent_x.x.x_x64_setup.exe`, o download começará em seguida.

![sourceforge .exe qbittorrent](docs/img/qbittorrent-sourceforge-setup.png)

Abra o instalador e avance até a etapa dos componentes, sugiro marcar a opção "Criar Atalho no Desktop", mantenha as outras como estão.

![instalando qbittorrent, check opcoes](docs/img/qbittorrent-check-atalho.png)

## Configurando o motor de busca

Depois de instalado o programa, vamos habilitar e configurar o motor de busca para encontrar arquivos torrent nos diversos sites agregadores.

Procure a aba Visualizar no canto superior esquerdo e marque a opção "Motor de Busca". Aguarde alguns segundos enquanto o qBittorrent inicializa o módulo. 

![qbittorrent menu visualizar](docs/img/qbittorrent-visualizar-motor-busca.png)

Uma nova aba de Busca deve aparecer ao lado da de Transferências.

![qbittorrent aba de busca](docs/img/qbittorrent-aba-busca.png)

Na aba de Busca, clique no botão "Plugins de busca..." no canto inferior direito e dirija-se ao [site de plugins](https://plugins.qbittorrent.org).

Desça a página e navegue pela lista de motores de busca. Ao encontrar um que deseja incluir na ferramenta, clique com o botão direito no ícone de download e selecione "Copiar link".

![qbittorrent copiar link](docs/img/qbittorrent-copiar-link.png)

De volta no qBittorrent, clique em "Instalar um novo plugin", e depois em "Link da web", o link que foi copiado terá sido colado automaticamente na janela que abrir, depois disso basta clicar em ok.

![qbittorrent instalar motor busca](docs/img/qbittorrent-instalar-motor-busca.png)

Para efeitos de referência, segue minha lista pessoal de motores de busca. Infelizmente o processo é manual, e os motores devem ser adicionados um por um.

![qbittorrent lista de motores de busca](docs/img/qbittorrent-lista-de-plugins.png)

Depois de instalar os plugins desejados, basta pesquisar na barra de busca, e aguardar os registros aparecerem. Na tabela que abrir, clique para ordenar por Semeadores. Aquele com o maior número de semeadores normalmente é a melhor opção.

![qbittorrent ordenando seeders](docs/img/qbittorrent-seeders.png)
