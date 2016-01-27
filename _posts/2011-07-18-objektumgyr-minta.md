---
layout: post
title: Objektumgyár minta
created: 1311016340
comments: true
categories: [zend]
---
Először is elnézést a cím miatt, de talán kis figyelmet kapok miatta a szakma jeles képviselőitől, akik nem értik, hogy mit akar ez jelenteni. A "Factory Pattern"-ről van szó, egy osztályszervezési mintáról, melyet modern programtervezéskor használunk és magas szintű nyelveken valósítunk meg (Például Java, C# és kicsit bátortalanul megemlítem a PHP-t is, merthogy most arról van szó a Zend certificate általam önképzéssel megvalósuló megszerzése céljából)

Tehát így kerül a csizma az asztalra, miután ezen túl vagyunk, csapjunk a leves közepébe. A Gang of Four: Design patterns művében legtöbbször <em>Factory Method</em> és <em>Abstract Factory</em> címszóval hivatkozik erre az objektumorientált programtervezési mintára. A továbbiakban szem előtt tartom, hogy egy szoftveren nem egyedül dolgozunk, ezért a másik fejlesztő által írt kódot, ami az én osztályaimat használja, kliens kódnak fogom nevezni. Tehát az itt tárgyalt mintának a kliens kódja szempontjából az az előnye, hogy anélkül tudja példányosítani egy belső osztályomat, hogy komolyabban kellene tanulmányoznia azt. Ezzel komoly munkaórák spórolhatók és átláthatóbb programkódot eredményez a használata.

A legegyszerűbb módja ennek, ha van egy factory osztályunk, annak pedig egy olyan eljárása, ami egy paraméter alapján "legyártja" a kívánt objektumot.

Ha például egy online fizetési módot szeretnénk megvalósítani PHP segítségével, az így fog kinézni:

<code class="php">
class Payment {
  public $data;
  function __constructor($data) {
    $this->data = $data;
  }
  function process() {}
  function getResult() {}
}

class CreditCard extends Payment {}
class PayPal extends Payment {}

class PaymentFactory {
  const CREDIT_CARD = 1;
  const PAYPAL = 2;
  function createPayment($data) {
      switch ($data['type']) {
      case CREDIT_CARD : 
        return new CreditCard($data);;
      case PAYPAL :
        return new PayPal($data);
      default :
        return new Payment($data);
      }
  }
}

// kliens kód
$data = $_POST;
$pay = PaymentFactory::createPayment($data);
$pay->process();
</code>

Persze ez egy nagyon kigyomlált kód az érthetőség kedvéért, validálás nélkül használja a $_POST változót, de az majd egy másik történet lesz. Mint látható az, hogy melyik fizetési mód lesz példányosítva egy felhasználótól érkező form alapján dől el és ez meghatározhatja a következő lépést is, például a kártyaadatok bekérése vagy az átirányítást a PayPal fizetőoldalára. A kliens kódnak csak arról kell tudnia, hogy a form milyen értékeket adhat át az asszociatív tömb "type" részeként. Jelen esetben 1-et vagy 2-t, de tulajdonképpen ez a kód nagyon rugalmas, kibővíthető több fizetési móddal is. Továbbgondolva egyetlen hátránya, hogy a sok leaf osztály miatt nagyon megnőhet az a switch blokk és ugyan nem valósult meg kódduplikáció, de mégis szeretem kerülni a túl sok if-et és switch-et, ha objektumorientált programozásról beszélek.

Nézzük, mi van akkor, ha a kliens számára biztosítani szeretnénk, hogy a saját osztályait gyárthassa a saját feltételei alapján? Erre találták ki az <strong>Abstract Factory</strong> mintát. Az alábbi példa egy kártyát és egy virtuális fizetőeszköz implementálását várja el a kliens osztályától.

<code class="php">
abstract class AbstractPaymentFactory {
  abstract function getCardPayment();
  abstract function getVirtualPayment();
}
interface PaymentInterface { function process(); }

class AmazonPaymentFactory extends AbstractPaymentFactory {
  function getCardPayment() {
    return new AmazonCreditCardPayment();
  }
  function getVirtualPayment() {
    return new AmazonMoneyBookersPayment();
  }
}

class EbayPaymentFactory extends AbstractPaymentFactory {
  function getCardPayment() {
    return new EbayCreditCardPayment();
  }
  function getVirtualPayment() {
    return new EbayPaypalPaypent();
  }
}

// a fizetési módok osztályai
class EbayCreditCardPayment implements PaymentInterface { function process() {} }
class EbayPaypalPaypent implements PaymentInterface { function process() {} }
class AmazonCreditCardPayment implements PaymentInterface { function process() {} }
class AmazonMoneyBookersPayment implements PaymentInterface { function process() {} }

// példányosítás
$factory = new AmazonPaymentFactory();
if($_POST['type'] == 1) {
  $pay = $factory->getCardPayment();
} else {
  $pay = $factory->getVirtualPayment();
}
$pay->process();
</code>

Ha olyan vagy mint én és szereted rendben tudni a kódodat, akkor most megszólalt a szirénád, mivel sokféle osztály esetén rengeteg kód duplikáció keletkezik fölöslegesen. A következő minta, ami megoldja ezt a problémát, a <strong>Decorator Pattern</strong>, de azt majd egy következő bejegyzésben...

Figyeljük meg, hogy mivel a PHP nem képes a visszatérési értékek type casting-jára, nem tudjuk elvárni a klienstől, hogy a getCardPayment() és getVirtualPayment() visszatérési értékei a PaymentInterface egyik megvalósítása legyen. Ez a nyelv hibája, amit az osztályunk dokumentációjával kell kompenzálnunk és megkövetelnünk, hogy minden esetben ilyen osztállyal tárjen vissza a kliens megvalósítása is.
