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

`sudo apt install git w3m tmux vim-nox`

## Instalar servidor impressão

Ler [aqui](https://pimylifeup.com/raspberry-pi-print-server/)

## Instalar servidor scanner

[Fonte](https://www.cnx-software.com/2017/02/19/how-to-use-chip-board-as-a-linux-printer-scanner-server/)

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
./configure LDFLAGS="-latomic"
make
make install
```
Se der erro, conferir https://github.com/meganz/MEGAcmd/issues/451 para solução

Para fazer backup da pasta pessoal a cada 4 horas, executar `mega-cmd` e executar:
```
backup /home/pi /pirate --period="0 0 */4 * * *" --num-backups=10
```
Quando sair, executar `mega-cmd-server &` para manter a execução do backup
