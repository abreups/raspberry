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
