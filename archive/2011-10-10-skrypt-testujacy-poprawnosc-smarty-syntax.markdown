---
title:  "Skrypt testujący poprawność SMARTY syntax"
date:   2011-10-10 13:58:14
description: "Skrypt testujący poprawność SMARTY syntax"
---

Tym razem coś o SMARTY. Prosty, może nie najwydajniejszy i nie maksymalnie zoptymalizowany, robiący pewne rzeczy tak a nie inaczej bo nie chciało mi się przebijać przez dokumentację i kod źródłowy, jako komunikat wyrzucający fatala pehapa powodującego przerwanie skryptu, jednak posiadający niesamowitą zaletę - działa. Przydatny w momencie gdy wprowadzamy wiele zmian w templatkach i nie chce nam się dokładnie testować, nie mamy takiej możliwości bądź nie mamy na to czasu.

Skrypt sprawdza tylko poprawność syntaxu, wywołujemy go z linii komend, a wygląda tak:

```php
<?php
$tplFiles = array();
$dirPath = '/sciezka/do/katalogu/w/ktorym/bedziemy/kompiloac/plus/minimum/cztery/znaki';
$exceptionFiles = array('/sciezka/do/katalogu/z/templatkami/nazwa.tpl');
exec('find /sciezka/do/katalogu/z/templatkami/ -name \*.tpl', $tplFiles);
$smarty = new Smarty();
foreach ($tplFiles as $filePath) {
  if (!in_array($filePath, $exceptionFiles)) {
    $smarty->_compile_resource($filePath, $dirPath);
  }
}
```
Proste, łatwe i przyjemne.