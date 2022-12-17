# Configuração do Sistema de Renovação

***

Adicione essa linha ao crontab do seu sistema para executar o script de renovação diariamente.

```bash
crontab -e # Pick '1' if prompted
```
Em seguida, cole esta linha e saia depois:

```bash
0 0 * * * php /var/www/jexactyl/artisan p:schedule:renewal >> /dev/null 2>&1
```

?>
Parabéns! As renovações foram configuradas e devem estar funcionando normalmente.
Se você encontrar algum problema, por favor, deixe-nos saber em nosso [Discord](discord.gg/8r7n7mU33R).
