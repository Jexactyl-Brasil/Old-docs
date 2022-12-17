# Aceitando pagamentos com a Stripe API

***

Este guia mostrará como começar facilmente a usar o Stripe com Jexactyl
para processar compras a crédito.

!> Este sistema foi implementado na `v3.2.0`. Verifique se você está executando esta versão
ou mais recente para usar o sistema Stripe.

***

### Obtenha o segredo do cliente e o segredo do webhook

Para processar pagamentos via Stripe, você precisará primeiro
crie uma conta e gere uma chave de API, bem como um segredo do webhook.

?> Registre uma conta no Stripe em https://stripe.com para começar.

***

### 1. Faça login no Painel Stripe

Uma vez logado e configurado, você deve estar em uma página que se parece com esta:

![image](../../public/images/stripe-dashboard.jpg)

### 2. Gere uma chave de API

Clique na guia `Desenvolvedores` à direita da tela. Em seguida, na barra lateral,
vá para 'chaves de API' e gere uma nova chave de API.

![image](../../public/images/stripe-apikey.jpg)

### 3. Criar Webhook

Depois de criar a chave de API, você precisará criar um `webhook` que irá
permitir que eventos Stripe sejam processados via Jexactyl. Vá para 'Webhooks' na barra lateral
e gerar um novo webhook.

![image](../../public/images/stripe-webhook.png)

No campo `Endpoint URL` digite: `https://<your.panel.tld>/stripe/listen`.

Em seguida, adicione os seguintes eventos:

![image](../../public/images/stripe-perms.jpg)

### 4. Copie as chaves geradas

Depois de fazer isso, copie o segredo do webhook e a chave da API,
para que possamos colocá-los no arquivo de configuração .env.

![image](../../public/images/stripe-webhook-secret.jpg)
![image](../../public/images/stripe-api-secret.jpg)

***

### 5. Adicionar ID e Segredo do Cliente ao Jexactyl
Em seguida, você precisará colocar essas chaves em seu arquivo `.env` para permitir o login do Jexactyl.

```bash
cd /var/www/jexactyl
nano .env

# Preencha os campos STRIPE_CLIENT_SECRET e STRIPE_WEBHOOK_SECRET
```

### 6. Ative o gateway Stripe nas configurações

?> Certifique-se de que a configuração 'Stripe' esteja definida como 'Habilitado'.

![image](../../public/images/store_admin.png)

### 7. Teste sua configuração

Vá até a Loja Jexactyl e clique na guia 'Carteira'. Quando estiver lá, tente comprar créditos `x` com o Stripe.
Se a página redirecionar para um portal de compras Stripe, parabéns! Você configurou e configurou com sucesso o Stripe.

?> Se você tiver problemas ao começar a usar o Stripe, informe-nos no nosso [Discord](discord.gg/8r7n7mU33R)
