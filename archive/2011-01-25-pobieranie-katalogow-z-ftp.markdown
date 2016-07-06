---
title:  "Pobieranie katalogów z FTP"
date:   2011-01-25 23:01:25
description: "Pobieranie katalogów z FTP"
---

Tym razem którki wpis o tym jak pobierać katalogi z serwera FTP. Standardowe narzędzie jakim jest konsolowy klient ftp nie posiada takiej opcji. A inne narzędzia jak chociażby klient FTP dostępny w Krusaderze często zawodzą gdy chcemy pobrać wiele plików wraz ze strukturą katalogów w jakiej się znajdują. Z pomocą przychodzi nam wget. Musimy tylko wiedzieć jak wywołać wgeta z pocją logowania do serwera FTP. To naprawdę nie jest skomplikowane. Polecenie to ma postać

```bash
wget --user=nazwa_uzytkownika --password='haslo_dla_uzytkownika' -r ftp://adres.serwera.pl/katalog
```

I to wszystko. Mam nadzieję, że komuś się to przyda.