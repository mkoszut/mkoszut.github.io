---
title:  "Dodawanie plików do Octawave OCS przy pomocy API"
date:   2015-04-26 15:34:01
description: "Dodawanie plików do Octawave OCS przy pomocy API"
---

API Octawave jest zgodne z API OpenStack, jednak posiada kilka konfiguracyjnych różnic, o których można przeczytać tutaj. Do pracy z API Ocatawave postanowiłem użyć gemu Fog (w projekcie był już używany do wrzucania obrazów do Amazon S3 przez gem Carrierwave). Po przejrzeniu dokumentacj gemu na githubie wszystko wydawało się proste.

Problemy zaczęły się jak po wejściu w zakładkę Oktawave Cloud Storage zobaczyłem formularz logowania, a moje dane do logowania nie pasowały. Na szczęście nad formularzem była informacja o zarządzaniu użytkownikami. Tam okazało się, że można dodać nowego użytkownika, a prawidłowy login dla administratora to "admin" a nie mój login do konta. OK, nie jest to oczywiste ale do zaakceptowania. Dodałem użytkownika, próbuję wylistować katalogi i okazuje się, że nie mam uprawnień. Szukam jakiś opcj konfiguracyjny ale nic nie znajduje. Przeszukiwanie insternetu też nie przyniosło rezultatu. Okazało się, że przy użytkowniki jest magiczny plusik, który rozwiaja box z zarządzaniem uprawnieniami (patrz obrazy poniżej).

![Zarządzanie uprawnieniami katalogów w w Octawave OCS](https://s3-us-west-2.amazonaws.com/mkoszut/ocs_octawave.png "Zarządzanie uprawnieniami katalogów w w Octawave OCS")

![Zarządzanie uprawnieniami katalogów w w Octawave OCS](https://s3-us-west-2.amazonaws.com/mkoszut/ocs_2.png "Zarządzanie uprawnieniami katalogów w w Octawave OCS")

Okazuje się, że to pomogło. Została jeszcze jedna nieoczywistość do wyjaśnienia, mianowicie to, że kontenery widoczne są jako katalogi. Nie jest o oczywiste, a mogła by się gdzieś znaleźć taka informacja.

A tu jeszcze kawałek kodu dodający plik do folderu na OCS.

```ruby
service = Fog::Storage.new({
    :provider => 'OpenStack', 
    :openstack_username => 'login:login', 
    :openstack_api_key => 'haslo', 
    :openstack_auth_url => 'https://ocs-pl.oktawave.com/auth/v1.0'
})
colors_of_lychee_backup = service.directories.get 'colors_of_lychee_backup'
colors_of_lychee_backup.files.create :key => 'space.jpg', :body => File.open(File.join(File.dirname(__FILE__), 'zorza_3.jpg'))
```
Wszystko wydaje się proste, w sumie trudne nie jest. Na pewno brakuje dokumentacj, która miałaby wszystkie te informację zebrane w jednym miejscu.