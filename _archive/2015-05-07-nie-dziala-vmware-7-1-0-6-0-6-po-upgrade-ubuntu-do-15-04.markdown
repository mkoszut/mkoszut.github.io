---
title:  "Nie działa VMware 7.1.0/6.0.6 po upgrade Ubuntu do 15.04"
date:   2015-05-07 07:56:39
description: "Nie działa VMware 7.1.0/6.0.6 po upgrade Ubuntu do 15.04"
---

Po aktualizacji systemu z wersji 14.04 do 15.04, oczywiście po drodze przechodząc przez 14.10 okazało się, że nie uruchamia się VMware Player w wersji 6.0.6. No to szybka aktualizacja VMware Player'a do 7.1.0 i nadal to samo. Jak żyć?!

Zamiast działającego VMware dostałem lakoniczny komunikat z informacją, że więcej informacji znajdę w logach. Logi nie powiedziały mi niczego konkretnego, a przynajniej ja nie zrozumiałem z tego za wiele. O to co w nich znalazłem:

```bash
2015-05-07T09:35:06.518+02:00| vthread-4| I120: Building module with command "/usr/bin/make -j4 -C /tmp/modconfig-TTtpFI/vmnet-only auto-build HEADER_DIR=/lib/modules/3.19.0-16-generic/build/include CC=/usr/bin/gcc IS_GCC_3=no"<br>2015-05-07T09:35:08.644+02:00| vthread-4| W110: Failed to build vmnet.  Failed to execute the build command.
```

Po rozmowie z Mr. Google okazało się, że działającym rozwiązaniem jest zastosowanie patch'a. Poniżej instrukcja skąd go wziąć i jak go zaaplikować.

```bash
curl http://pastie.org/pastes/9934018/download -o /tmp/vmnet-3.19.patch
cd /usr/lib/vmware/modules/source
sudo tar -xf vmnet.tar
sudo patch -p0 -i /tmp/vmnet-3.19.patch
sudo tar -cf vmnet.tar vmnet-only
sudo rm -r *-only
sudo vmware-modconfig --console --install-all
```

I już wporządku mój żołądku.