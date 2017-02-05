---
title:  "Scribd, ciekawe narzędzie do prezentacji dokumentów"
date:   2011-03-18 10:24:01
description: "Scribd, ciekawe narzędzie do prezentacji dokumentów"
---

Tym razem postanowiłem napisać o Scribd. Jest to bardzo Ciekawe narzędzie do prezentowania wszelkiego rodzaju dokumentów w sieci. Obsługuje większość popularnych formatów, zarówno OpenOffisowych, Microsoftowych, PDF, PS i jeszcze kilka innych. Scribd jest skryptem umieszczanym na stronie, jego wywołanie jest bardzo proste, ogranicza się do wklejenie kodu aplikacji. Dokumentacja dostępna na stronie jest bardzo bogata i precyzyjna. Nie zawiera jednak gotowego skryptu PHP umożliwiającego automatyczne dodawanie dokumentu, o czym właśnie zamierzam napisać.

No to zaczynamy. Wklejamy kod:

```html
<script type='text/javascript' src='http://www.scribd.com/javascripts/view.js'></script> 
<div id='embedded_flash'><a href="http://www.scribd.com">Scribd</a></div>
<script type="text/javascript">
  var scribd_doc = scribd.Document.getDoc({$sitedata.fileinfo.scribd_doc_id}, '{$sitedata.fileinfo.scribd_access_key}');
  var oniPaperReady = function(e){  
    // scribd_doc.api.setPage(3);
  }
  scribd_doc.addParam( 'jsapi_version', 1 );
  scribd_doc.addEventListener( 'iPaperReady', oniPaperReady );
  scribd_doc.write( 'embedded_flash' ); 
</script>
```

Pozostaje jeszcze problem uploadu plików na serwer, z którego korzysta Scribd. Można to rozwiązać w bardzo ładny sposób dzięki PHP z CURL-em. Kod wraz z prostą obsługą błędów będzie wyglądał następująco

```php
$curl = curl_init();
$curl_setopt($curl,CURLOPT_URL,'api.scribd.com/api?method=docs.upload');
$curl_setopt($curl, CURLOPT_POST, 1);
$curl_setopt($curl, CURLOPT_POSTFIELDS, array('api_key' => 'jakis_key_wuzytkownika', 'file' => '@sciezka_do_pliku));
$curl_setopt($curl,CURLOPT_RETURNTRANSFER,1); 
$curl_setopt($curl,CURLOPT_CONNECTTIMEOUT,5);
$scribd = curl_exec($curl);
$curl_close($curl);
$xml = new SimpleXMLElement($scribd);
if($xml->error){
  unlink('ciezka_do_pliku');
  return 'kod_bledu';
} else {
  $scribd_did = $xml->doc_id;
  $scribd_akey = $xml->access_key;
}
```

Prawda, że proste? 