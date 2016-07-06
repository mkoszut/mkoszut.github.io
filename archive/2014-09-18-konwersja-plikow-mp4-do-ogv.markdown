---
title:  "Konwersja plików .mp4 do .ogv"
date:   2014-09-18 14:52:52
description: "Konwersja plików .mp4 do .ogv"
---

Konwertowanie plików wideo najczęściej robi się przy użyciu FFmpeg'a jednak w ostatnich wersjach Ubuntu FFmpeg nie jest defaultowo instalowany. Ba nie ma go nawet w oficjalnym repo. Oczywiście można skompilować/zainstalować go z paczki dostępnej w sieci ale możemy również użyć dostępnego standardowo avconv. Jest to konwerter działający tak samo jak FFmpge, ma nawet bardzo podobną składnię.

Jak już rozwiążemy problem konwertera to okaże się, że standardowe ustawienia powodują, że skonwertowany plik bardzo traci na jakości, zwłaszcza jak bym wysokiej. W tym miejscu podam opcje z jakimi należy wywołać konwerter żeby straty były stosunkowo niewielkie a i nowy plik nie był gigantycznych rozmiarów.

Oto i one:

```bash
avconv -i input.mp4 -acodec libvorbis -vcodec libtheora -q:v 10 -q:a 5 -f ogg output.ogv
```