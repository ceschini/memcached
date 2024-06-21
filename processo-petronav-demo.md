# Processo da tarefa de DB do big anchoring

1. Preparar queries pra coletar os dados de 1 mês.
2. Exportar os dados retornados.
3. Compartilhar os dados.
4. Criar o banco.
5. Criar as tabelas e usuários.
6. Inserir os dados.
7. Realizar testes usando o big anchoring.

# Como foi cada passo

1. Sem problemas, rapidamente consegui formular as queries necessárias para cada uma das tabelas.
2. Exportei como csv e também foi tranquilo.
3. A petrobras não deixa compartilhar nada pra fora da organização então tive que subir os arquivos no one drive, apenas exibir eles pelo one drive na minha máquina e depois selecionar todas as linhas e copiar/colar em uma nova planilha local.
4. Criar o banco exigiu tempo também pra descobrir como fazer isso dentro do docker, mas descobri um passo-a-passo e um repositório com scripts da oracle pra fazer isso.
5. Fui ver como inserir as tabelas e os dados a partir da biblioteca python e descobri que o csv não continha informações de datatypes nem formatação dos campos numéricos e de time_stamp. Fui na base novamente e exportei os dados como comandos `INSERT`, dessa forma cada linha se tornou um comando SQL capaz de ser chamado diretamente pelo banco. Tentei compartilhar via TEAMS ou salvar no one drive e exibir na minha máquina local porém sem acesso. Tive então que colar as linhas do arquivo em um site tipo pastebin e de lá copiar para minha máquina.
6. Consultei a base da petrobras novamente para pegar os schemas das tabelas a partir do `DESCRIBE`, depois foi necessário escrever as chamadas SQL de create table na mão usando os campos retornados pelo comando. Criei os usuários com a mesma nomenclatura e com as permissões padrões do script `create_user.sql` fornecido pelo repositório da python-oracledb.
7. Na hora de inserir os dados deu erro de validação devido às chaves primárias e estrangeiras que defini induzindo a partir do nome das linhas, pois o `DESCRIBE` não me retornou essa informação.
8. Recriei as tabelas sem informar chave primária e estrangeira e assim consegui inserir os dados.
9. Fui testar os endpoints e o /yaw/gps retornou "Less than two sensors in platform. Impossible to calculate yaw." Isso mostrou uma certa homogeneidade nos dados e a nossa necessidade de uma forma melhor de exportar e importar maiores volumes de dados (ou pelo menos replicar de forma heterogenia).

# Problemas

1. A petrobras não deixa compartilhar nada pra fora da organização então tive que subir os arquivos no one drive, apenas exibir eles pelo one drive na minha máquina e depois selecionar todas as linhas e copiar/colar em uma nova planilha local.
2. Fui ver como inserir as tabelas e os dados a partir da biblioteca `python-oracledb` e descobri que o csv não continha informações de datatypes nem formatação dos campos numéricos e de time_stamp. Fui na base novamente e exportei os dados como comandos `INSERT`, dessa forma cada linha se tornou um comando SQL capaz de ser chamado diretamente pelo banco. Tentei compartilhar via TEAMS ou salvar no one drive e exibir na minha máquina local porém sem acesso. Tive então que colar as linhas do arquivo em um site tipo pastebin e de lá copiar para minha máquina.
3.  Na hora de inserir os dados deu erro de validação devido às chaves primárias e estrangeiras que defini induzindo a partir do nome das linhas, pois o `DESCRIBE` não me retornou essa informação.
4. Recriei as tabelas sem informar chave primária e estrangeira e assim consegui inserir os dados.
5. Fui testar os endpoints e descobrimos alguns erros nas consultas devido à `TIME_STAMPS` atualizados, além de uma possível imcompatibilidade com versões de bibliotecas, mas isso já é para outras issues.
