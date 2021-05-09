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

`sudo apt install git w3m tmux`

