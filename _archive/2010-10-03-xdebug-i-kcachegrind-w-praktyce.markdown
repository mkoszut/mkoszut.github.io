---
title:  "Xdebug i KCachegrind w praktyce"
date:   2010-10-03 14:34:43
description: "Xdebug i KCachegrind w praktyce"
---

Dzisiaj trochę o optymalizacji. Jednym z najważniejszych atrybutów aplikacji internetowych jest czas ich wykonania. Każda strona powinna ładować się możliwie szybko, nie generując niepotrzebnego obciążenia serwera. Problemem jest jak sprawdzić, która część napisanego przez nas kodu należy poprawić w celu zwiększenia wydajności aplikacji. Jednym z narzędzi pozwalających to zrobić jest Xdebug.

Xdebug jest rozszerzeniem dostępnym dla języka PHP. Jego instalacją, przynajmniej dla większości systemów linuxowych, sprowadza się do ściągnięcia paczki z repozytoriów. Konfiguracja jest równie prosta. W pliku konfiguracyjnym php.ini, lub dla linuxów ubuntupodobnych xdebug.ini dodajemy parę linijek. Wyglądają one tak:

```
zend_extension=/usr/lib/php5/20090626/xdebug.so 
xdebug.profiler_enable_trigger = 1
xdebug.profiler_enable = 1
xdebug.profiler_output_dir = /tmp
xdebug.profiler_output_name = cachegrind.out.%s.%u
```

Pierwsza linijka powstanie automatycznie. I raczej nie należy jej zmieniać. Kolejne dyrektywy mówią o: tworzeniu raportów dla każdego skryptu wywołanego na serwerze, katalogu, w którym mają być zapisywane raporty oraz o strukturze nazwy pliku raportu. To wystarczy.

Teraz jak już mamy raporty generowane przez Xdebug musimy mieć możliwość odczytania ich. Jednym z takich narzędzi jest KCachegrind. Daje on możliwość sprawdzenia czasu potrzebnego na wykonanie skryptu, liczby wywołań danych funkcji. Wszystko jest przedstawione w bardzo czytelny, również graficzny sposób.