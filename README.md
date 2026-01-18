# Instalação do Zabbix 7.0 no Ubuntu 24.04 (Noble)

Este repositório contém um passo a passo completo para instalação do Zabbix 7.0 LTS no Ubuntu 24.04, utilizando:

- MySQL como banco de dados
- Apache como servidor web
- Zabbix Server + Frontend + Agent

Inclui também prints das etapas de instalação e o arquivo com todos os comandos usados.

---

## Estrutura do Repositório

```
README.md  
comandos.txt  
zabbix/  
 ├── instalacao.png  
 ├── DBPassword.png  
 ├── versao-para-ubuntu.png  
 └── final.png
```

---

## 1. Atualização do Sistema

```bash
sudo apt update
sudo apt upgrade
sudo -s
```

---

## 2. Adicionando o Repositório Oficial do Zabbix

```bash
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_latest_7.0+ubuntu24.04_all.deb
dpkg -i zabbix-release_latest_7.0+ubuntu24.04_all.deb
apt update
```

---

## 3. Instalando os Pacotes do Zabbix + MySQL

```bash
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
apt install mysql-server -y
```

---

## 4. Configuração do Banco de Dados MySQL

Entrar no MySQL:

```bash
mysql -uroot -p
```

Criar o banco e o usuário:

```sql
create database zabbix character set utf8mb4 collate utf8mb4_bin;
create user zabbix@localhost identified by 'zabbix123';
grant all privileges on zabbix.* to zabbix@localhost;
set global log_bin_trust_function_creators = 1;
quit;
```

Importar o schema:

```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -uzabbix -p zabbix
```

Desativar permissões temporárias:

```bash
mysql -uroot -p
set global log_bin_trust_function_creators = 0;
quit;
```

---

## 5. Configurar o Zabbix Server

Editar:

```bash
nano /etc/zabbix/zabbix_server.conf
```

Localizar:

```
DBPassword=
```

Definir:

```
DBPassword=zabbix123
```

Imagem:

```
zabbix/DBPassword.png
```

---

## 6. Iniciando os Serviços

```bash
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

---

## 7. Acessando o Frontend

```
http://SEU_IP/zabbix
```

Exemplo:

```
http://10.0.1.5/zabbix
```

Tela de instalação:

```
zabbix/instalacao.png
```

Tela de escolha da plataforma:

```
zabbix/versao-para-ubuntu.png
```

---

## 8. Credenciais Padrão

```
Usuário: Admin
Senha: zabbix
```

---

## 9. Dashboard Final

```
zabbix/final.png
```
