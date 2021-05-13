# Receber arquivos pelo bluetooth
Se o LCD35 estiver instalado, tem que adicionar umas cláusula no /boot/config.txt:
```
core_freq=250
dtoverlay=miniuart-bt
```

Depois adicionar o usuário pi no grupo bluetooth e reiniciar:
```bash
sudo usermod -aG bluetooth pi
sudo reboot
```

Depois conectar o Windows no raspberry pi, executar bluetoothctl
```
discoverable on
agent on
default-agent
```

Sem sair do bluetoothctl, conectar o aparelho ao raspberry pi, depois:
```
trust XX:XX:XX:XX:XX:XX
discoverable off
exit
```

Instalar e executar o obexpushd

```bash
sudo apt install bluetooth obexpushd
sudo mkdir /home/pi/bluetooth
sudo cp obexpush.service /etc/systemd/system/
sudo systemctl enable obexpush
sudo systemctl start obexpush.service
```

