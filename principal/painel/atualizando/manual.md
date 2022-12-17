# Atualizando Jexactyl

***

A atualização do Jexactyl mantém seu sistema estável, seguro e permite
seus usuários/administradores experimentem novos recursos mais rapidamente. Usar
o seguinte guia abaixo como referência para atualizar o Jexactyl.

?>
Faça um backup de sua instalação antes de continuar.

***

### Modo de manutenção

Comece desligando o Painel enquanto realizamos as atualizações.

```bash
cd /var/www/jexactyl # Substitua 'jexactyl' por 'pterodactyl' se você migrou
php artisan down
```

### Baixe a nova versão

Em seguida, usaremos cURL para baixar o arquivo de lançamento do GitHub
e extraia-o.

```bash
curl -L https://github.com/Ashu11-A/Jexactyl_PT-BR/releases/latest/download/panel.tar.gz | tar -xzv
chmod -R 755 storage/* bootstrap/cache # Defina as permissões do servidor corretamente
```

### Atualizar pacotes do Composer

Devido ao Jexactyl se manter atualizado usando os pacotes mais recentes, você
precisará atualizar as dependências do Composer que permitem que o Jexactyl
para funcionar corretamente em sua máquina.

```bash
composer install --no-dev --optimize-autoloader
```

### Sincronizar alterações no banco de dados

Você precisará migrar as novas informações do banco de dados para o seu
banco de dados para usar os recursos mais recentes do Jexactyl.

```bash
php artisan migrate --seed --force
```

### Definir permissões do servidor web

Depois de alterar os arquivos, devemos permitir novamente as permissões para nossos
webserver para que o Jexactyl possa ser hospedado e acessado adequadamente.

```bash
cd /var/www/jexactyl

# EXECUTE APENAS UM DOS SEGUINTES COMANDOS!

# Se estiver usando NGINX ou Apache (não no CentOS):
chown -R www-data:www-data *

# Se estiver usando NGINX no CentOS:
chown -R nginx:nginx *

# Se estiver usando o Apache no CentOS
chown -R apache:apache *
```

### Finalizar atualização

Como etapa final, reinicie o trabalhador da fila e traga o painel
novamente on-line para que os usuários possam experimentar o que há de mais recente.

```bash
sudo systemctl restart panel.service # Replace 'panel' with 'pteroq' if you have migrated
php artisan up
```

?> Algum problema? Entre em contato conosco em [Discord](discord.gg/8r7n7mU33R).

