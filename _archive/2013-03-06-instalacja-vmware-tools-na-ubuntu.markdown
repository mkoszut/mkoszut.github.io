---
title:  "Instalacja VMWare Tools na Ubuntu"
date:   2013-03-06 16:15:22
description: "Instalacja VMWare Tools na Ubuntu"
---

Jeśli chcemy dodać udostępnianie katalogów z dysku naszego komputera maszynie wirtualnej musimy mieć zainstalowane VMWare Toola. Rzecz niby banalna a potrafi zająć chwilę i zmusza do przeglądania internetu w poszukiwaniu niektórych informacji. A bywa to upierdliwe zwłaszcza jak robi się to już któryś raz bo znowu zmieniała się wersja jądra systemu.

Pierwszą rzeczą jaką musimy zrobić to włączyć i podmontować VMVare Tools. Włączamy je przez kliknięcie opcji Install VMWare Tools w VMWare. Następnie montujemy dysk do katalogu komendą

```bash
sudo mount /dev/cdrom /media/cdrom
```

Jeśli katalog /media/cdrom nie istnieje tworzymy go, ewentualnie montujemy w innym miejscu.

Następnie kopiujemy plik VMwareTools-X.X.X-XXXXXX.tar.gz do katalogu domowego i rozpakowujemy go. Przed przystąpieniem do instalacji trzeba zainstalować GCC i Kernel Headers. W tym celu wykonujemy takie oto polecenie 

```bash
sudo apt-get install gcc build-essential checkinstall linux-headers-$(uname -r)
```

Teraz przechodzimy do katalogu w którym są rozpakowane VMWare Tools i wykonujemy następujące polecenie

```bash
sudo ./vmware-install.pl
```

Postępujemy zgodnie z poleceniami instalatora, czyli w większości przypadków naciskamy Enter. Po pomyślnej instalacji restartujemy system i wszystko powinno śmigać.