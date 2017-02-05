---
title:  "Upload plików bez przeładowania strony - Ajaxupload"
date:   2010-12-15 19:06:21
description: "Upload plików bez przeładowania strony - Ajaxupload"
---

Wykorzystanie Ajax-a w tworzonych przez nas aplikacjach internetowych jest coraz większe. Pozwala nam on zaoszczędzić nie jedno przeładowanie strony, a co za tym idzie zmniejszamy obciążenie łącza, dodając swego rodzaju dynamikę do statycznego obrazu strony.

Jednym z ułatwień jest uploadowanie plików. Ja w tym celu wykorzystałem skrypt napisany przez Andrew Valums, można go znaleźć tutaj. Skrypt pozwala na odebranie informacji zwrotnej, dzięki czemu jest bardzo wygony w użyciu i pozwala na dalszą interakcję w dodane już pliki, na przykład ich usuwanie jeśli się rozmyślimy. Nie będę się tutaj skupiał na istocie działania skryptu, przedstawię tylko jego praktyczne zastosowanie.

Prosty skrypt wywołujący osadzony w jQuery, dodający informację o uploadowaniu, jak i zakończeniu procesu, wygląda następująco

```javascript
$j(document).ready(function(){
    new AjaxUpload('button2', {
        action: 'addfile.php',
        name: 'file',
        data : {},
        onSubmit : function(file , ext){
            if (ext && /^(jpg|png|jpeg|gif)$/.test(ext)){
                this.setData({
                    'key': 'Ta informacja zostanie wysłana z plikiem'
                });
                $j('#uploader .text').text('Dodawanie pliki ' + file);
            } else {
                $j('#uploader .text').text('Error: tylko pliki graficzne mogą być zapisane');
                return false;
            }
        },
        onComplete : function(file,res){
            $j('#uploader .text').text('Plik został pomyślnie dodany');
            $j('#uploader .link').append(res);
        }
    });
});
```

W pliku addfile.php dodajemy prosty upload. Wszystkie informacje jakie będzie zwracał plik addfile.php są zawarte w zmiennej res. Jeśli przygotujemy w  formularzu dodawania pliku w html obiekt następującej postaci

```html
<div id="uploader">
    <input value="Dodaj grafikę" type="button" id="button2"/><br />
    <p class="text"></p>
    <p class="link"></p>
</div>
```

to informacja o dodawaniu pliku oraz jego dodaniu oraz informacja, jaką przygotujemy dodatkowo w pliku addfile.php zostanie wyświetlona na stronie. Możliwość przekazania informacji z addfile.php niesie wiele korzyści. Możemy dzięki temu generować ścieżki do pliku z ukryciem jego miejsca. O tym może następnym razem. Miłego uploadowania.