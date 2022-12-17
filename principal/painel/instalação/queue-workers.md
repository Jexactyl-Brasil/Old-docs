# trabalhadores da fila

***

### Crontab
A primeira coisa que precisamos fazer é criar um novo cronjob que seja executado a cada minuto para processar tarefas específicas do Jexactyl, como limpeza de sessão e envio de tarefas agendadas para daemons.

Você vai querer abrir seu crontab usando `sudo crontab -e` e então colar a linha abaixo. **Nano é o editor de texto mais fácil de usar, então pressione `1` quando solicitado para escolher um editor.**

```bash
* * * * * php /var/www/jexactyl/artisan schedule:run >> /dev/null 2>&1
```

***

### Systemd Queue Worker
Em seguida, você precisa criar um novo trabalhador systemd para manter nosso processo de fila em execução em segundo plano. Essa fila é responsável por enviar e-mails e lidar com muitas outras tarefas em segundo plano para o Jexactyl.

Crie um arquivo chamado `painel.service` em `/etc/systemd/system` com o conteúdo abaixo.

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
sudo systemctl enable --now painel.service
sudo systemctl enable --now redis-server
```
