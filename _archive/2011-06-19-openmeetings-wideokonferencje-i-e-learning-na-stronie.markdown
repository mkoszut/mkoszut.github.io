---
title:  "OpenMeetings - wideokonferencje i e-learning na stronie"
date:   2011-06-19 16:14:43
description: "OpenMeetings - wideokonferencje i e-learning na stronie"
---

Tym razem coś o wideokonferencjach. Ciekawą, a zarazem otwartoźródłową aplikacją dającą nam takie możliwości jest OpenMeetings. OpenMeetings jest napisana w Javie. Daje nam możliwość prowadzenia wideokonferencji, czatu i dodatkowo udostępnia tablicę, na której użytkownicy mogą wykonywać różne operacje, z wyświetleniem slideshow włącznie. OpenMeetings mimo wielu zalet nie jest pozbawiony wad. Jego główną wadą jest laypot. Jest dosyć prosty, a do dyspozycji mamy tylko kilka szablonów kolorystycznych.

Instalacja na Ubuntu jest bez problemowa, mimo że opis znajdujący się na stronie projektu jest po hiszpańsku, Google Tłumacz daje radę.

Nie małym wyzwaniem jest integracja z naszą stroną. Jednak i to nie jest bardzo skomplikowane. Mi z pomocą przyszła paczka przygotowana dla Moodle. Do integracji potrzebna jest biblioteka nusoap, zbiór funkcji dostarczanych przez autora projektu oraz funkcje z pliku openmeetings_gateway.php, które poddamy drobnym modyfikacjom.

Rozpocznijmy od wspomnianych już modyfikacji. Modyfikujemy funkcję openmeetings_loginuser z pliku openmeetings_gateway.php pozbywając się z niej deklaracji zmiennych globalnych Moodle oraz dodajemy adres IP serwera oraz port, na którm OpenMeetings jest dostępny. Dzięki czemu otrzymujemy taką postać funkcji

```php
function openmeetings_loginuser() {
  $client_userService = new nusoap_client('http://'.adres_ip_serwera.':'.numer_portu.'/openmeetings/services/UserService?wsdl', "wsdl");
  $client_userService->setUseCurl(true);
  $err = $client_userService->getError();
  if ($err) {
    echo 'Constructor error' . $err;
    echo 'Debug' . htmlspecialchars($client->getDebug(), ENT_QUOTES);
    exit();
  }
 
  $result = $client_userService->call('getSession');
  if ($client_userService->fault) {
    echo 'Fault (Expect - The request contains an invalid SOAP body)'; print_r($result);
  } else {
    $err = $client_userService->getError();
    if ($err) {
      echo 'Error' . $err;
    } else {
      $this->session_id = $result["return"]["session_id"];
      $params = array(
        'SID' => $this->session_id,
        'username' => 'login_administratora',
        'userpass' => 'haslo_administratora'
      );
 
      $result = $client_userService->call('loginUser',$params);
      if ($client_userService->fault) {
        echo 'Fault (Expect - The request contains an invalid SOAP body)'; print_r($result);
      } else {
        $err = $client_userService->getError();
        if ($err) {
          echo 'Error' . $err;
        } else {
          $returnValue = $result["return"];
        }
      }
    }
  }
  if ($returnValue>0){
    return true;
  } else {
    return false;
  }
}
```

Idąc tym tropem w podobny sposób modyfikujemy funkcje openmeetings_createroomwithmod oraz openmeetings_setUserObjectAndGenerateRoomHashByURL. Chodzi o to żeby zmienić adres IP oraz numer portu. Teraz w prosty sposób możemy stworzyć nowy pokuj, a następnie udostępnić go wybranemu przez nas użytkownikowi. Tworzenie pokoju wygląda tak.

```php
$openmeetings_gateway = new openmeetings_gateway();
if ($openmeetings_gateway->openmeetings_loginuser()) {
  $roomid = $openmeetings_gateway->openmeetings_createroomwithmod('nazwa pokoju', TRUE);
}
```

Udostępnienie tego pokoju użytkownikowi jest równie proste. Plik wywołujący pokój będzie miał taką postać:

```php
$openmeetings_gateway = new openmeetings_gateway();
if ($openmeetings_gateway->openmeetings_loginuser()) {
 
  $returnVal = $openmeetings_gateway->openmeetings_setUserObjectAndGenerateRoomHashByURL('login','imie','nazwisko','0','adres@mail.com','id_uzytkownika','moodle','id_pokoju','0 dla użytkownika, 1 dla moderatora');
 
  $iframe_d = 'http://'.adres_ip_serwera.':'.numer_portu.'/openmeetings/?' .<br>  "secureHash=" .  $returnVal .
  "&scopeRoomId=16";
 
  printf("<iframe src='%s' width='%s' height='%s' />",$iframe_d,"100%",640);
 
} else {
  echo "Could not login User to OpenMeetings, check your OpenMeetings Module Configuration";
  exit();
}
```

Jak widać nie jest to jakoś specjalnie skomplikowane, ale może komuś ułatwi życie i oszczędzi czasu. 