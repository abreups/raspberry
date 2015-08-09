Instrucoes para configurar o Raspberry Pi:
==========================================

Para configurar o teclado ABNT:
------------------------------

Veja este post: https://www.raspberrypi.org/forums/viewtopic.php?t=15642&p=316382

Que diz:

editar o arquivo o /etc/default/keyboard para


    XKBMODEL="abnt2" 
    XKBLAYOUT="br"
    XKBVARIANT=""
    XKBOPTIONS="lv3:alt_switch,compose:rctrl"


Para configurar o Wi-Fi:
------------------------

Dentre as diversas opções recomendadas, tais como:
http://raspberrypihq.com/how-to-add-wifi-to-the-raspberry-pi/

a que melhor funcionou foi:
http://thepihut.com/blogs/raspberry-pi-tutorials/16018016-how-to-setup-wifi-on-the-raspberry-pi-raspbian

que basicamente manda editar o arquivo /etc/wpa_supplicant/wpa_supplicant.conf
e acrescentar:


    network={
        ssid="ssid da rede wi-fi"
        psk="senha do wi-fi"
    }

Funciona com Wi-Fi dongle LevelOne WUA-0603, com chipset Ralink.

Nota: se o seu endereço IP aparecer como 169.254.xxx.yyy é provável que
o seu Raspberry Pi não tenha conseguido um IP do DHCP server, e o IPv4LL
auto designou um IP nesse range. Veja https://apps.ubuntu.com/cat/applications/avahi-autoipd/


Para acessar a GUI via X-Server:
--------------------------------

Para acessar o RPi usando ssh faça: 

    ssh -X 192.168.0.xxx -l pi


Depois chame o X-server com: 

    startlxde

Você precisar ter um X-Server instalado no seu PC/Mac.

Adicionando uma câmera:
----------------------

    sudo apt-get install motion

Veja: http://pingbin.com/2012/12/raspberry-pi-web-cam-server-motion/


Compartilhar uma pasta:
----------------------

Para usar o Samba, veja este artigo:

http://www.simonthepiman.com/how_to_setup_windows_file_server.php

Para compartilhar a pasta onde as imagens do motion são salvas:

    [motion]
    comment = Pi camera
    path = /tmp/motion
    public = Yes
    browseable = Yes
