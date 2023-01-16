# Download de arquivos

***

### Criar diretório

O primeiro passo neste processo é criar a pasta onde o
panel será executado e, em seguida, nos moveremos para a pasta recém-criada.
Abaixo está um exemplo de como realizar esta operação.

```bash
sudo mkdir -p /var/www/jexactyl
cd /var/www/jexactyl
```

***

### Baixar o Painel

Depois de entrar neste diretório, você pode `curl` (baixar) a versão mais recente para sua máquina.
Então, você pode extraí-lo usando o comando `tar` e atribuir permissões usando `chmod`. Atribuímos permissões
para os diretórios `storage/*` e `bootstrap/cache` para permitir que o site armazene objetos em cache e carregue mais rápido.

```bash
sudo curl -Lo panel.tar.gz https://github.com/Jexactyl-Brasil/Jexactyl-Brasil/releases/latest/download/panel.tar.gz
sudo tar -xzvf panel.tar.gz
sudo chmod -R 755 storage/* bootstrap/cache/
```
