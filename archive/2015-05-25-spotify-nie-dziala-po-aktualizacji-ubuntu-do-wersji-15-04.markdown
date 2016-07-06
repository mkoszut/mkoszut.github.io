---
title:  "Spotify nie działa po aktualizacji Ubuntu do wersji 15.04"
date:   2015-05-25 13:38:02
description: "Spotify nie działa po aktualizacji Ubuntu do wersji 15.04"
---

Po aktualizacji Ubuntu i instalacji Spotify dostałem taki oto komunikat

```bash
spotify: error while loading shared libraries: libgcrypt.so.11: cannot open shared object file: No such file or directory
```

Działającym rozwiązaniem okazało się zainstalowanie [dobianowej](https://packages.debian.org/wheezy/amd64/libgcrypt11/download) paczki z biblioteką.