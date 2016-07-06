---
title:  "Jaki Linux na iBook G4?"
date:   2011-06-12 19:13:27
description: "Jaki Linux na iBook G4?"
---

Tym razem coś o linuxie na maku.

Jak się okazuje sprawa nie jest całkiem banalna. Głównym problemem jest tutaj architektura PowerPC. W przypadku dystrybucji opartych o binarne repozytoria nie wszystkie paczki są dostępne w tej architekturze i właśnie w tym momęcie zaczeły się moje problemy i kilku dniowe przeboje z iBook'iem G4.

Zacząłem od instalacji poczciwego Debiana Squeeze. Instalacja przebiegła szybko i sprawnie. Zarówno dla wersji z KDE jak i XFCE. Jednak jak się okazało w repozytowium nie ma pczaki Cromium dla architektury PowerPC. Trochę mnie to zmartwiło, jest to moja ulubiona przeglądarka.

Stwierdziłęm, co tam, zainstaluje sobie Gentoo. Tam mogę sobie skompilować wszystko. Instalacja przebiegała całkiem sprawnie, do momętu gdy trzeba było uruchomić skompilowane jądro. Okazał się, że jądro generowane automatycznie nie podnosi się. Nie poddałem się, wróciłem do konfirugacji. Tym razem użyłem konfiga dostarczonego wraz z płytą instalacyjną. Takia sama systuacja. Nie podniosło się dokłądnie w tym samym momęcie. Po kilku próbach i współpracy z wujkiem Google dałem sobie spokuj. To minęły już 3 dni od rozpoczęcia tej zabawy. Poddałem się.

Następnie spróbowałem Ubuntu 10.10. Instalacja tak jak w przypadku Debiana przebiegła szybki i sprawnie. Pierwszy problem. Ubuntu automatycznie nie wykryło sterowników do bezprzewodowej karty siebiowej. Instalacja z palca spowodowała, że karta się podniosła. Drugi problem. Brak akceleracji 3D dla karty Radeon Mobility 9200. Instalacja z terminalu, przy wparciu z dostępnych w sieci tutoriali zakończyła się fiaskiem. Może za szybko się poddałem, ale nie po to jest Ubuntu żeby z nim walczyć. Instalując je liczę na szybką i bezproblemową konfigurację. A na dodatek była to wersja 10.10 a nie 11.04.

Pomyślałem jeszcze o instalacju FreeBSD 8.2. Zrezygnowałem już po przeczytaniu znanych problemów. Nie działa wsparcie dla klawiatury i touchpada. Nie chciało mi się z tym bawić.

Cóż po paru dniach wróciłem do starego poczciwego Debiana. Siec bezprzewodową zainstalowałęm bez problemów. Terminal przydał się również do włączenia akceleracji 3D. Ale już za pierwszym raziem ruszyło. Dokumentacja Debiana bardzo się przydała. Ale od tego w końcu ona jest.

Jak widać to, że jakaś dystrybucja oficjalnie, bądz nie całkiem oficjalnie wspiera architekturę PowerPC nie znaczy, że system zaraz będzie gotowy do użycia. Jednak Debian zdecydowanie wygrywa.