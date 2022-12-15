# Nginx sem configuração SSL

***

> A Jexactyl recomenda enfaticamente que você use SSL para proteger seu site.
Considere habilitar o SSL seguindo o guia [Configurando o SSL](docs/webservers/ssl-setup.md).

***

### Remova a configuração padrão

Em primeiro lugar, vamos remover a configuração NGINX padrão do seu servidor.
```bash
rm /etc/nginx/sites-available/default; rm /etc/nginx/sites-enabled/default
```

Feito isso, podemos fazer nossa configuração para o Jexactyl rodar.

***

### Criar arquivo de configuração

!> Certifique-se de substituir `<domain>` pelo seu próprio domínio neste arquivo de configuração.
Observe também que esta configuração é para NGINX com SSL ativado.
Se você deseja usar o Apache como servidor web ou não deseja usar SSL, consulte
às instruções do outro servidor web.

Faça um arquivo chamado `panel.conf` em `/etc/nginx/sites-available` e insira o seguinte:

```nginx
server {
    # Replace the example <domain> with your domain name or IP address
    listen 80;
    server_name <domain>;


    root /var/www/jexactyl/public;
    index index.html index.htm index.php;
    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    access_log off;
    error_log  /var/log/nginx/jexactyl.app-error.log error;

    # allow larger file uploads and longer script runtimes
    client_max_body_size 100m;
    client_body_timeout 120s;

    sendfile off;

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param PHP_VALUE "upload_max_filesize = 100M \n post_max_size=100M";
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param HTTP_PROXY "";
        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
        fastcgi_connect_timeout 300;
        fastcgi_send_timeout 300;
        fastcgi_read_timeout 300;
    }

    location ~ /\.ht {
        deny all;
    }
}

```

***

### Ativando a Configuração

Em primeiro lugar, vamos vincular o arquivo que criamos ao diretório que o NGINX usa para configurações.
```bash
ln -s /etc/nginx/sites-available/panel.conf /etc/nginx/sites-enabled/panel.conf
```

Em seguida, podemos testar nossa configuração nginx para garantir que esteja funcionando e seja válida:
```bash
nginx -t
```

Por fim, podemos reiniciar o processo do servidor NGINX para disponibilizar nosso Painel no domínio.
```bash
systemctl restart nginx
```

?>
Parabéns! Jexactyl está instalado e deve estar funcionando normalmente.
Se você encontrar algum problema, informe-nos em nosso [Discord](https://discord.com/invite/qttGR4Z5Pk).
