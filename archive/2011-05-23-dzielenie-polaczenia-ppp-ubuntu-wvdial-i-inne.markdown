---
title:  "Dzielenie połączenia PPP. Ubuntu, WvDial i inne"
date:   2011-03-18 10:24:01
description: "Dzielenie połączenia PPP. Ubuntu, WvDial i inne"
---

Tym razem podzielę się wrażeniami związanymi z dzieleniem połączenia internetowego uruchamianego przy pomocy WvDial. Jak go skonfigurować pisałęm już wcześniej. Chcielibyśmy mieć postawiony również serwer DHCP i żeby wszystko automatycznie było uruchamiane przy starcie systemu.

Chcielibyśmy aby WvDial był uruchamiany przy starcie komputera. Potrzebny jest nam do tego mały demonik. Plik powiedzmy nazwiemy go wvdial umieszczamy w katalogu /etc/init.d/. Powinien on mieć taką postać:

```bash
#!/bin/bash
### BEGIN INIT INFO# Provides:wvdial
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start daemon at boot time
# Description:       Enable service provided by daemon.
### END INIT INFO

PIDFILE=/var/run/ppp0.pid

set -e
case "$1" in
  start)
    if [ -f $PIDFILE ]; then
      echo "WvDial jest uruchomiony."
      sleep 5
      exit
    fi    echo "Uruchamianie WvDial"
    wvdial play
  ;;
  stop)
    if [ -f $PIDFILE ]; then
      echo "Zatrzymywanie WvDial."
      killall -9 pppd && killall -9 wvdial
      rm -f $PIDFILE
      exit
    fi
    echo "WvDial zostal uruchomiony."
    exit
  ;;
  restart)
    if [ -f $PIDFILE ]; then
      echo "Zatrzymywanie WvDial."
      killall -9 pppd && killall -9 wvdial
      rm -f $PIDFILE
      exit
    fi
    wvdial play
    echo "WvDial zostal uruchomiony."
    exit
    ;;
  *)
     echo "Usage: $0 {start|stop|restart}"
      exit 1
esac
```

Aby nasz demon został automatycznie uruchomiony przy starcie systemu musimy wykonać następującą komendę.

```bash
sudo update-rc.d wvdial defaults
```

Pamiętajm aby nadać mu prawa wykonywalności.

Teraz musumy sprawić aby wybrany przez nas interfejs sieciowy miał statyczny adres (dzięki temu możemy dzielić połączenie). W tym celu trzeba wyedytwać plik /etc/network/interfaces. Dodajemy do niego wpis w postaci

```bash
auto eth0
iface eth0 inet static
address 192.168.0.1
netmask 255.255.255.0
```

w przypadku wyboru innego interfejsu zamieniamy eth0 na to wybrane przez nas.

Teraz dodajemy odpowiedzni wpis dla serwera DHCP. Jeśli serwer nie jest zainstalowany instalujemy go najpierw

```bash
sudo aptitude install dhcp3-server
```

Potrzebny nam wpis umieszczam w pliku /etc/dhcp/dhcpd.conf. Powinien on mieć postać

```bash
subnet 192.168.0.0 netmask 255.255.255.0 {
  range 192.168.0.128 192.168.0.254;
  option subnet-mask 255.255.255.0;
  option broadcast-address 192.168.0.255;
  option routers 192.168.0.1;
  option domain-name "nazwa naszego localhosta";
  option domain-name-servers 212.85.112.37, 213.25.47.166 ;
  option netbios-name-servers 192.168.0.100;
}
```

I już prawie jesteśmy gotowi. Teraz potrzebny jest nam jeszcze skrypt uruchamiający maskowanie adresów id. Możemy umieścić go wo katalogu /etc/init.d/. Roboczo nazwiemy go masq, a będzie on wyglądał tak:

```bash
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -F -t natiptables -X -t natiptables -F -t filteriptables -X -t filter
iptables -t filter -P FORWARD DROP
iptables -t filter -A FORWARD -s 192.168.0.0/255.255.0.0 -d 0/0 -j ACCEPT
iptables -t filter -A FORWARD -s 0/0 -d 192.168.0.0/255.255.0.0 -j ACCEPT
iptables -t nat -A POSTROUTING -s 192.168.0.0/16 -d 0/0 -j MASQUERADE
```

Tak jak w przypdaku pliki wvdial musimy nadać mu prawa wykonywalności. Aby skrypt był wykonywany automatycznie po starcie stystemu zróbmy tak

```bash
sudo update-rc.d masq defaults
```

Teraz trzeba uruchomić wszystkie stworzone przez nas demony albo ewentualnie wykonać szybki restart. 