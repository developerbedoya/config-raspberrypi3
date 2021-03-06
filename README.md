# Configurando meu Raspberry Pi 3

Primeiro, instalar Raspbian, utilizando [Raspberry Pi Imager](https://www.raspberrypi.org/software/).

## Configuração ssh

Criar arquivo `ssh` na partição boot.

## Configuração Wi-Fi

Criar arquivo `wpa_supplicant.conf` na partição boot com o seguinte conteúdo

```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=BR

network={
    ssid="Nome rede"
    psk="Senha"
}
```

[Fonte](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md)

## Configurar sudoers

`sudo vi /etc/sudoers.d/010_pi-nopasswd`

Trocar
```
pi ALL=(ALL) NOPASSWD: ALL
```
por
```
pi ALL=(ALL) PASSWD: ALL
```
Salvar com `:w!` para forçar salvar arquivo somente leitura.

## Instalar pacotes base

`sudo apt install git w3m w3m-img tmux vim-nox`

## Instalar servidor impressão e scanner

Ler [aqui](https://www.cnx-software.com/2017/02/19/how-to-use-chip-board-as-a-linux-printer-scanner-server/)

### Resolver permissão scanner:
* Pegar arquivo `60-hp-deskjet-f4180.rules` do repositório e executar:
```bash
sudo cp 60-hp-deskjet-f4180.rules /lib/udev/rules.d/
sudo chown root:root /lib/udev/rules.d/60-hp-deskjet-f4180.rules
sudo chmod 644 /lib/udev/rules.d/60-hp-deskjet-f4180.rules
sudo usermod -aG scanner pi
```
* Reiniciar a máquina e configurar sane para acesso remoto
Os arquivos saned.socket e saned@.service estão no repositório

```bash
sudo cp saned.socket /etc/systemd/system/
sudo chown root:root /etc/systemd/system/saned.socket
sudo chmod 644 /etc/systemd/system/saned.socket

sudo cp saned@.service /etc/systemd/system/
sudo chown root:root /etc/systemd/system/saned@.service
sudo chmod 644 /etc/systemd/system/saned@.service
``` 

## Instalar servidor NFS
No caso, compartilhar a pasta /home/pi/share e instalar cliente NFS para Windows nos recursos do Windows.

[Fonte](https://pimylifeup.com/raspberry-pi-nfs/)

## Instalar Midnight Commander
```
sudo apt install mc
```

## Instalar genius (calculadora)
```
sudo apt install genius
```

## Instalar LCD35 (o 180 é opcional, é para virar o LCD 180º
```bash
git clone https://github.com/goodtft/LCD-show
chmod -R 755 LCD-show
cd LCD-show/
sudo ./LCD35-show 180
```

Depois de reiniciar:
```bash
sudo apt-get -f install
sudo apt install xinit xserver-xorg-video-fbturbo
```

Criar arquivo `/etc/X11/Xwrapper.config` e digitar:
```
allowed_users = anybody
```

Editar arquivo `/usr/share/X11/xorg.conf.d/99-fbturbo.conf` e trocar `fb0` por `fb1`



## Instalar suporte para mouse no console
```
sudo apt install gpm
```

## Instalar megacmd - para backups no mega.nz
```
sudo apt install libcrypto++ libpcrecpp0v5 libc-ares-dev zlib1g-dev libuv1 libssl-dev libsodium-dev readline-common sqlite3 curl autoconf libtool g++ libcrypto++-dev libz-dev libsqlite3-dev libssl-dev libcurl4-gnutls-dev libreadline-dev libpcre++-dev libsodium-dev libc-ares-dev libfreeimage-dev libavcodec-dev libavutil-dev libavformat-dev libswscale-dev libmediainfo-dev libzen-dev
git clone https://github.com/meganz/MEGAcmd
cd MEGAcmd && git submodule update --init --recursive
sh autogen.sh
LDFLAGS="-latomic" ./configure
make
sudo make install
sudo ldconfig
```
Se der erro, conferir https://github.com/meganz/MEGAcmd/issues/451 para solução

### Criar e habilitar serviço mega-cmd-server
Primeiro tem que logar no mega-cmd pelo menos uma vez para armazenamento de credenciais.
```bash
sudo cp mega-cmd-server.service /etc/systemd/system/
sudo chown root:root /etc/systemd/system/mega-cmd-server.service
sudo systemctl enable mega-cmd-server
sudo systemctl start mega-cmd-server
```

Para fazer backup da pasta pessoal a cada 4 horas, executar `mega-cmd` e executar:
```
backup /home/pi /pirate --period="0 0 */4 * * *" --num-backups=10
```
Quando sair, executar `mega-cmd-server &` para manter a execução do backup
