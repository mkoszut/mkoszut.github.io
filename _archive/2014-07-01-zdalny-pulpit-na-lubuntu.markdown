---
title:  "Zdalny pulpit na Lubuntu"
date:   2014-07-01 17:36:19
description: "Zdalny pulpit na Lubuntu"
---

Mam w domu netbooka z Lubuntu, który pełni rolę routera. A dokładniej rutuje internet z Aero2 do routera WiFi. W sumie nic nadzwyczajnego. Ale co godzinę trzeba do niego podejść i wpisać nową Captcha, jest to uciążliwe zwłaszcza jak człowiek wygodnie siedzi sobie na kanapie i ostatnią rzeczą na jaką ma ochotę jest odrobina ruchu.

Zatem postanowiłem uruchomić tam zdalny pulpit. Padło na xrdp. Instalacja jest jak to w przypadku większości aplikacji banalna. Jak zwykle wystarczy kilka linijek.

```bash
sudo aptitude update
sudo aptitude upgrade
sudo aptitude install xrdp
```

Teraz tworzymy plik .xsession w katalogu domowym

```bash
vim ~/.xsession
```

Dodajemy do niego magiczną linijkę

```bash
/usr/bin/lxsession -s LXDE
```

A następnie restartujemy xrdp poleceniem

```bash
sudo service xrdp restart
```

Teraz zostaje nam już tylko uruchomić klienta zdalnego pulpitu i połączyć się z naszym serwerem. Proste jak konstrukcja cepa :D