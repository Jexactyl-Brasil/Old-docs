# Inicializador do Jexactyl

***

### Crontab
A primeira coisa que precisamos fazer é criar um novo cronjob que seja executado a cada minuto para processar tarefas específicas do Jexactyl, como limpeza de sessão e envio de tarefas agendadas para daemons.Usaremos `Nano` como editor.

Use o comando a baixo e digite 1 para selecionar o editor de texto nano.

```bash
sudo crontab -e

```

Em seguida, cole essa linha para adicionar o Cron de tarefas da jexactyl.

```bash
* * * * * php /var/www/jexactyl/artisan schedule:run >> /dev/null 2>&1
```

E por ultimo, cole essa linha para adicionar o Cron de Renovações do Jexactyl

```bash
0 0 * * * php /var/www/jexactyl/artisan p:schedule:renewal >> /dev/null 2>&1
```

***

### Systemd Queue Worker
Em seguida, você precisa criar um novo trabalhador systemd para manter nosso processo de fila em execução em segundo plano. Essa fila é responsável por enviar e-mails e lidar com muitas outras tarefas em segundo plano para o Jexactyl.

Agora criaremos um arquivo chamado `panel.service` na pasta /etc/systemd/system/.

```bash
sudo nano /etc/systemd/system/panel.service
```
Após isso, cole o texto abaixo dentro do arquivo que acabamos de criar.

```bash
# Jexactyl Queue Worker File
# ----------------------------------

[Unit]
Description=Jexactyl Queue Worker

[Service]
User=www-data
Group=www-data
Restart=always
ExecStart=/usr/bin/php /var/www/jexactyl/artisan queue:work --queue=high,standard,low --sleep=3 --tries=3
StartLimitInterval=180
StartLimitBurst=30
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

### Ativar Queue Worker
Por fim, ative o serviço do painel jexactyl que acabamos de criar, bem como o serviço redis, para iniciar e executar na inicialização.
```bash
sudo systemctl enable --now panel.service
sudo systemctl enable --now redis-server
```
