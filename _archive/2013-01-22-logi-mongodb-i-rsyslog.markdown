---
title:  "Logi MongoDB i RSyslog"
date:   2013-01-22 12:09:52
description: "Logi MongoDB i RSyslog"
---

W wersji MongoDB 2.1 pojawiła się możliwość wysyłania logów mongosa do sysloga. Jeśli korzystamy z jakiegoś centralnego serwera logów zmienienie ustawień jest z naszej perspektywy pożądane. A można powiedzieć, że wręcz wymagane. Jak to zrobić?

Wystarczy w pliku konfiguracyjnym zakomentować zmienna ustawiającą ścieżkę pliku logów oraz dodać nowy parametr syslog ustawiony na true. Nasze zmiany w pliku konfiguracyjnym będą wyglądały mniej-więcej tak:

```
#logpath=/var/log/mongodb/mongodb.log
#logappend=true
syslog=true
```

Możemy jeszcze wykonać dwie operacje. Pierwsza to ustawienie zmiennej quiet na true. Spowoduje to wyciszenie logowania części operacji wykonywanych przez mongosa, w tym  zaakceptowane połączenia i krótko trwające operacje. Przydaje się to ponieważ MongoDB tworzy olbrzymie ilości logów i jeśli nie są one nam niezbędne do szczęścia to można je wyciszyć. Drugą rzeczą jaką możemy zrobić jest zmiana maksymalnego czasu nielogowanych zapytań. Standardowo jest on ustawiony na 100ms. Jeśli nasze zapytania standardowo wykonują się dłużej to też nie ma co zaśmiecać sobie logów takimi operacjami. Zmanę można zrobić wykonując polecenie

```javascript
db.setProfilingLevel(0,1000)
```

Ustawi ono level profilowania na 0 oraz czas maksymalny czas nielogowanych zapytań na 1000ms.

Miłej zabawy!