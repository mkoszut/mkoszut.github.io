---
title:  "VMware Player instalacja na Ubuntu 12.10"
date:   2012-10-21 15:20:26
description: "VMware Player instalacja na Ubuntu 12.10"
---

Kolejny wpis ku pamięci. Jak wiadomo właśnie wyszła nowa wersja Ubuntu, na pierwszy rzut oka niewiele się zmieniło, może to i dobrze nie trzeba się przyzwyczajać do nowych rzeczy. Choć nie powiem postanowiłem spróbować zabawy z Unity, wcześniej zawsze używałem KDE. Ale do rzeczy, przecież nie o to w tym wpisie chodziło.

Jednym z podstawowych narzędzi mojej pracy jest wirtualna maszyna, najczęściej używam VMware Player. Zrobiłem update Ubuntu w wersji 12.04 do 12.10 i okazało się, że nie mogę podnieść VMPlayer-a. No więc zacząłem szukać w sieci, pierwsze co znalazłem to informacja na stronie dokumentacji Ubuntu, że błąd jest znany, na stronie błędu nie ma informacji o jego rozwiązaniu. Załamka. I co teraz. No ale nie poddałem się poszukałem dalej i udało mi się znaleźć patch rozwiązujący ten problem (do pobrania z linku). Przed zastosowaniem patcha wykonujemy polecenie

```bash
sudo rm /lib/modules/$(uname -r)/misc/vm*
```

oczywiście jeśli wcześniej już zainstalowaliśmy moduły VMware (ale skąd wiedzielibyśmy, że nie działa jeśli ich nie ma :) Teraz uruchamiamy patch i cieszymy się znowu działającą maszyną wirtualną. 