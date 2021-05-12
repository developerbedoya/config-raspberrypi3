# Slideshow de fotos usando feh

Maiores informações [aqui](https://www.instructables.com/Easy-Raspberry-Pi-Based-ScreensaverSlideshow-for-E/)

Criar pasta de fotos compartilhada:
```
mkdir ~/shared/photos
```
sudo apt install feh
```

Editar ~/.xsession:
```
xset s off
xset -dpms
xset s noblank
exec feh -Y -x -q -D 5 -B black -F -Z -z -r ~/shared/photos
```
