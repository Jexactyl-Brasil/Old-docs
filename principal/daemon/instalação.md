## Instalação do Daemon

!> Aviso dos tradutores.

- Este método de instalação foi tirado da documentação oficial do pterodactyl, para permitir os usuários terem a tradução completa tanto do painel, quanto das wings, caso queira usar a documentação oficial e original(está em Ingles) click [Aqui](https://pterodactyl.io/wings/1.0/installing.html).

***

# Instalando o Wings

Wings é o daemon de controle de servidor de próxima geração da Pterodactyl. Foi reconstruído a partir do
Ground Up usando o Go e as lições aprendidas com o nosso primeiro Nodejs Daemon.

!>Aviso:Você só deve instalar o Wings se estiver executando **Pterodactyl 1.x**. Não instale este software
para versões anteriores de Pterodactyl.


## Sistemas suportados

A seguir está uma lista de sistemas operacionais suportados. Por favor, esteja ciente de que esta não é uma lista exaustiva,
há uma alta probabilidade de que você pode executar o software em outras distribuições Linux sem muito esforço.
Você é responsável por determinar quais pacotes podem ser necessários nesses sistemas. Há também um muito
alta probabilidade de que as novas versões dos sistemas operacionais suportados abaixo funcionem muito bem, você não está restrito a
apenas as versões listadas abaixo.

| Operating System | Version |     Supported      | Notes                                                                          |
|------------------|---------|:------------------:|--------------------------------------------------------------------------------|
| **Ubuntu**       | 18.04   | :white_check_mark: | Documentação escrita assumindo o Ubuntu 18.04 como o sistema operacional base. |
|                  | 20.04   | :white_check_mark: |                                                                                |
|                  | 22.04   | :white_check_mark: |                                                                                |
| **CentOS**       | 7       | :white_check_mark: |                                                                                |
|                  | 8       | :white_check_mark: | Observe que o CentOS 8 é EOL. Use Rocky ou Alma Linux.                         |
| **Debian**       | 10      | :white_check_mark: |                                                                                |
|                  | 11      | :white_check_mark: |                                                                                |
| **Windows**      | All     |        :x:         | Este software não será executado em ambientes Windows.                         |

## Requisitos do Sistema

Para executar o Wings, você precisará de um sistema Linux capaz de executar contêineres do Docker. A maioria dos VPS e quase todos
servidores dedicados devem ser capazes de executar o Docker, mas há casos de borda.

Quando seu provedor usa a virtualização `Virtuozzo`, `OpenVZ` (ou `OVZ`) ou `LXC`, você provavelmente não poderá
corra Asas. Alguns provedores fizeram as alterações necessárias para a virtualização aninhada para oferecer suporte ao Docker. Peça à equipe de suporte do seu provedor para se certificar. KVM é garantido para trabalhar.

A maneira mais fácil de verificar é digitar `systemd-detect-virt`.
Se o resultado não contiver `OpenVZ` ou `LXC`, tudo bem. O resultado de `nenhum` aparecerá ao executar hardware dedicado sem qualquer virtualização.

Se isso não funcionar por algum motivo, ou você ainda não tiver certeza, você também pode executar o comando abaixo.

```bash
sudo dmidecode -s system-manufacturer
VMware, Inc.
```

## Dependências

- curl
- Docker

### Instalando o Docker

Para uma instalação rápida do Docker CE, você pode executar o comando abaixo:

```bash
curl -sSL https://get.docker.com/ | CHANNEL=stable bash
```

Se você preferir fazer uma instalação manual, consulte a documentação oficial do Docker para saber como instalar o Docker CE em seu servidor. Alguns links rápidos
estão listados abaixo para sistemas comumente suportados.

- [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce)
- [CentOS](https://docs.docker.com/install/linux/docker-ce/centos/#install-docker-ce)
- [Debian](https://docs.docker.com/install/linux/docker-ce/debian/#install-docker-ce)

!>Aviso:Verifique seu Kernel
Lembre-se de que alguns hosts instalam um kernel modificado que não oferece suporte a recursos importantes do docker. Por favor
verifique seu kernel executando `uname -r`. Se o seu kernel termina em `-xxxx-grs-ipv6-64` ou `-xxxx-mod-std-ipv6-64` você está
provavelmente usando um kernel não suportado. Verifique nossas [Modificações do Kernel](principal/daemon/kernel-Modificado.md) guia para mais detalhes.

#### Iniciar o Docker na inicialização

Se você estiver em um sistema operacional com systemd (Ubuntu 16+, Debian 8+, CentOS 7+) execute o comando abaixo para que o Docker inicie quando você inicializar sua máquina.

```bash
systemctl enable --now docker
```

#### Habilitando Swap

Na maioria dos sistemas, o Docker não poderá configurar o espaço de permuta por padrão. Você pode confirmar isso executando `docker info` e procurando a saída de `AVISO: Sem suporte a limite de swap` perto da parte inferior.

Habilitar a troca é totalmente opcional, mas recomendamos fazê-lo se você estiver hospedando para outras pessoas e para evitar erros de OOM.

Para habilitar o Swap, abra `/etc/default/grub` como um usuário root e encontre a linha começando com `GRUB_CMDLINE_LINUX_DEFAULT`. Fazer
certeza de que a linha inclui `swapaccount=1` em algum lugar dentro das aspas duplas.

Depois disso, execute `sudo update-grub` seguido de `sudo reboot` para reiniciar o servidor e ter o swap habilitada.
Abaixo está um exemplo de como a linha deve ser, _do não copie essa linha textualmente. Muitas vezes, ele tem parameters._ adicionais específicos do sistema operacional

```text
GRUB_CMDLINE_LINUX_DEFAULT="swapaccount=1"
```

?>Dica:Configuração do GRUB,algumas distribuições Linux podem ignorar `GRUB_CMDLINE_LINUX_DEFAULT`. Portanto, você pode ter que usar "GRUB_CMDLINE_LINUX" em vez disso, caso o padrão não funcione para você.

## Instalando o Wings

O primeiro passo para instalar o Wings é garantir que tenhamos a configuração de estrutura de diretórios necessária. Para isso,
execute os comandos abaixo, que criarão o diretório base e baixarão o executável wings.

```bash
mkdir -p /etc/pterodactyl
curl -L -o /usr/local/bin/wings "https://github.com/pterodactyl/wings/releases/latest/download/wings_linux_$([[ "$(uname -m)" == "x86_64" ]] && echo "amd64" || echo "arm64")"
chmod u+x /usr/local/bin/wings
```

!>Aviso:Servidores OVH/SYS.Se estiver a utilizar um servidor fornecido pela OVH ou SoYouStart, esteja ciente de que o seu espaço em disco principal está provavelmente atribuído a
`/home`, e não `/` por padrão. Por favor, considere o uso de `/home/daemon-data` para dados do servidor. Isso pode ser facilmente
ao criar o node.


## Configurar

Depois de instalar o Wings e os componentes necessários, o próximo passo é criar um nó no painel instalado. Vá para a visualização administrativa do Painel, selecione Node na barra lateral e, no lado direito, clique no botão Criar Novo.

Depois de criar um nodes, clique nele e haverá uma guia chamada Configuração. Copie o conteúdo do bloco de código, cole-o em um novo arquivo chamado `config.yml` em `/etc/pterodactyl` e salve-o.

Alternativamente, você pode clicar no botão Gerar Token, copiar o comando bash e colá-lo em seu terminal(em alguns casos se isso não funcionar use o metodo normal).

![imagem de exemplo de configuração de Wings](./../../.vuepress/public/wings_configuration_example.png)

!>Aviso:Quando seu Painel estiver usando SSL, as Wings também devem ter uma criada para seu FQDN. Consulte a página de documentação [Configurar SSL](principal/painel/servidores-web/setup-ssl) para saber como criar esses certificados antes de continuar.

### Iniciando Wings

Para iniciar o Wings, basta executar o comando abaixo, que o iniciará em um modo de depuração. Depois de confirmar que ele está sendo executado sem erros, use `CTRL + C` para encerrar o processo e daemonizá-lo seguindo as instruções abaixo. Dependendo da conexão de internet do seu servidor, puxar e iniciar o Wings pela primeira vez pode levar alguns minutos.

```bash
sudo wings --debug
```

Opcionalmente, você pode adicionar o sinalizador `--debug` para executar o Wings no modo de depuração.

### Daemonização (usando systemd)

Executar Wings em segundo plano é uma tarefa simples, apenas certifique-se de que ele é executado sem erros antes de fazer
este. Coloque o conteúdo abaixo em um arquivo chamado `wings.service` no diretório `/etc/systemd/system`.

```text
[Unit]
Description=Pterodactyl Wings Daemon
After=docker.service
Requires=docker.service
PartOf=docker.service

[Service]
User=root
WorkingDirectory=/etc/pterodactyl
LimitNOFILE=4096
PIDFile=/var/run/wings/daemon.pid
ExecStart=/usr/local/bin/wings
Restart=on-failure
StartLimitInterval=180
StartLimitBurst=30
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

Em seguida, execute os comandos abaixo para recarregar systemd e iniciar o Wings.

```bash
systemctl enable --now wings
```

### Alocações de Nodes

A alocação é uma combinação de IP e Porta que você pode atribuir a um servidor. Cada servidor criado deve ter pelo menos uma alocação. A alocação seria o endereço IP da sua interface de rede. Em alguns casos, como quando atrás do NAT, seria o IP interno. Para criar novas alocações, vá para Nodes > seu node > Alocação.

![imagem de exemplo de alocações de node](../../.vuepress/public/node_allocations.png)

Digite `hostname -I | awk '{print $1}'` para encontrar o IP a ser usado para a alocação. Alternativamente, você pode digitar `ip addr | grep "inet "` para ver todas as suas interfaces disponíveis e endereços IP. Não use 127.0.0.1 para alocações.
