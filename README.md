Instruções para configurar o Raspberry Pi:
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

Para acertar o relógio:
----------------------

Veja a data e a hora com:

    date
    
Se precisar de ajuste entre no raspi-config

    sudo raspi-config
    
e vá na opção "4 Internationalisation Options". Siga os menus.

O relógio do Raspberry Pi é sincronizado pelo serviço ntp.

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

Para colocar um IP estático (fixo) veja:

http://www.suntimebox.com/raspberry-pi-tutorial-course/week-3/day-5/

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

Para você usar o motion como time-lapse camera veja o parâmetro snapshot_interval:

http://chris.gg/2012/07/using-a-ps3-eyetoy-with-the-raspberry-pi/4/

e

http://lavrsen.dk/foswiki/bin/view/Motion/FrequentlyAskedQuestions#How_do_I_disable_or_enable_saving_jpeg_files_when_motion_is_detected_63

No meu caso fiz (/etc/motion/motion.conf):

    snapshot_interval 60
    output_normal off
    ffmpeg_cap_new off
    
e aí fico só com os snapshots (sem filminho e sem detecção de movimento).

Compartilhar uma pasta:
----------------------

Para usar o Samba, veja este artigo:

http://www.simonthepiman.com/how_to_setup_windows_file_server.php

Para compartilhar a pasta onde as imagens do motion são salvas (/etc/samba/smb.conf):

    [motion]
    comment = Pi camera
    path = /tmp/motion
    public = Yes
    browseable = Yes

Byobu: sessão "permanente" de terminal
-------------------------------------

Veja: http://www.howtogeek.com/58487/how-to-easily-multitask-in-a-linux-terminal-with-byobu/

    sudo apt-get install screen byobu

Conectando um pen-drive:
------------------------

Regra geral:

1. Encaixe o pen-drive numa porta usb

2. Veja em qual device ele foi alocado (provavelmente no sda1):

        sudo ls /dev/sd*

3. Crie um local para "montar" o drive:

        sudo mkdir /mnt/usb
    
4. "Monte" o drive:

        sudo mount -t vfat /dev/sda1 /mnt/usb
    
5. Veja o que tem no pen-drive:

        ls /mnt/usb
    
Algumas referências:

a. http://www.makeuseof.com/tag/how-to-add-usb-storage-to-the-raspberry-pi/

b. http://raspi.tv/2012/how-to-mount-and-use-a-usb-hard-disk-with-the-raspberry-pi

c. http://www.raspberrypi-spy.co.uk/2014/05/how-to-mount-a-usb-flash-disk-on-the-raspberry-pi/

Time lapse camera (funcionou por um tempo mas depois preferi usar o próprio motion):
-----------------

Veja: https://www.raspberrypi.org/learning/webcam-timelapse-setup/worksheet.md

Para o nome dos arquivos eu usei o formato "ano mes dia hota minuto segundo", ou seja:

    %Y%m%d%%H%M%S

E para converter os arquivos de imagem em um vídeo use o ffmpeg:

    ./ffmpeg -framerate 3 -pattern_type glob -i "2015*.jpg" -c:v libx264 -r 30 -pix_fmt yuv420p out.mp4
    
Os arquivos de imagem têm nomes segundo o padrão "2015*.jpg"

Referência: http://superuser.com/questions/624567/ffmpeg-create-a-video-from-images


