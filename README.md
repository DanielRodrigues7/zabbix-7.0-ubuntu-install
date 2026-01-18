Instalação do Zabbix 7.0 no Ubuntu 24.04 (Noble)
Este repositório contém um passo a passo completo para instalação do Zabbix 7.0 LTS no Ubuntu 24.04, utilizando:

MySQL como banco de dados
Apache como servidor web
Zabbix Server + Frontend + Agent

Inclui também prints das etapas de instalação e o arquivo com todos os comandos usados.

📂 Estrutura do Repositório
├── README.md
├── comandos.txt
└── imagens/
    ├── instalacao.png
    ├── DBPassword.png
    ├── versao-para-ubuntu.png
    └── final.png


🚀 1. Atualização do Sistema
Shellsudo apt updatesudo apt upgradesudo -sMostrar mais linhas

📥 2. Adicionando o Repositório Oficial do Zabbix
Shellwget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.debdpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.debapt updateMostrar mais linhas

📦 3. Instalando os Pacotes do Zabbix + MySQL
Shellapt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agentapt install mysql-server -yMostrar mais linhas

🗄️ 4. Configuração do Banco de Dados MySQL
Entre no MySQL:
Shellmysql -uroot -pMostrar mais linhas
Comando usado no tutorial:
Senha do root mysql: zabbix123

Crie o banco e o usuário:
SQLcreate database zabbix character set utf8mb4 collate utf8mb4_bin;create user zabbix@localhost identified by 'zabbix123';grant all privileges on zabbix.* to zabbix@localhost;set global log_bin_trust_function_creators = 1;quit;Mostrar mais linhas
Importe o schema:
Shellzcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbixMostrar mais linhas
Desative o log especial:
Shellmysql -uroot -pset global log_bin_trust_function_creators = 0;quit;Mostrar mais linhas

⚙️ 5. Configurar o Zabbix Server
Edite o arquivo:
Shellvim /etc/zabbix/zabbix_server.confMostrar mais linhas
Ou usando nano:
Shellnano /etc/zabbix/zabbix_server.confMostrar mais linhas
Localize a linha:
DBPassword=

E defina:
DBPassword=zabbix123

📸 Print da edição do arquivo:
imagens/DBPassword.png

▶️ 6. Iniciando os Serviços
Shellsystemctl restart zabbix-server zabbix-agent apache2systemctl enable zabbix-server zabbix-agent apache2Mostrar mais linhas

🌐 7. Acessando o Frontend do Zabbix
Acesse no navegador:
http://SEU_IP/zabbix

Ou, no exemplo:
http://10.0.1.5/zabbix

📸 Tela inicial do setup:
imagens/instalacao.png
📸 Página de escolha da versão no site oficial:
imagens/versao-para-ubuntu.png

🔐 8. Credenciais Padrão
Login inicial:
Usuário: Admin
Senha: zabbix
