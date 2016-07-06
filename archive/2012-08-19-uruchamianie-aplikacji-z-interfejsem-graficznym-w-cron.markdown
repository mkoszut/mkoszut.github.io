---
title:  "Uruchamianie aplikacji z interfejsem graficznym w CRON"
date:   2012-08-19 21:36:50
description: "Uruchamianie aplikacji z interfejsem graficznym w CRON"
---

CRON potężnym narzędziem jest i tego tłumaczyć nie trzeba. Wykonywanie prostych skryptów o określonej godzinie zawsze jest przydatne ale co zrobić jak chcemy uruchomić aplikację z interfejsem graficznym? Okazuje się, że proste dodanie ścieżki do pliku wykonywalnego w crontab nie daje efektów. I co dalej maturzysto? Rozwiązanie okazuje się banalne, jednak trzeba chwilę pogooglać. A tu będzie podane na tacy :) Wystarczy dodać przed poleceniem komendę export DISPLAY=:0 W całości linia w crontab będzie wyglądać następująco:

```bash
0  0 * * *   uzytkownik  export DISPLAY=:0 && /sciezka/do/pliku
```

Oczywiście to polecenie wykona się każdego dnia o północy.