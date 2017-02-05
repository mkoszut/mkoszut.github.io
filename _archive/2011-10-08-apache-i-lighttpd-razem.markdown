---
title:  "Apache i lighttpd razem"
date:   2011-10-08 11:54:19
description: "Apache i lighttpd razem"
---

Przyszedł taki moment kiedy musiałem mieć serwer apachea i lightiego na jednym kompie. Pierwsze co mi przyszło do głowy to postawienie proxy na apacheu i redirektowanie na port 8080, dla odpowiednich domen, na którym słuchał już lighttpd. Jak pomyślałem tak zrobiłem. Po paru minutach miałem już działające oba serwery www. Jednak jak się okazało nie wszystko działa tak pięknie jak ja bym tego chciał. PHP widziało $_SERVER['HTTP_HOST'] jako www.domena.pl:8080, a to nie było mi na rękę. Potrzebowałem, żeby PHP widziało domena.pl jako domena.pl bez oznaczenia portu. Pomyślałem postawie na odwrót, najpierw lightiego potem apachea.

Jednak dowiedziałem się, że można to zrobić inaczej. Przecież serwery nie muszą słuchać na wszystkich IP na porcie 80. Mogą to robić tylko na jednym IP. Rozwiązanie okazało się proste, szybkie i skuteczne. Na początek konfigurujemy dnsmasq dodając wpisy w postaci:

```bash
address=/domena1.dev/127.0.0.2
address=/domena2.dev/127.0.0.3
```

Jak skonfigurować dnsmasq jeśli używamy go po raz pierwszy? Wcześniej dodajemy wpis w takiej samej postaci jak w resolv.conf, czyli

```bash
maneserver 123.456.789.123
```

To jest oczywiście jakiś wymyślony adres serwera DNS, zmieniamy go na taki, który nam pasuje. W pliku resolv.conf dodajemy wpis:

```bash
nameserver 127.0.0.1
```

Restartujemy dnsmasq i cieszymy się serwerem DNS na własnym komputerze.

Teraz jak skonfigurować lighttpd? Okazuje się, że jest to wręcz banalne. Do pliku konfiguracyjnego dodajemy wpis w postaci:

```bash
server.bind="127.0.0.2"
```

W pliku konfiguracyjnym apache dodajemy podobny wpis, a wygląda on tak:

```bash
Listen 127.0.0.3:80
```

Pamiętajmy o zmianach adresów dla serwerów wirtualnych jeśli będą potrzebne. Restartujemy serwery www i cieszymy się z dwóch na jednym komputerze.