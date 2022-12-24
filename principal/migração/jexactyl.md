# Migrar do Jexactyl v2.x

!>Aviso dos Tradutores.

>Este painel Modifica Arquivos raizes do Jexactyl e do pterodactyl, é fortemente recomendado o uso de um database Novo e Sem arquivos para melhor funcionamento.

***

Usando este guia, você poderá atualizar para Jexactyl v3.x de v2.x.

***

### Faça backup do seu painel!

Embora esta migração seja projetada para ser o mais simples possível, recomendamos fortemente que você faça um backup
de todos os dados, apenas para garantir que nada dê errado durante a migração.
Você pode fazer isso executando os seguintes comandos:

```bash
# Faz backup da estrutura do arquivo e da chave .env.
cp -R /var/www/jexactyl /var/www/jexactyl-backup

# Despeje o banco de dados MySQL e salve-o no diretório de backup.
mysqldump -u root -p panel > /var/www/jexactyl-backup/panel.sql
```

***

### Marcar painel como indisponível

?> Certifique-se de estar no diretório `/var/www/jexactyl` antes de continuar.

Enquanto a migração ocorre, colocaremos o painel em um estado "indisponível" para que os usuários não possam
acessar a interface do usuário ou API. Podemos fazer isso executando o seguinte:

```bash
php artisan down
```

***

### Baixar Jexactyl

Depois que seu backup for concluído e o Painel estiver off-line, faremos o download dos arquivos Jexactyl
e sobrescrever os existentes.

```bash
# Baixe a versão mais recente do Jexactyl usando CURL.
curl -L -o panel.tar.gz https://github.com/Jexactyl-Brasil/Jexactyl-Brasil/releases/latest/download/panel.tar.gz

# Baixe os arquivos atualizados e exclua o arquivo compactado.
tar -xzvf panel.tar.gz && rm -f panel.tar.gz
```

Em seguida, configure as permissões para que os arquivos do Painel possam ser acessados.

```bash
chmod -R 755 storage/* bootstrap/cache
```

***

### Atualizar as dependências do Composer

Após o download dos novos arquivos, você precisará atualizar as dependências do PHP Composer
que executam este Painel. Para fazer isso, use `composer` para atualizar os pacotes:

```bash
# Correção temporária de erros.
composer require asbiin/laravel-webauthn

composer install --no-dev --optimize-autoloader
```

***

### Limpe o cache da interface do usuário compilado

Você também deseja limpar o cache do painel para que o novo site apareça corretamente.

```bash
php artisan optimize:clear
```

***

### Atualizar migrações de banco de dados

Jexactyl inclui novos recursos e funções que exigem que você migre para seu banco de dados.
Felizmente, este é um processo simples que envolve apenas a execução de um comando:

```bash
php artisan migrate --seed --force
```

***

### Reatribuir permissões do servidor web

Devido à mudança de arquivos na máquina, precisaremos permitir que o Apache/NGINX os leia
novos arquivos. Você pode fazer isso executando o comando específico para o seu servidor web:

```bash
# Se estiver usando NGINX ou Apache (não no CentOS):
chown -R www-data:www-data /var/www/jexactyl/*

# Se estiver usando NGINX no CentOS:
chown -R nginx:nginx /var/www/jexactyl/*

# Se estiver usando o Apache no CentOS
chown -R apache:apache /var/www/jexactyl/*
```

### Reiniciar os trabalhadores da fila

Após cada atualização, você deve reiniciar o trabalhador da fila, para garantir que o novo código seja carregado e usado.

```bash
php artisan queue:restart
```

***

### Marcar painel como online

Agora que a migração foi concluída, você pode colocar o Painel novamente online e disponibilizá-lo aos usuários.

```bash
php artisan up
```

?>
Parabéns! Você migrou para Jexactyl e tudo deve estar funcionando normalmente.
Se você encontrar algum problema, informe-nos em nosso [Discord](discord.gg/8r7n7mU33R).
