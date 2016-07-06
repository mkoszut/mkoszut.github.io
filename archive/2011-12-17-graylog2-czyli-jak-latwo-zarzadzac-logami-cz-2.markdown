---
title:  "Graylog2, czyli jak łatwo zarządzać logami cz. 2"
date:   2011-12-17 22:41:02
description: "Graylog2, czyli jak łatwo zarządzać logami cz. 2"
---

Tak jak pisałem w poprzednim wpisie tym razem będzie o przekierowaniu logów systemowych do Graylog2. A następnie o komunikacji z Graylogiem przy pomocy formatu GELF (Graylog Extended Log Format), przedstawię też kod prostego wrappera do GELF napisanego w PHP.

Aby wszystkie logi systemowe były wysyłane do Grayloga trzeba dodać odpowiednie przekierowanie tych właśnie logów w pliku konfiguracyjnym sysloga. W naszym przypadku za logi systemowe odpowiada rsyslog. Jest on standardowo instalowany na systemach debianowych. ﻿Plik konfiguracyjny znajdziemy w katalogu /etc/. Jak łatwo się domyślić jest to plik rsyslog.conf. Jeśli chcemy aby nasze logi były wysyłane protokołem UDP wystarczy dodać do niego wpis w postaci 

```bash
*.* @nazwa_hosta:port
```

a jeśli z wykorzystaniem TCP w postaci

```bash
*.* @@nazwa_hosta:port
```

Parametr *.* powoduję, że do Grayloga wysyłamy wszystkie logi. Odpowiednio go zmieniając możemy ograniczać odpowiednio źródło logów oraz poziom. Więcej na ten temat znajdziemy w dokumentacji rsysloga. Port na jakim standardowo jest uruchomiony Graylog2 to 514. Można oczywiście zmienić go w pliku konfiguracyjnym. Jest jeszcze problem wyboru protokołu jakim chcemy pluć do Grayloga. UDP ze względu na swoją prostotę wydaje się wręcz idealny. Jednak ma pewne ograniczenia, a mianowicie maksymalny rozmiar pakietu. Jeśli będziemy przesyłać duże logi (np.: error logi lighttpd) to lepiej wybrać TCP.

Skoro już wszystkie logi systemowe wysyłamy do Grayloga to dlaczego komunikatów o błędach w naszych programach czy skryptach również tam nie przesyłać. Świetnie nadaje się do tego GELF. Jest to protokół Grayloga przystosowany do przesyłania wiadomości do Grayloga. Używa się go bardzo przyjemnie i bezproblemowo. Przedstawię teraz prostą funkcję pehapową (oczywiście można wrzucić ją do klasy w zależności od naszych potrzeb) dzięki której będziemy mogli obsłużyć wyjątki czy po prostu logować istotne dla nas rzeczy.

```php
function log($message, $level = 'INFO', $include_backtrace = true){
  $gelf = new Gelf('nasz_host', 'port_dla_gelf');
  $level = strtoupper($level);
  switch ($level) { 
    case 'EMERGENCY': $numLevel = 0; break; 
    case 'ALERT': $numLevel = 1; break; 
    case 'CRITICAL': $numLevel = 2; break; 
    case 'ERROR': $numLevel = 3; break; 
    case 'WARNING': $numLevel = 4; break; 
    case 'NOTICE': $numLevel = 5; break; 
    case 'INFO': $numLevel = 6; break; 
    case 'DEBUG': $numLevel = 7; break; 
    default : $numLevel = 6; 
  } 
  $attributes = false; 
  if (is_array($message)) { 
    $attributes = $message; 
    $message = $attributes['message']; 
    unset($attributes['message']); 
    foreach ($attributes as $key => $value) { 
      $gelf->setAdditional($key, $value); 
    }
  } 
  if ($include_backtrace) { 
    $backtrace = debug_backtrace(); 
    if ($backtrace[1]) { 
      $nesting = 1; 
    } else { 
       $nesting = 0; 
    } 
    $file = $backtrace[0]['file']; 
    $line = $backtrace[0]['line']; 
    $function = $backtrace[$nesting]['function']; 
    $class = $backtrace[$nesting]['class']; 
    $message .= ' Class: '.$class.', function: '.$function; 
    $gelf->setFile($file); 
    $gelf->setLine($line); 
  } 
  if (strlen($message) > 150) { 
    $shortMessage = substr($message,0,150).'...'; 
  } else { 
    $shortMessage = $message; 
  }
  $gelf>setShortMessage($shortMessage); 
  $gelf->setFullMessage($message); 
  $gelf->setHost(gethostname()); 
  $gelf->setLevel($numLevel); 
  $gelf->setFacility('php log'); 
  $gelf->send();
}
```

Funkcja ta pozwala wysyłać informacji do Grayloga z parametrem $include_backtrace równym true przechwytuje również informacje w jakim miejscu kodu nastąpiło wydarzenie. Może się to przydać jeśli przechwytujemy wyjątki w kodzie i logujemy je do bazy. Klasa Gelf wykorzystana w tej funkcji znajduje się w oficjalnym repo pod tym adresem.