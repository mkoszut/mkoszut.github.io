---
title:  "Graylog2, czyli jak łatwo zarządzać logami"
date:   2011-11-28 22:22:52
description: "Graylog2, czyli jak łatwo zarządzać logami"
---

Zarządzanie logami bywa strasznie upierdliwe. Są one rozrzucone po całym systemie, pliki mają różne uprawnienia i żeby je przejrzeć trzeba o tym pamiętać.

Ostatnio bawiłem się trochę MongoDB i padł pomysł trzymania logów aplikacji właśnie w mongosie. Jakby na to nie patrzyć ta baza jest praktycznie stworzona do takiego celu. Jednak zanim przystąpiłem do pisania klasy do obsługi tego rozejrzałem się po sieci w poszukiwaniu gotowych rozwiązań. Ciekawą propozycją wydawał się Graylog2. Zainstalowałem. Zacząłem testować. Faktycznie świetne trafienie.

Trochę o Graylog2. Jest to narzędzie składające się z dwóch części. Pierwsza to serwer stworzony do przechwytywania i dodawania logów do bazy. Serwer jest napisany w javie co jest małym mankamentem, już po odpaleniu zajmuje 110MB ramu. Co przy słabszych kompach może mieć znaczenie. Druga to webowy interfejs stworzony do zarządzania, wyświetlania logów zgromadzonych przez serwer. Interfejs został napisany w railsach, co moim zdaniem pisze mu się na plus.

Instalacja i konfiguracja zarówno serwera, jak i interfejsy webowego jest banalna. Dla popularnych linuxów są nawet przygotowane paczki.

O samej konfiguracji nie będę się rozpisywał, dokumentacja jest poprawna i przejrzysta, za to chciałbym przedstawić skrypt do startu/stopu serwera oraz co zrobić aby Graylog2 uruchamiał się po starcie systemu.

Dla systemów debianowych skrypt umieszczamy w katalogu /etc/init.d/, a wygląda on tak:

```bash
#! /bin/sh
set -ePATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
DESC="Graylog2"
NAME=graylog2
d_start() {
  start-stop-daemon --start --background --exec /bin/graylog2ctl start
}
d_stop() {
  start-stop-daemon --stop --quiet --pidfile /tmp/graylog2.pid
}
case "$1" in
start)
  echo -n "Starting $DESC:"
  d_start
  echo "."
;;
stop)
  echo -n "Stopping $DESC:"
  d_stop
  echo "."
;;
restart|force-reload)
  echo -n "Restarting $DESC:"
  d_stop
  sleep 1
  d_start
  echo "."
;;
*)
  echo "Usage: {start|stop|restart|force-reload}" >&2
  exit 1
;;
esac
exit 0
exit 0
```

Skrypt jest całkiem prosty, a jest też całkiem użyteczny. Aby skrypt był uruchamiany przy starcie sytemu musi być dodany likn do niego w katalogach rc.*. W systemach dobianowych robi się to komendą

```bash
update-rc.d nazwa_skryptu defaults
```

To tyle tym razem. Następnym razem napisze o konfiguracji rsyslog-a, która pozwoli pluć mu do Graylog2 oraz o komunikacji z nim przy pomocy GELF, czymkolwiek to jest.