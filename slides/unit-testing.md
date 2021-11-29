---
marp: true
_class: lead
paginate: true
---

# BTTR Tests

---

## ~~Waar gaan we het niet over hebben~~

* ~~het feit dat deze presentatie een dikke week te laat gegeven wordt :ok_hand:.~~

---

## Waar gaan we het ~~wel~~ over hebben

* Het nut van automatische tests;
* Korte inleiding automatisch testen;
* De subtiele kunst van het unit testen;
* Jullie ingezonden vragen.

---

## Wat zijn automatische tests en waarom schrijven we ze?

* Vast te stellen dat de implementatie werkt en voldoet aan de producteisen;
* Regressie tegen gaan;
* Als een middel om betere architectuur te bewerkstelligen;
* Ter documentatie en kennisdeling.
* Om de implementatie uit te denken/werken;

<!--
4. Verder toelichten

5. Die laatste zal niet voor iedereen van toepassing zijn. Wanneer je iets moet opzetten wat je al een hele tijd niet meer gedaan hebt of iets compleet nieuws aan het onderzoeken bent is het lastig om test-driven te werken.
-->

---

## Kortom, zodat we 's nachts beter slapen en overdag comfortabeler werken

---

## De verschillende typen tests

* Unit
* Functional
* Integration
* End-to-end
* Acceptance
* Performance
* Smoke
* Stress

<!--
Veel verschillende categorieën tests
Beperken zich allemaal tot het testen van verschillende facetten van een applicatie (invalshoeken)
Bovenste vier voor ons het belangrijkste
Performance en stress tests spreken voor zich denk ik
Smoke is een vorm van aftastende test. Eerst kijken waar vuur is, dan eventueel verdere, mnder betrekkelijke tests uitvoeren.
-->

---

## Wij beperken ons tot deze en in het bijzonder de Unit Test

* Unit
* Functional
* Integration
* End-to-end

---

## Unit tests

Hebben een hele nauwe focus en tonen aan dat hetgeen onder test (vaak één object) zich correct gedraagd.

---

<!--
backgroundImage: url("https://media.giphy.com/media/erq0p23IX7NUbYN0oD/giphy.gif")
-->

<!--
Grondig

bewijzen, logisch en volledig, dat het object onder test zich correct gedraagt.
-->

---

<!--
backgroundImage: url("https://cdn.runningshoesguru.com/wp-content/uploads/2017/06/Stability-Running-Shoes-1110x650.jpg")
-->

<!--
Stabiel

Moeten niet kapot gaan als er een implementatie detail verandert.
-->

---

<!--
backgroundImage: url("https://media.giphy.com/media/l3vRhLNLIKxdypg7m/giphy.gif")
-->

<!--
Snel

Als ze niet snel zijn dan gaat niemand ze runnen :thinking:.
-->

---
<!--
backgroundImage: url("https://media.giphy.com/media/l41lMMzTCBvXqtEUU/giphy.gif")
-->

<!--
Spaarzaam
Maximaal zoveel tests als nodig om te bewijzen dat het object zich correct gedraagt.
-->

---

## Integration tests

<!--
backgroundImage: none
-->

Specifiek de interactie tussen verschillende objecten wordt getest :drum:

<!--
Deze tests zijn relatief snel. Zeker met correct gebruik van test doubles (nep data en stuntmannen).
Stellen vast dat de integratie :drums: tussen bepaalde objecten werkt zoals bedoeld.

vb: je bouwt een koppeling met een externe API en de applicatie. De integratie test stelt vast dat de koppeling werkt en de externe API de juiste data teruggeeft.
-->

---

## Functional tests

Hier ligt de nadruk meer op de correcte implementatie van de 'business rules'.

<!--
Deze tests zijn nagenoeg even snel. Zeker met correct gebruik van test doubles (nep data en stuntmannen).

Hierbij gaat het dus om het verifieren van bepaalde functionaliteit van een applicatie.
Functional tests worden ook wel black box tests genoemd omdat je zoveel mogelijk implementatie details buiten beschouwing laat.
Black box testing is DE manier om je tests zo robuust mogelijk te houden.

vb: wanneer gebruikers van jouw super innovatieve todo app 1000 todo's hebben afgerond moeten ze een niveau omhoog gaan en willen we dat er een confetti kanon afgevuurd wordt in de UI.

Je test de specifieke uitkomst, niet sec de interactie tussen de Todo, Progress en UI componenten.
-->

---

## End-to-end tests

Bootsen het gedrag van de eindgebruiker na aan de hand van de volledige applicatie.
Ze verifiëren dat bepaalde gebruiksscenario’s werken naar behoren.

<!--
Deze vorm van testen is super waardevol maar ook erg langzaam. Zeker in de context van communicatie netwerken waarbij ze te maken hebben met netwerk vertragingen (latency). Het wordt meestal aangeraden om end-to-end tests te schrijven voor belangrijke onderdelen van de applicatie maar hoofdzakelijk te varen op unit en integratie tests.

Er zijn uitzonderingen, bijvoorbeeld wanneer je een legacy applicatie in een test harnas probeert te krijgen. In dat geval zijn end-to-end tests een onmisbare tool.
-->

---

<!--
backgroundImage: url("https://media.giphy.com/media/Oj5w7lOaR5ieNpuBhn/giphy.gif")
-->

---

<!--
backgroundImage: none
-->

## Oké, we zitten op dezelfde lijn

---

## Nu, wat maakt een goede test?

Samengevat, doe dit:

| Bericht  | Query                                    | Command                                          |
| -------- | ---------------------------------------- | ------------------------------------------------ |
| Inkomend | Assert het resultaat (test de interface) | Assert directe en publieke side effects          |
| Zichzelf |                                          |                                                  |
| Uitgaand |                                          | Expect dat er een bericht wordt verzonden (mock) |

---

<!--
backgroundImage: url("https://media.giphy.com/media/3oEduS8IhBAMURbhYY/giphy.gif")
-->

---

<!--
backgroundImage: none
-->

## Iets minder kort samengevat

* Zo spaarzaam mogelijk testen van de (publieke) interface van een object;
* Implementatie details (zoveel mogelijk) buiten beschouwing laten;
* Test doubles (mocks, spies, fake timers);
* De juiste zaken testen en niet meer dan dat.

---

## Verschillende soorten berichten en hun oorsprong

1. Het object ontvangt berichten van anderen (inkomend)
2. Verstuurt berichten naar anderen (uitgaand)
3. Het object verstuurt de berichten naar zichzelf (intern)

<!--
Waar het in de praktijk vaak op neerkomt wanneer een test suite te fragiel is dat mensen graag hun werk goed en grondig
willen doen met als resultaat dat ze veel te veel tests schrijven. Onnodige tests. Fragiele tests.

Dat hoeft niet zo te zijn wanneer we internaliseren wat er in de test matrix staat.

Een object dat software architectonisch goed in elkaar zit kun je testen door puur te kijken naar de berichten die erin gaan en eruit komen. Met andere woorden, naar de input en output van publieke methoden. Uitgangspunt is ook hier om zoveel mogelijk als een zwarte doos te behandelen. Dit zorgt ervoor dat tests minder gebonden zijn aan implementatie details met als resultaat dat ze minder broos worden.

De berichten: inkomend, intern en uitgaand zijn weer onder te verdelen in twee categorieën:
-->

---

## Queries en Commands

* Queries zijn methode die iets returnen maar niks veranderen (geen side effects).
* Commands zijn methode die niets returnen maar wel veranderen (wel side effects).

<!--
Soms kunnen we geen onderscheid maken tussen de twee. Een klassiek voorbeeld is het poppen van een item van een array. Dit is zowel een command als een query.

De veelgebruikte fluent syntax in Laravel is ook een voorbeeld van een context waarin dat onscheid vaag is.

In dergelijke gevallen test je zowel het query als het command gedeelte.
-->

---

## Test inkomende Queries door middel van assertions op de output ervan

<!--
Dus dat wat ze teruggeven. Returnen.
-->

---

```php
class Wheel
{
  public function __construct($rim, $tire) {
    $this->rim = $rim;
    $this->tire = $tire;
  }

  public function diameter() {
    return $this->rim + ($this->tire * 2);
  }
}
```

<!--
vb: We hebben een wheel object welke een publieke methode heeft om de diameter van het wiet te queryen.
-->

---

```php
class WheelTest extends TestCase
{
    public function test_it_calculates_the_diameter()
    {
      $wheel = new Wheel(26, 1.5);

      $this->assertEquals($wheel->diameter(), 29);
    }
}
```

<!--
Voor degene die onbekend zijn met PHPUnit, dit is een PHPUnit testcase.
PHPUnit voert de code in alle methodes welke beginnen met de naam test_ uit en bepaald of de tests zijn geslaagd of niet.

Vanuit de context van de TestCase bezien is een call naar $wheel->diameter() een inkomende query.

We testen dus de return value van de aanroep van de methode.
-->

---

```php
class Gear
{
  public function __construct($chainring = 0, $cog = 0, $wheel = 0) {
    #...
  }

  public function gear_inches() {
    return $this->ratio * $this->wheel->diameter();
  }

  private function ratio() {
    return $this->chainring / $this->cog;
  }
}
```

<!--
een ander vb:

De gear class heeft een publieke methode om gear_inches te berekenen. Een methode om te bepalen welke afstand een fiets af kan leggen per omwenteling.

Gear gebruikt $wheel->diameter() (een uitgaande query).
-->

---

```php
class GearTest extends TestCase
{
    public function test_it_calculates_gear_inches()
    {
      $gear = new Gear($chainring: 52, $cog: 11, $wheel: new Wheel(26, 1.5));

      $this->assertEquals($gear->gear_inches(), 137.1);
    }
}
```

<!--
Ook deze methode testen we op dezelfde manier omdat $gear->gear_inches() een inkomende query is in de context van de TestCase.

We asserten wederom de return value van de aanroep van de interface.
-->

---

## Oftewijl, test de interface, niet de implementatie

<!--
Als we alleen de interface testen, dan betekent dat dat we de implementatie kunnen veranderen, zonder dat we de tests kapot maken.
-->

---

## Test inkomende Commands door assertions op de public side effects

---

```php
class Gear
{
  public function __construct($chainring = 0, $cog = 0, $wheel = 0) {
    #...
  }

  public function setCog(Cog $cog) {
    $this->cog = $cog;
  }
}
```

<!--
Ik heb de Gear class van hiervoor een beetje aangepast en een setCog() command methode toegevoegd.
-->

---

```php
class GearTest extends TestCase
{
    public function test_setting_new_cog_size()
    {
      $gear = new Gear($chainring: 52, $cog: 11, $wheel: new Wheel(26, 1.5), $observer);

      $gear->setCog(27);

      # assert directe of publieke side effect
      $this->assertEquals($gear->cog, 27);
    }
}
```

---

### Test private methodes, oftewel berichten verzonden aan zelf, simpelweg niet

---

```php
class Gear
{
  public function __construct($chainring = 0, $cog = 0, $wheel = 0) {
    #...
  }

  public function gear_inches() {
    return $this->ratio * $this->wheel->diameter();
  }

  private function ratio() {
    return $this->chainring / $this->cog;
  }
}
```

<!--
Een voorbeeld:

De private methodes zijn een implementatie detail en mogen veranderen zolang de publieke interface blijft werken zoals hij deed.
-->

---

## Test uitgaande queries, net als private berichten, simpelweg niet

<!--
De uitgaande query van het ene object is de inkomende query van de andere. Het testen van de inkomende query volstaat.
-->

---

## Test uitgaande commands door te expecten dat ze verzonden worden

---

```php
class Gear
{
  public function __construct($chainring = 0, $cog = 0, $wheel = 0, Observer $observer) {
    #...
    $this->observer = $obs;
  }

  public function setCog(Cog $cog) {
    $this->cog = $cog;
    $this->observer->changed($chainring, $cog);
  }

  ...
}
```

<!--
Het verzenden van een changed event naar de Observer MOET gebeuren. Voor de Observer is changed een incoming command en die zullen we dan ook als zodanig moeten
testen maar de Observer weet niet Objecten er allemaal van hem afweten. Het is dus aan de class die het changed event (command) verzend om te verifieren dat dit
event verzonden wordt.
-->

---

```php
class GearTest extends TestCase
{
    public function test_saves_changed_cog_to_the_database()
    {
      $observer = new Observer();
      $gear = new Gear($chainring: 52, $cog: 11, $wheel: new Wheel(26, 1.5), $observer);

      $gear->setCog(27);

      # assert something about the state of the database
      $latest = Gear::latest()->first();
      $this->assertEquals($latest->cog, 27);
    }
}
```

<!--
Dit is geen unit test maar een integration test. De test is afhankelijk van een side effect ergens in de keten van objecten verantwoordelijk voor het communiceren met de database.

De vraag die je jezelf moet stellen in dit geval is: 'is het de verantwoordelijkheid van de Gear class om te testen of de wijziging wel goed is doorgevoerd in de database?'

Het enige wat Gear moet doen is testen of het bericht wel wordt verzonden.
-->

---

```php
class GearTest extends TestCase
{
    public function test_when_a_cog_changes_it_notifies_observers()
    {
      $spy = Mockery::spy('Observer');
      $gear = new Gear($chainring: 52, $cog: 11, $wheel: new Wheel(26, 1.5), $observer: $spy);

      $gear->setCog(27);

      $spy->shouldHaveReceived()->changed($gear->chainring, $gear->cog)->once();
    }
}
```

---

## Nog een keer, de test matrix

| Bericht  | Query                                    | Command                                          |
| -------- | ---------------------------------------- | ------------------------------------------------ |
| Inkomend | Assert het resultaat (test de interface) | Assert directe en publieke side effects          |
| Zichzelf |                                          |                                                  |
| Uitgaand |                                          | Expect dat er een bericht wordt verzonden (mock) |

<!--
Door je enkel te beperken tot het testen van deze berichten, op de manier zoals hier beschreven, bespaar je jezelf een hoop
onnodige tests en maak je je test suite een stuk stabieler.

Deze lessen zijn ook toepasbaar wanneer je een integration of feature test schrijft zij het binnen een grotere context.
-->

---

## De praktijk is weerbarstig

Zeker wanneer classes niet goed voldoen aan het Single Responsibility Principe:

> Every module, class or function in a computer program should have responsibility over a single part of that program's functionality, and it should encapsulate that part.

Tegelijkertijd is dat precies de reden waarom het zo waardevol is te doen.

<!--
Zoals eerder gezegd wordt je met de neus op de feiten gedrukt wanneer je een unit test schrijft. Je zal sneller geneigd zijn om een class op te breken omdat deze teveel afhankelijkheden heeft.

Maar wordt in de context van Laravel bijvoorbeeld bewust van de impact van Facades op de code en de afhankelijkheid aan de service container die je daarmee creeërt.
-->

---

## Feature tests in de context van Laravel

Laravel's service container werkt aardig efficient.

<!--
Hoewel het goed is om jezelf bewust te zijn van bepaalde valkuilen kun je in de gemiddelde Laravel applicatie prima uit de voeten met feature tests.

Om een voorbeeld te geven: de gehele test suite van bidfood-connect, welke voor nagenoeg 100% bestaat uit feature tests van de APIs en dus ook gebruik maken van Rebing, is lokaal binnen 8 seconden klaar. Geen reden om die niet gewoon constant te hebben draaien om te kijken of er iets stuk is gegaan.
-->

---

## Dan rest mij nog het beantwoorden van de laatste onbehandelde vraag

---

## Hoe weet je dat je geen test bent vergeten te schrijven?

Kun je dat meten met code coverage?

<!--
Ja, dat kan. Code coverage gebruikt bepaalde algoritme om te bepalen of alle mogelijke paden door de code zijn bewandeld door de test suite.

Function coverage
Statement coverage
Edge coverage
Branch coverage
Condition coverage
-->

---

## Fin

<!--
Ik hoop dat deze theoretische introductie nieuwe inzichten heeft opgeleverd en eventuele vragen weg heeft genomen.
In een aparte video ga ik veder in op het schrijven van unit/component tests voor Vue aan de hand van Jest en Vue-Test-Utils.
Die zal wat meer hands on zijn.

Stel eventuele vragen gerust dan zal ik ze minimaal 3 seconden later beantwoorden, haha.
-->
