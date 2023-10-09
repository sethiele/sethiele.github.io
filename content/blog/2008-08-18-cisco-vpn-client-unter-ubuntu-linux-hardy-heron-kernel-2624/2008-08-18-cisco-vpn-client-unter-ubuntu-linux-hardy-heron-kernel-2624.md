---
title: CISCO VPN Client unter Ubuntu Linux Hardy Heron Kernel 2.6.24
author: Sebastian
date: 2008-08-18T21:54:52+00:00
categories:
  - Linux/Unix
tags:
  - Anleitung
  - Linux/Unix
  - VPN
  - WiFi
  - Wlan
import: sebastian.thiele.me
---
Ich hatte da mal wieder das Problem, das ich den CISO Client (4.8.01.0640) nicht installieren konnte. Da half auch nicht die Anleitung der Ubuntu Wiki (http://wiki.ubuntuusers.de/Cisco-VPN-Client)
  
Folgender Fehler trat auf:

<pre>Making module
make -C /lib/modules/2.6.24-19-generic/build SUBDIRS=/home/sebastian/
Downloads/vpnclient modules
make[1]: Betrete Verzeichnis '/usr/src/linux-headers-2.6.24-19-generic'
  CC [M]  /home/sebastian/Downloads/vpnclient/linuxcniapi.o
In file included from /home/sebastian/Downloads/vpnclient/Cniapi.h:15,
                 from /home/sebastian/Downloads/vpnclient/linuxcniapi.c:31:
/home/sebastian/Downloads/vpnclient/GenDefs.h:113: Fehler: In Konflikt stehende
Typen für »uintptr_t«
include/linux/types.h:40: Fehler: Vorherige Deklaration von »uintptr_t«
war hier
make[2]: *** [/home/sebastian/Downloads/vpnclient/linuxcniapi.o] Fehler 1
make[1]: *** [_module_/home/sebastian/Downloads/vpnclient] Fehler 2
make[1]: Verlasse Verzeichnis '/usr/src/linux-headers-2.6.24-19-generic'
make: *** [default] Fehler 2
Failed to make module "cisco_ipsec.ko".</pre>

OK dann war da doch etwas bei der [Anleitung das half][1]:
  
Es muss ein Patch installiert werden

<pre>cd vpnclient
wget http://projects.tuxx-home.at/ciscovpn/patches/vpnclient-linux-2.6.22.diff
patch &lt; vpnclient-linux-2.6.22.diff
sudo ./vpn_install</pre>

Danach ging alles wie nach Anleitung. Evtl kann das ja noch jemandem helfen.

**Nachtrag (08.09.08)**

Da mitlwerweile eine neue Kernalversion raus ist muss natürlich auch ein anderer patch eingespielt werden. einfach mit _**uname -a**_ die Kernalversion aussuchen und dann unter http://projects.tuxx-home.at/ciscovpn/patches/ den richtigen Patch aussuchen. Das reicht.

**Nachtrag (09.10.08)**

Ich habe es mit in meinen neuen Blog übernommen, da es sicher für viele Interessant ist. Darunter diverse Erstsemester (unteranderem an der TFH-Wildau, Humbold Universität zu Berlin, Universität Hanover, und einige mehr) die jetzt neu in den Campusnetzen herumschwirren wollen..

 [1]: http://wiki.ubuntuusers.de/Cisco-VPN-Client#Installation-unter-Ubuntu-Hardy-Heron