# Instalar dependências
Você precisará instalar as seguintes dependências para executar e usar o Jexactyl:



PHP `8.1` com as extensões:(PHP 8.2 Ainda não tem suporte.)
- `cli`
- `openssl`
- `gd`
- `mysql`
- `PDO`
- `mbstring`
- `tokenizer`
- `bcmath`
- `xml`
- `curl`
- `zip`
- `fpm`.

MariaDB `10.2` ou superior, com `redis-server`.

Um servidor web ('NGINX' é preferido.)

`curl`, `tar`, `unzip`, `git` and `composer` v2.

## Exemplo de instalação de dependência

!> Seu sistema operacional pode ser diferente do que usamos para esta instalação.
Certifique-se de que esses comandos funcionem para você e, se não funcionarem, consulte
o gerenciador de pacotes do seu sistema operacional para saber como instalar as dependências.

```bash
sudo apt -y install software-properties-common curl apt-transport-https ca-certificates gnupg

sudo LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php
sudo add-apt-repository ppa:redislabs/redis -y

# O comando abaixo não é necessário se você estiver usando o Ubuntu 22.04 ou superior.
curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash

sudo apt update
sudo apt -y install php8.1 php8.1-{cli,gd,mysql,pdo,mbstring,tokenizer,bcmath,xml,fpm,curl,zip} mariadb-server nginx tar unzip git redis-server nano
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```
