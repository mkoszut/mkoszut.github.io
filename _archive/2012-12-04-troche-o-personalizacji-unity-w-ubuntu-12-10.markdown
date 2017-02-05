---
title:  "Trochę o personalizacji Unity w Ubuntu 12.10"
date:   2012-12-04 17:04:08
description: "Trochę o personalizacji Unity w Ubuntu 12.10"
---

Po update Ubuntu do wersji 12.10 postanowiłem pobawić się Unity. Do tej pory, przez wiele lat używałem KDE, jednak Unity spodobało mi się tak bardzo, że postanowiłem na wszystkich komputerach zmienić środowisko graficzne. I znowu pojawiły się te same problemy.

Pierwszym problemem było dodanie skrótu do Launchera, który miałby uruchamiać jakiś skrypt. Oczywiście możemy uruchomić skrypt z terminala i będziemy mieli ikonkę na Launcherze, ale jest ona bez nazwy właściwej dla programu tylko ze ścieżką do tego właśnie skryptu. No i co z tym zrobić. Wystarczyło przecież dodać odpowiedni plik do właściwego katalogu. Ale jaki plik do jakiego to już nie pamiętam. No dobra bez przedłużania. Trzeba dodać plik z rozszerzeniem *.desktop do katalogu ~/.local/share/applications/. Plik ten ma mieć postać

```
[Desktop Entry]
Comment[pl]=Komentarz do programu
Comment=Komentarz do programu
Exec='/sciezka/do/skryptu'
Icon=/sciezka/do/grafiki.png
Name[pl]=Nazwa programu
Name=Nazwa programu
StartupNotify=true
Terminal=false
Type=Application
Version=1.0
```

Teraz przeciągamy ikonkę na Launcher i cieszymy się ładną ikonką, która wyświetli się po każdym uruchomieniu.

Następną kwestią jest auto-uruchamianie programu po zalogowaniu. To jest jeszcze prostsze ale też trzeba trochę pogrzebać. No więc tak, wyszykujemy aplikacji Startup Applications i tam dodajemy nazwę programu lub ścieżkę do skryptu.