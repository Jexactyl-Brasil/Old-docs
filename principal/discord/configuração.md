# Configurar Discord 0Auth para Jexactyl

***

Este guia mostrará como vincular um aplicativo de autenticação do Discord ao Jexactyl, o que permite
os usuários se autentiquem com o Jexactyl por meio de sua conta do Discord.

***

### Configurar um aplicativo Discord

Em primeiro lugar, você precisará criar um aplicativo Discord para obter um ID de cliente e um segredo de cliente.
Você pode fazer isso acessando o [Discord Developer Portal](https://discord.com/developers)
e clicando no botão 'Novo aplicativo'. Dê um nome de sua escolha (isso ficará visível para
clientes) e clique em 'Criar'.

### Obtenha o ID e o segredo do cliente

Em seguida, precisamos pegar duas coisas: o ID do nosso aplicativo e também seu segredo para manter o aplicativo seguro.
![Discord ID image](../../public/images/discord_id.png)
![Discord ID image 2](../../public/images/discord_id_2.png)

### Configurar URLs de redirecionamento

Agora você precisará configurar os redirecionamentos para que o Discord saiba para onde apontar seus usuários após a autenticação.
Você pode fazer isso clicando no botão 'Adicionar redirecionamento' e adicionando esses dois URLs.
![Discord Redirect image](../../public/images/discord_redirect.png)

### Adicionar ID e Segredo do Cliente ao Jexactyl

Por fim, vá para a página de configurações de 'Registro do usuário' do Jexactyl e preencha seu ID e segredo do cliente
para Discórdia. Certifique-se de ativar o módulo de registro, caso contrário, os usuários não poderão se autenticar!
![Enable Jexactyl image](../../public/images/discord_jexactyl.png)

### Teste seu Aplicativo

Experimente e tente fazer login via Discord. Se você encontrar um erro como `invalid_redirect_uri`, consulte
etapa 3 novamente e certifique-se de que suas configurações estejam 100% corretas e válidas.

?>
parabéns! O Jexactyl Discord Auth deve estar funcionando normalmente.
Se você encontrar algum problema, informe-nos em nosso [Discord](https://discord.gg/8r7n7mU33R).
