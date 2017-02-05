---
title:  "Magento, SOAP API i konfiguracja Nginx"
date:   2015-06-17 10:49:51
description: "Magento, SOAP API i konfiguracja Nginx"
---

Plik htaccess dla Magento jest dostępny wraz z paczką, którą pobieramy ze strony Magento. Na szczęście nie wszyscy używają Apache, są tacy, którzy wolą nginx. Niestety w tej paczce nie ma pliku konfiguracyjnego dla nginx i trzeba stworzyć go samemu. W sieci jest cała masa przykładowych plików i w większości przypadków wszystko działa poprawnie. Czasami trzeba coś dodać, jednak nie są to rewolucje, a raczej kosmetyczne zmiany.

Do tej pory nie miałem problemów związanych z użyciem nginx zamiast Apache.Ddoszło do tego jak chciałem komunikować się z Magento przy pomocy SOAP'owego API. Okazało się, że nie mogę się zalogować. Przejrzenie logów od razu mówi, że coś jest nie tak z konfiguracją nginx, w access.log logowanie jest takim zapytaniem

```
172.16.182.1 - - [17/Jun/2015:05:56:38 -0400] "POST /index.php/?type=v2_soap HTTP/1.1" 200 3244 "-" "Ruby"
```

A powinno być kierowane na /api.php. Do tej pory tak wyglądał wpis odpowiedzialny za przekierowania dla api

```
location /api {
	rewrite ^/api/([a-z][0-9a-z_]+)/?$ /api.php?type=$1 last;
}
```

Zamieniamy go na

```
location /api {
	rewrite ^/api(\.php)?/([a-z][0-9a-z_]+)/?$ /api.php?type=$1 last;
}
```
Istotna jest jeszcze jedna kwestia. Adres URL musi mieć na końcu index.php. Czyli URL ma kształt podobny do tego
```
https://magento.dev/index.php
```

Przy takiej konfiguracji wszystko chodzi poprawnie.