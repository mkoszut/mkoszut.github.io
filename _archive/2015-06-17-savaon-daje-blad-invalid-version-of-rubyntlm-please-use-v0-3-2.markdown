---
title:  "Savaon daje błąd 'Invalid version of rubyntlm. Please use v0.3.2+'"
date:   2015-06-17 09:03:03
description: "Savaon daje błąd 'Invalid version of rubyntlm. Please use v0.3.2+'"
---

Próba użycie gemu savon skończyła się takim błędem?

```
net_http.rb:58:in `check_net_ntlm_version!': Invalid version of rubyntlm. Please use v0.3.2+. (ArgumentError)
```

A jesteś pewien, że gem jest w wersji 0.5.0. Najprostszym działającym rozwiązaniem tego problemu jest usunięcie gemu ntlm-http. Usuwamy używając komendy:

```bash
rvmsudo gem uninstall ntlm-http
```

Z tego co wiem innego rozwiązanie jak na razie nie ma.