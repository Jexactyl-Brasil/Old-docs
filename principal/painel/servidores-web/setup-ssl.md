# Configurar SSL com Certbot

***

### Baixe o pacote Certbot

Começaremos baixando o pacote `certbot` que pode ser usado para criar certificados SSL
para o seu site.
```bash
apt install -y certbot
```

***

### Criando um certificado

Supondo que você tenha definido seu domínio para apontar para o IP do seu servidor web, você está pronto para criar um certificado.
Criar um certificado SSL é tão simples quanto executar um dos comandos abaixo:

```bash
# Se você estiver usando NGINX:
certbot certonly --nginx -d example.com

# Se você estiver usando o Apache:
certbot certonly --apache -d example.com

# Use isso se nenhum dos dois funcionar. Certifique-se de parar seu servidor web primeiro ao usar este método.
certbot certonly --standalone -d example.com
```
