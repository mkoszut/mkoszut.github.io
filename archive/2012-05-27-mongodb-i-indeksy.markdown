---
title:  "MongoDB i indeksy"
date:   2012-05-27 14:33:49
description: "MongoDB i indeksy"
---

Jak indeksować bazę MongoDB? No właśnie, ostatnio okazało się, że mimo dodawania kolejnych indeksów baza, którą się zajmuję, wcale nie przyspiesza, a wręcz zwalnia. Oczywiście chodzi o czas wykonywania zapytań do bazy. Okazało się, że problemem jest ilość indeksów. Budowanie wielu pojedynczych indeksów przez wykonywanie operacji

```javascript
db.collection.ensureIndex({'nazwa_pola':1});
```

wcale nie powoduje dodania ich podczas wyszukiwania dokumentów. Jak do tego doszedłem? Z pomocą przyszło mi polecenie explain() wbudowane w MongoDB.
Okazało się, że podczas zapytania wykorzystywany i tak jest tylko jeden index. Jeśli w zapytaniu wykorzystujemy więcej niż jedno kryterium to trzeba zbudować index zawierający wszystkie kryteria. Polecenie to wygląda bardzo podobnie jak w przypadku pojedynczego indeksu, a mianowicie

```javascript
db.collection.ensureIndex({'nazwa_pola_1':1, 'nazwa_pola_2':1});
```

W testach wyszło, że czas wykonania zapytania opartego o tak zbudowanych indeksach jest o 40% mniejszy niż w przypadku dwóch pojedynczych indeksów. 

Dodając indeksy możemy użyć opcji background, wtedy indeksy budują się w tle, co umożliwia nam ciągły dostęp do kolekcji. W sieci znalazłem dodatkowo ciekawy skrypt umożliwiający sprawdzenie postępu dodawania indeksów do kolekcji. O to i on.

```javascript
var currentOps = db.currentOp();
 
if(!currentOps.inprog || currentOps.inprog.length < 1) {
    print("No operations in progress");
} else {
    for(o in currentOps.inprog) {
        var op = currentOps.inprog[o];
        if(op.msg && op.msg.match(/bg index build/)) {
            print(op.opid+' - '+op.msg);
        }
    }
}
```

Wywołujemy go podając polecenie:

```bash
mongo nazwa_bazy nazwa_skryptu.js
```