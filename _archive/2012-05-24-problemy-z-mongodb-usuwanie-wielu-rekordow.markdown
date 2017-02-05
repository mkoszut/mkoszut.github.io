---
title:  "Problemy z MongoDB - usuwanie wielu rekordów"
date:   2012-05-24 17:20:45
description: "Problemy z MongoDB - usuwanie wielu rekordów"
---

Spotkałem się ostatnio z ciekawym, jednocześnie wkurzającym problemem. Usuwanie wielu, coś koło 75-80% kolekcji, rekordów zajmuje bardzo, ale to bardzo dużo czasu. Jednym z rozwiązań znalezionych w sieci okazało się napisanie skryptu, który wyciąga pojedyncze rekordy i dopiero wtedy je usuwa. Podobno podnosi to wydajność o rząd wielkości. Jednak czas jaki usuwałyby się rekordy przy ogólnym zapytaniu do bazy, a nawet dziesięciokrotne przyspieszenie go, nie był dla mnie satysfakcjonujący. Może trwało by to cały dzień.

Rozwiązanie problemu okazało się banalne, chociaż z nieznanych powodów na początku Mongos odmawiał współpracy. Pierwszym pomysłem był wyciągnięcie z kolekcji tylko tych rekordów, które były potrzebne a potem dodanie indeksów. Jednak jak się okazało baza wywalała się i nie udało mi się dojść tak naprawdę dlaczego. Powstawał błąd DR102 związany z zapisem dużej paczki (ponad 512MB) danych i jurnalem.

Okazało się, że skopiowanie kolekcji do nowej, z już dodanym indeksami rozwiązuje ten problem. Dlaczego? Nie wiem ale działa. I wszystko zajęło znacznie mniej czasu niż usuwanie. Zrobiłem to takimi mniej-więcej zapytaniami

```javascript
db.collectionNew.ensureIndex({'date':1});
start = new Date().getTime();
db.collection.find({'date': {'$gt': timestamp }}).forEach( function(c){db.collectionNew.insert(c)} );
db.collection.find({'date': {'$gt': start }}).forEach( function(c){db.collectionNew.insert(c)} );
db.collection.renameCollection("collectionOld"); db.collectionNew.renameCollection("collection");
```

Indeksów oczywiście było więcej. Zapytania te dodają indeks do nowej kolekcji, następnie sprawdzają datę początku kopiowania a następnie kopiują dane, które zostały dodane do kolekcji po rozpoczęciu pierwszego kopiowania. Następnie kolekcje mają zmieniane nazwy.