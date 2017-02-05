---
title:  "Własne modyfikatory w SMARTY"
date:   2011-01-17 16:19:26
description: "Własne modyfikatory w SMARTY"
---

Po co nam własne modyfikatory? Otuż powód jes prosty. Jeśli chcemy wykorzystać jakieś możliwości PHP, a nie ma gotowego modyfikatora to nie mamy innego wyjścia, zwłaszcza jak potrzebną nam funkcję będziemy często wykorzystywać.

Dodanie własnego modyfikatora nie jest rzeczą skomplikowaną. Można powiedzieć, że jest wręcz banalne. Zaczynamy od dodania informacji dla SMARTY gdzie jest katalog, w którym będziemy przechowywać modyfikator. Wygląda to następująco:

```php
$tpl -> plugins_dir[] = 'smarty_plugins_path/';
```

Następnie musimy dodać do tego katalogu plik, w którym znajdzie się modyfikator nazwijmy go modifier.l.php. a w nim dodajmy wpis

```php
<?php
function smarty_modifier_l($text){
  $text = trim($text);
  $text = preg_replace("@\s+@","_",$text);
  return $text;
}
?>
```

Jest to modyfikator o nazwie l, którego zadaniem jest zamienienie spacji na podkreślnik dla danego tekstu. Użycie zbudowanego w ten sposób modyfikatora wygląda następująco

```
{$text|l}
```

I to tyle. Owocnego programowania.