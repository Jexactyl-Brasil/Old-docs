# Instalar ferramentas de compilação

***

Este guia irá informá-lo sobre como começar a construir e modificar o Jexactyl.

***

### Instalando o NodeJS e o Yarn

Em primeiro lugar, precisaremos instalar o pacote 'NodeJS' e também adicionar o 'Yarn' para que possamos construir o front-end do Painel.

```bash
curl -sL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt install -y nodejs
```

Instale o 'Yarn' e as dependências necessárias para que o Jexactyl seja construído.

```bash
sudo npm i -g yarn
cd /var/www/jexactyl
sudo yarn # Installs building dependencies.
```

***

Em seguida, consulte nosso guia de [Build](principal/build/construindo.md) sobre como construir o frontend.
