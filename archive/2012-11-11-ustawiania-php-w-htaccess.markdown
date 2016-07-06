---
title:  "Ustawiania PHP w .htaccess"
date:   2012-11-11 17:18:02
description: "Ustawiania PHP w .htaccess"
---

Człowiek uczy się całe życie. Dzisiaj zupełnie przypadkiem dowiedziałem się, że można ustawienia PHP dodawać w pliku .htaccess Apache. A wydawało mi się, że już mało rzeczy jest w stanie mnie zaskoczyć :)

Robi się to tak:

```
php_flag nazwa_ustawienia wartosc_ustawienia
```

Jak się okazuje, na niektórych hostingach jest to jedyny sposób na zmianę ustawień bez zaśmiecania każdego pliku gdzie jest ono nam potrzebne.