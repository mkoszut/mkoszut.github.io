---
title:  "nginx + thin + rvm + root = mały problem ale do przejścia"
date:   2012-09-25 22:32:52
description: "nginx + thin + rvm + root = mały problem ale do przejścia"
---

Wydaje się, że taka konfiguracja dla niewielkich aplikacji webowych tworzonych w oparciu o Ruby on Rails jest bardzo dobrym pomysłem. Tak też jest, trzeba to wszystko jeszcze połączyć w zgrabną całość jeśli chcemy to umieścić na serwerze. I nagle powstają problemy. Ale może zacznę od początku.

Zakładam, że nginx oraz Ruby z RVM są już zainstalowane i nie trzeba tego robić. A jak ktoś nie ma to w sieci znajdzie masę manuali specjalnie dla jego systemu operacyjnego.

Standardowo opiszę jak to zrobić dla Ubuntu i innych systemów opartych na Debianie.

Na początek instalujemy thin-a.

```bash
rvmsudo gem install thin
```

Chcemy, aby thin był uruchamiany przy starcie systemu więc dodajemy go. W przypadku serwerów na systemach opartych o Debiana robimy to poleceniem

```bash
sudo /usr/sbin/update-rc.d -f thin defaults
```

Wchodzimy do katalogu naszej aplikacji napisanej w RoR i wykonujemy polecenie tworzące plik konfiguracyjny dla thin

```bash
thin config -C /etc/thin/nazwa-aplikacji -s3 -p5000
```

Spowoduje to dodanie pliku z konfiguracją dla 3 workerów na portach 5000, 5001 oraz 5002.

Teraz tworzymy konfigurację nginx-a. Powinna ona wyglądać mniej-więcej tak:

```bash
upstream nazwaaplikacji {
  server 127.0.0.1:5000;
  server 127.0.0.1:5001;
  server 127.0.0.1:5002;
}
server {
  listen 127.0.0.2:80;
  server_name nazwa-aplikacji.com;
  root /scierzka/do/katalogu/aplikacji/public;
  location / {
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    try_files $uri/index.html $uri.html $uri @ruby;
  }
  location @ruby {
    proxy_pass http://nazwaaplikacji;
  }
}
```

Restartujemy wszystko jako root i nagle okazuje się, że to co działało pod Webrick-iem przestało działać, a my otrzymujemy błąd 502 w przeglądarce. I co teraz?

Okazuje się, że root nie ma możliwości korzystać z Ruby instalowanego przez RVM. Musimy zatem stworzyć wrapper umożliwiający dowolnemu użytkownikowi uruchomić thin z poziomu Ruby-iego zainstalowanego przez RVM. Robimy to poleceniem

```bash
rvm wrapper ruby-1.9.3-p125 bootup thin
```

W tym przypadku odnosi się on do wersji Ruby 1.9.3.p125 jak używamy innej to wpisujemy ją w to miejsce. Kolejną rzeczą, którą musimy zrobić jest zmiana w pilik /etc/init.d/thin umożliwiająca odpalenie thin z dodanego przez nas wrapper-a. Podmieniamy tylko linijkę zawierającą 

```bash
DAEMON=/usr/bin/thin
```

na 

```bash
DAEMON=/home/nazwa_usera/.rvm/wrappers/default/thin
```

W tym przypadku RVM był zainstalowany przez użytkownika nazwa_usera dlatego wrapper znajduje się w jego katalogu domowym. Wydawało by się, że teraz powinno wszystko chodzić. Ale czasami zdarza się, że w logach thin dostajemy trochę lakoniczny błąd:

```bash
/home/nazwa_usera/.rvm/gems/ruby-1.9.3-p125/gems/thin-1.5.0/lib/thin/backends/tcp_server.rb:16:in `connect': cannot load such file -- thin/connection (LoadError)
```

Nie wiadomo za bardzo co zrobić i do czego się on tak na prawdę odnosi, bo po sprawdzeniu okazuje się, że taki plik istnieje. Co to może być? Jak się okazuje w Gemfile naszego projektu trzeba dodać jeszcze gem thin.

I to byłoby na tyle. Miłej zabawy.