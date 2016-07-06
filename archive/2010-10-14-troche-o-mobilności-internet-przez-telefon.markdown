---
title:  "Trochę o mobilności - internet przez telefon"
date:   2010-10-14 10:43:51
description: "Trochę o mobilności - internet przez telefon"
---

Dzisiaj będzie o połączeniu telefonu z komputerem przez Bluetooth i korzystaniu w ten sposób z internetu.

Potrzebny nam będzie telefon i komputer posiadające Bluetooth oraz odpowiednie biblioteki dla systemu. W tym przypadku będzie to Kubuntu 10.04. Musimy mieć zainstalowane pakiet bluez-utils oraz wvdial. Konfiguracja jednego i drugiego jest prosta, jednak wymaga użycia konsoli, co może zniechęcać niektórych użytkowników.

Konfiguracja Bluetooth jest powiązana z plikiem /etc/bluetooth/rfcomm.conf. Powinien on wyglądać następująco:

```
rfcomm0 {
    bind yes;
    device MAC;
    comment "Jakaś nazwa";
}
```

W polach MAC oraz "Jakaś nazwa" wstawiamy odpowiednio adres MAC urządzenia z którym chcemy się połączyć oraz nazwę jaką ma mieć dane połączenie, jest ona dowolna.

Plik konfiguracyjny /etc/wvdial.conf powinien mieć postać odpowiadającą przykładowi poniżej, jest on sprofilowany pod sieć Era.

```
[Dialer Nazwa]
Modem = /dev/rfcomm0
Baud = 460800
Init1 = ATZ
Init2 = ATQ0 V1 E1 S0=0 &C1 &D2
Init3 = AT+CGDCONT=1,"IP","erainternet"
Init4 = AT&C1
Phone = *99#
Username = erainternet
Password = erainternet
Ask Password = 0
Dial Command = ATDTW
Stupid Mode = 1
```

Dla innych sieci będą się różnić pola Init3, Phone, Username, Password. Konfiguracje te są ogólnie dostępne dla poszczególnych sieci bez problemu. Wystarczy pochylić się nad wujkiem Google.

Połączenie uruchamiamy wydając następujące polecenia:

```
sudo rfcomm connect hci0
```

Połączenie z internetem uzyskujemy za pomocą wvdial wydając polecenie:

```
sudo wvdial Nazwa
```

Miłej zabawy wszędzie gdzie jest zasięg.