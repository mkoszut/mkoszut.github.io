---
title:  "Generowanie grafiki przy pomocy imagecopyresampled"
date:   2010-12-27 11:54:13
description: "Generowanie grafiki przy pomocy imagecopyresampled"
---

Tym razem będzie coś o czym wspominałem w poprzednim wpisie, czyli o generowaniu obrazów przy pomocy funkcji imagecopyresampled dostępnej w PHP. Pierwszym i podstawowym pytaniem jest pytanie co nam daje zastosowanie tej funkcji? Można wykorzystać ją w dwuch celach. Pierwszy to skalowanie grafiki, a co za tym idzie zmiejszanie jej wagi. Jest to przydatne w momęcie, gdy dajemy użytkownikom możliwość dodawania grafik i chcemy prezentować ich miniatury. Druga możliwość to ukrywanie rzeczywistego położenia grafiki w struktórze katalogów, ewentualnie generowanie grafik na podstawie danych przechowywanych w bazach, co w obecnej chwili jest bardzo praktycznym pomysłem.

Jak już wiemy do czego możemy wykorzystać dane nam narzędzie przejdzmy do konkretów. Zaprezentuje tutaj struktórę pliku akcji potrzebnego do wygenerowanie grafiki oraz metody w klasie zarządzającej generowaniem.

Plik akcji generujący grafikę na podstawie grafiki znajdującej się w katalogu użytkownkia ma postać:

```php
require_once  'Images.class.php';
$imgid = $_REQUEST['imgid']; 
$q = $_GET['q'];
$scale=100;
$width=700;
$height=600;
if(isset($_REQUEST['p'])) $scale = $_REQUEST['p'];
if(isset($_REQUEST['w'])) $width = $_REQUEST['w'];
if(isset($_REQUEST['h'])) $height = $_REQUEST['h'];
if(file_exists('katalog_uzytkownika'.Images::imgIdToName($imgid))){
  $img = new Images($imgid);
  if(!$img->sendFileToBrowser($q,$scale,$height,$width)) echo " "; 
}
```

Plik ten sprawdza czy istnieje grafika, którą chcemy wygenerować oraz wcześniej ustala standardowe paramety wywołania dla metody klasy Images zarządzającej grafikami. Jak widać jego struktóra jest bardzo prosta i chyba nic więcej nie trzeba tłumaczyć. Teraz zaprezentuję prostą klasę Images stworzoną do obsługi grafik. metoda __construct bobiera z bazy informacje o pliku znajdującym się w katalogu użytkownika. Reszta metod jest bazpośrednio związana z generowanie grafiki dla przeglądarki. Klasa Images ma postać:

```php
class Images{
  public $imgId;
  private $userId;
  private $name;
  private $path;
  private $sqlDB;
  private $image;
  private $type;

  public function __construct($imgid){
    $this->sqlDB = mysql_connect(dane serwera mysql do polaczenia);
    if($this->sqlDB){
      if(mysql_select_db(nazwa serwera, $this->sqlDB)){
        $query = mysql_query("select imageName, imageDes from user_image where imageId='$imgid'");
        $data = mysql_fetch_object($query);
        $this->imgId = $imgid;
        $this->userId = $userid;
        $this->name = $data->imageName;
        $this->path = '../profiles/'.$userid.'/images/'.$data->imageName.'';
        $info = getimagesize($this->path);
        $this->type = $info[2];
        if( $this->type == IMAGETYPE_JPEG ) {
          $this->image = imagecreatefromjpeg($this->path);
        } elseif( $this->type == IMAGETYPE_GIF ) {
          $this->image = imagecreatefromgif($this->path);
        } elseif( $this->type == IMAGETYPE_PNG ) {
          $this->image = imagecreatefrompng($this->path);
        }
        unset($this->sqlDB);
      }
    }
  }

    /*
     * Wysyla wygenerowany plik graficzny do przegladarki
     * @param $q - opcja rozwojowa dla zmiany kolorystyki, np w sepii, szrosciach
     * @param $scale - skala grafiki w procentach
     * @param $height - wysokosc grafiki w pikselach
     * @param $width - szerokosc grafiki w pikselach
     * @return graphics
     */
    
    public function sendFileToBrowser($q='l',$scale=100,$height=600,$width=700){
        //jesli szerokosc jest ustawona na 800 
        //to zdjecia szersze skaluje do 800 
        //a wezsze pozostawia takie jak sa chyba 
        //ze sa wyzsze od 600 to wtedy skaluje je do 600
        $q = strtolower($q);
        if ($q=='l') {
            $imgw = imagesx($this->image);
            $imgh = imagesy($this->image);
            if($width!=800){
                if($scale!=100){
                    $width = $imgw * $scale/100;
                    $height = $imgh* $scale/100;
                }
                if($height!=600){
                    $ratio = $height / $imgh;
                    $width = $imgw*$ratio;
                }
                if($width!=700){
                    $ratio = $width / $imgw;
                    $height = $imgh * $ratio;
                }
            } else{
                if($imgw < 800 && $imgh < 600){
                    $width = $imgw;
                    $height = $imgh;
                } else {
                    if($imgw > 800){
                        $ratio = $width / $imgw;
                        $height = $imgh * $ratio;
                    } else{
                        $height = 600;
                        $ratio = $height / $imgw;
                        $width = $imgh * $ratio;
                    }
                }
            }
            $new_image = imagecreatetruecolor($width, $height);
            imagecopyresampled($new_image, $this->image, 0, 0, 0, 0, $width, $height, $imgw, $imgh);

            $this->image = $new_image;

            switch ($this->type) {
                case IMAGETYPE_JPEG: imagejpeg($this->image); break;
                case IMAGETYPE_GIF: imagegif($this->image);
                default: imagepng($this->image); break;
            }

            $this->image = self::addHeader($this->image,$this->type);
            $this->image = self::send($this->image);
            return true;
        }
    }
    
    /*
     * Dodaje nagłuwek html do generowanej grafiki
     */
    private static function addHeader($image,$type){
        switch ($type){
            case IMAGETYPE_JPEG: $type='image/jpeg'; break;
            case IMAGETYPE_GIF: $type='image/gif'; break;
            default: $type='image/png'; break;
        }

        header('Content-Type:'.$type);
        header('Content-Transfer-Encoding: binary');
    }
    
    /*
     * Otwiera plik grafiki z opcjami odczytu
     */
    private static function send($image){
		$fp = fopen($image, 'rb');
                rewind($fp);
		fpassthru($fp);
                fclose($fp);
    }
}
```

Myślę, że to tłumaczy już wszystko. Opisy znajdujące się powyżej metod powinny wyjaśniać ewentualne wątpliowści, a jakby się jakieś pojawiły ro najlepiej skonsultować je z manualem PHP. Miłej zabawy.