# Configuração do ambiente

***

### Criar arquivo de configuração
Precisamos começar copiando o arquivo de configurações padrão, chamado `.env.example`. vamos querer
isso será chamado `.env`, então é só copiar o arquivo para configurar o Painel
exatamente como você quer.

```bash
sudo cp .env.example .env
```

***

### Instalar pacotes do Composer
Depois disso, precisaremos instalar as dependências do Composer - o que permitirá o código PHP em nosso projeto
(como back-end e admin-side) para funcionar corretamente.

```bash
sudo composer install --no-dev --optimize-autoloader
```

Por fim, geraremos um token aleatório que será a chave de criptografia/aplicativo do nosso projeto.

!> Esta chave de criptografia é usada para armazenar dados importantes (como chaves de API).
NÃO compartilhe esta chave com ninguém - proteja-a como uma senha.
Se você perder esta chave, todos os dados serão impossíveis de recuperar.

```bash
sudo php artisan key:generate --force
```

***

### Configurar ambiente do painel
Jexactyl usa comandos CLI para definir a maioria das configurações básicas do Painel.
O seguinte permitirá que você defina configurações gerais, de banco de dados e de correio.

```bash
sudo php artisan p:environment:setup
sudo php artisan p:environment:database
sudo php artisan p:environment:mail # Não é necessário executar o Painel.
```

***

### Migração de banco de dados
Agora precisamos configurar todos os dados base para o Painel no banco de dados que você criou anteriormente. O comando abaixo pode levar algum tempo para ser executado dependendo da sua máquina. Por favor **NÃO saia do processo** até que esteja completo! Este comando configurará as tabelas do banco de dados e adicionará todos os Nests & Eggs que alimentam o Jexactyl.

```bash
sudo php artisan migrate --seed --force
```

***

### Criar usuário administrador
Para acessar o Painel pela primeira vez, você precisará criar um usuário via CLI - não se preocupe, você pode
crie usuários facilmente através do frontend! Aqui está um comando simples que o guiará pelas etapas de
configurando a conta de administrador inicial:
```bash
sudo php artisan p:user:make
```

***

### Atribuir permissões
Para que o servidor web que você está usando acesse os arquivos do Painel, precisamos atribuir permissões a ele
com o comando `chown`. Veja como fazer isso para todos os tipos de servidores da Web:

```bash
# Se estiver usando NGINX ou Apache (não no CentOS):
sudo chown -R www-data:www-data /var/www/jexactyl/*

# Se estiver usando NGINX no CentOS:
sudo chown -R nginx:nginx /var/www/jexactyl/*

# Se estiver usando o Apache no CentOS:
sudo chown -R apache:apache /var/www/jexactyl/*
```
