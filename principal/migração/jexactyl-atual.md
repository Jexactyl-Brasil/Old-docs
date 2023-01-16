# Migrar Paineis da Jexactyl e Pterodactyl

!>Aviso dos Tradutores.

>Este painel Modifica Arquivos raizes do Jexactyl e do pterodactyl, é fortemente recomendado o uso de um `Database Novo` e Sem arquivos para melhor funcionamento.

***

Usando este guia, você irá migrar o painel Jexactyl(3.x,2x) e o Pterodactyl(1.x,0.7).

***

### Crie um Backup do seu painel Atual!

Diferente da migração do jexactyl/Pterodactyl na qual apenas adiciona novos arquivos, o Jexactyl-Brasil modifica tudo desde sua raiz e por isso não é recomendado usar os arquivos originais da Jexactyl/Pterodactyl, porém não se preocupe, este processo é simples e segue a mesma ideia de um backup.
Você pode fazer isso executando os seguintes comandos:

## Se seu painel for Jexactyl use

```bash
# renomear a estrutura original da jexactyl.
sudo mv /var/www/jexactyl /var/www/jexactyl-backup

# Despeje o banco de dados MySQL e salve-o no diretório de backup.
sudo mysqldump -u root -p panel > /var/www/jexactyl-backup/panel.sql

# Crie e entre na pasta onde novo diretório do jexactyl-brasil.
sudo mkdir /var/www/jexactyl
cd /var/www/jexactyl

# Copiar .env
sudo cp /var/www/jexactyl-backup/.env /var/www/jexactyl/
```
## Se seu painel for Pterodactyl use

```bash

# renomear a estrutura original do pterodactyl.
sudo mv /var/www/pterodactyl /var/www/pterodactyl-backup

# Despeje o banco de dados MySQL e salve-o no diretório de backup.
sudo mysqldump -u root -p panel > /var/www/pterodactyl-backup/panel.sql

# Crie e entre na pasta onde novo diretório do jexactyl-brasil.
sudo mkdir /var/www/pterodactyl
cd /var/www/pterodactyl

# Copiar .env 
sudo cp /var/www/pterodactyl-backup/.env /var/www/pterodactyl/
```

***

### Baixando o Novo painel Jexactyl-Brasil

Depois do renomear, criar o novo diretório e copiar o `.env`,Faremos o download dos arquivos Jexactyl-Brasil e sobrescrever os existentes.

```bash
# Baixe a versão mais recente do Jexactyl-Brasil usando CURL.
sudo curl -L -o panel.tar.gz https://github.com/Jexactyl-Brasil/Jexactyl-Brasil/releases/latest/download/panel.tar.gz

# Baixe os arquivos atualizados e exclua o arquivo compactado.
sudo tar -xzvf panel.tar.gz && rm -f panel.tar.gz
```

### Configurar Permissões

Em seguida, configure as permissões para que os arquivos do Painel possam ser acessados.

```bash
sudo chmod -R 755 storage/* bootstrap/cache
```

***

### Baixar as dependências do Composer

Após o download dos novos arquivos, você precisará atualizar as dependências do PHP Composer
que executam este Painel. Para fazer isso, use `composer` para atualizar os pacotes:

```bash
sudo composer install --no-dev --optimize-autoloader
```

***

### Atualizar migrações de banco de dados

Jexactyl-Brasil inclui novos recursos e funções que exigem que você migre para seu banco de dados.
Felizmente, este é um processo simples que envolve apenas a execução de um comando:

```bash
sudo php artisan migrate --seed --force
```

***

### Reatribuir permissões do servidor web

Devido à mudança de arquivos na máquina, precisaremos permitir que o Apache/NGINX os leia
novos arquivos. Você pode fazer isso executando o comando específico para o seu servidor web:

```bash
# Se estiver usando NGINX ou Apache (não no CentOS):
sudo chown -R www-data:www-data /var/www/jexactyl/*

# Se estiver usando NGINX no CentOS:
sudo chown -R nginx:nginx /var/www/jexactyl/*

# Se estiver usando o Apache no CentOS
sudo chown -R apache:apache /var/www/jexactyl/*
```

### Reiniciar os trabalhadores da fila(Pode não ser necessário)

Após cada atualização, você deve reiniciar o trabalhador da fila, para garantir que o novo código seja carregado e usado.

```bash
sudo php artisan queue:restart
```

***

### Marcar painel como online(Pode não ser necessário)

Agora que a migração foi concluída, você pode colocar o Painel novamente online e disponibilizá-lo aos usuários.

```bash
sudo php artisan up
```

?>
Parabéns! Você migrou para Jexactyl-Brasil e tudo deve estar funcionando normalmente.
Se você encontrar algum problema, informe-nos em nosso [Discord](https://discord.gg/8r7n7mU33R).
