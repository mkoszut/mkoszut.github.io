---
title:  "Ssh-client - zmiana portu dla określonej grupy domen"
date:   2013-01-22 12:26:38
description: "Ssh-client - zmiana portu dla określonej grupy domen"
---

Rzeczy wydaje się bardzo prost jeśli mamy tylko jeden serwer, do którego logujemy się na innym porcie niż 22. Można logując się ustawiać parametr -p numer_portu przy każdym logowaniu. Jednak jak w całej grupie domen serwery ssh mają ustawiony inny niż standardowy port 22 to może warto zrobić taką konfigurację klienta ssh żeby sam zmieniał port. W takim przypadku nie trzeba dodatkowo pamiętać jakie parametry zmieniają port połączenia w różnych komendach, a sprawdzanie za każdym razem w dokumentacji bywa upierdliwe. Jak większość takich przypadków rzecz okazuje się prosta, a może nawet banalna.

W katalogu .ssh znajdującym się w naszym katalogu domowych tworzymy, jeśli jeszcze nie istnieje, plik config. Dodajemy do niego wpis

```
Host *.domena.com
   Port numer_portu
```

i przy następnym połączeniu nie musimy się już martwić.