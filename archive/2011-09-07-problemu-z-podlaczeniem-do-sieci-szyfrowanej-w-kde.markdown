---
title:  "Problemu z podłączeniem do sieci szyfrowanej w KDE"
date:   2011-09-07 17:13:57
description: "Problemu z podłączeniem do sieci szyfrowanej w KDE"
---

KDE w wersji 4 ma jeden znany mi problem. Jest to brak możliwości połączenia się przy pomocy narzędzi plasma-widget-networkmanagement lub network-manager-kde z sieciami bezprzewodowymi szyfrowanymi WPA lub WPA2. Oczywiście możemy łączyć się korzystając z lini komend, ale to już nie te czasy, teraz chcemy najczęściej mieć wszystko dostępne na jedno klikniecie. Istnieje jednak prosty sposób na ominięcie tego problemu, a mianowicie użycie nm-applet czyli managera sieci dla GNOME.

Pierwsza rzecz jaką robimy to usuwamy cały pakiet network-manager. Dla systemów opartych na Debianie będzie to polecenie

```php
sudo aptitude purge network-manager
```

Następnie instalujemy paczkę network-manager-gnome poleceniem

```php
sudo aptitude install network-manager-gnome
```

Managera uruchamiamy używając polecenia nm-applet. Problem jest już rozwiązany. Dla wygody możemy dodać w stawieniach KDE w zakładce "Uruchamiane i wyłączanie", w dziale "Pliki pulpitu" automatyczne uruchamianie nm-applet przy starcie KDE.