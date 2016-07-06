---
title:  "Trochę o SVN, a zwłaszcza o propset svn:ignore"
date:   2011-09-18 14:01:26
description: "Trochę o SVN, a zwłaszcza o propset svn:ignore"
---

Ten artykuł tak na prawdę tworzę dla siebie. Ile razy potrzebuje ignorować jakieś pliki bądź katalogi w zarządzanym przez siebie repozytorium opartym na SVN muszę skorzystać z pomocy wujka Google, a największym problemem jest to, że nie wszystko co znajdę działa. Dlatego właśnie wpis ten tworzę głównie dla siebie, będę miał to zawsze pod ręką i będę wiedział gdzie tego szukać. Przybliżę tutaj jak to zrobić korzystając z klienta linuxowego przy pomocy linii komend.

Jak korzystać z ignorowania plików, grup plików bądź katalogów w SVN? Sprawa wydaje się prosta. Korzystamy z polecenia

```bash
svn propset svn:ignore siezka_do_pliku .
```

I to prawie tyle trzeba pamiętać o tym, żeby ignorowane pliki nie były wcześniej częścią repozytorium. Dodanie do ignorowanych plików istniejących w repozytorium potrafi nie działać.

Kolejną rzeczą, o której warto pamiętać to przed dodaniem plików do katalogu który właśnie dodaliśmy do ignorowanych zrobić commit i faktycznie dodać ten katalog do ignorowanych.

A co zrobić jak chcemy ignorować jakąś grupę plików w katalogu. Na przykład jak chcemy ignorować pliki o rozszerzeniu .php. Sprawa jest prosta robimy tak

```bash
svn propset svn:ignore *.php .
```

Pamiętajmy o kropce na końcu. Zaobserwowałem, że jest często zjadana a bez niej nic nie zdziałamy.

To by było na tyle.