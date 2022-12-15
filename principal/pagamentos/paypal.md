# Aceite pagamentos com PayPal

***

Este guia mostrará como começar facilmente a usar o PayPal com Jexactyl
para processar compras a crédito.

!> Este sistema foi implementado na `v3.1.0`. Verifique se você está executando esta versão
ou mais recente para usar o sistema PayPal.

***

### Obtenha o ID do cliente e o segredo do cliente

Você precisará primeiro criar um novo 'App' com o PayPal para obter um ID de cliente e um segredo
para uso com Jexactyl.

***

### 1. Faça login no console do desenvolvedor do PayPal
![image](https://www.knowband.com/blog/wp-content/uploads/2019/02/Paypal-login-PayPal-client-Id.png)
![image](https://www.knowband.com/blog/wp-content/uploads/2019/02/2.gif)

### 2. Vá para o painel e crie um novo aplicativo
!> Certifique-se de que a alternância na parte superior da página esteja definida para o modo AO VIVO, não Sandbox.

![image](https://www.knowband.com/blog/wp-content/uploads/2019/02/5.png)

### 3. Crie seu aplicativo do PayPal
![image](https://www.knowband.com/blog/wp-content/uploads/2019/02/6.png)

### 4. Obtenha o ID e o segredo do cliente
![image](https://www.knowband.com/blog/wp-content/uploads/2019/02/2021-04-21.gif)

***

### 5. Adicionar ID e Segredo do Cliente ao Jexactyl
Em seguida, você precisará colocar essas chaves em seu arquivo `.env` para permitir o login do Jexactyl.

```bash
cd /var/www/jexactyl
nano .env

# Preencha os campos PAYPAL_CLIENT_ID e PAYPAL_CLIENT_SECRET
```

### 6. Ative o gateway do PayPal nas configurações

?> Certifique-se de que a configuração 'PayPal' esteja definida como 'Habilitado'.

![image](../../public/images/store_admin.png)

### 7. Teste sua configuração

Vá até a Jexactyl Storefront e clique na guia 'Carteira'. Quando estiver lá, tente comprar créditos `x` com o PayPal.
Se a página redirecionar para um portal de compras do PayPal, parabéns! Você configurou e configurou com sucesso o PayPal.

?> Se você tiver problemas ao começar a usar o PayPal, informe-nos no [Jexactyl Discord](https://discord.com/invite/qttGR4Z5Pk)
