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

* Soorten tests;
* Wat maakt een goede test?;
* De subtiele kunst van het Vue testen middels Jest en Vue-test-utils;
* Laravel en PHP testing middels PHPUnit;
* Behaviour Driven Development;
* Andere hersenscheetjes;
* Jullie vragen.

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

---

## De verschillende typen tests

.. ik vergeet er vast nog een paar

---

## De verschillende typen tests

Voor deze presentatie beperken we ons tot de meest relevante, namelijk:

* Unit
* Functional
* Integration
  en
* End-to-end

---

## Unit tests

Hebben een hele nauwe focus en tonen aan dat het object zich correct gedraagd.

---

## Ze zijn

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
backgroundImage: url("https://media.giphy.com/media/bJ91q0UsnGbacudx2g/giphy.gif")
-->

<!--
Stabiel

Moeten niet kapot gaan als er een implementatie detail verandert.
-->

---

<!--
backgroundImage: url("https://media.giphy.com/media/Gpu3skdN58ApO/giphy.gif")
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

<!--
Deze tests zijn relatief snel. Zeker met correct gebruik van test doubles.
-->

Stellen vast dat de integratie tussen bepaalde objecten werkt zoals bedoeld. Specifiek de interactie tussen de objecten wordt getest.

<!--
Voorbeeld: je bouwt een adapter (denk aan van die reisstekkers) tussen een externe API en de applicatie. Dan zou een integratie test van die adapter kunnen testen of het lukt om data op te vragen middels bij de API.
-->

---

## Functional tests

<!--
Deze tests zijn relatief snel. Zeker met correct gebruik van test doubles.
-->

Een functional test is een zogenaamde 'black box' test waarbij gekeken wordt of, gegeven een bepaalde input, de applicatie de juiste resultaten teruggeeft **in lijn met de business rules**.

<!--
Daarbij gaat het dus - bovenop een correcte integratie - om het verifieren van bepaalde functionaliteit van een applicatie. Merk op dat de integratie op een impliciete manier wordt getest en implementatie details bewust buiten beschouwing worden gelaten, dat is het geheim van een goede test.
-->

---

## End-to-end tests

Bootsen het gedrag van de eindgebruiker na aan de hand van de volledige applicatie. Ze verifiëren dat bepaalde gebruiksscenario’s werken naar behoren.

<!--
Deze vorm van testen is super waardevol maar ook erg langzaam. Zeker in de context van communicatie netwerken waarbij ze te maken hebben met netwerk vertragingen (latency). Het wordt doorgaans aangeraden om end-to-end tests te schrijven voor belangrijke onderdelen van de applicatie maar hoofdzakelijk te varen op unit en integratie tests.
-->

---

<!--
backgroundImage: url("https://media.giphy.com/media/Oj5w7lOaR5ieNpuBhn/giphy.gif")
-->

---

<!--
backgroundImage:
-->

## Wat maakt nou een goede test en waarom schrijven we ze?

---

## Nou, om

* Vast te stellen dat de implementatie werkt en voldoet aan de producteisen;
* Regressie tegen gaan;
* Als een middel om betere architectuur te bewerkstelligen;
* Ter documentatie en kennisdeling.
* Om de implementatie uit te denken/werken;

Kortom:

* Zodat we 's nachts beter slapen en overdag comfortabeler werken;

<!--
4. Verder toelichten

5. Die laatste zal niet voor iedereen van toepassing zijn. Wanneer je iets moet opzetten wat je al een hele tijd niet meer gedaan hebt of iets compleet nieuws aan het onderzoeken bent is het lastig om test-driven te werken.
-->

---

## Oké, maar wat maakt een goede test dan?

Een aantal dingen:

* Zo spaarzaam mogelijk testen van de (publieke) interface van een object;
* Implementatie details (zoveel mogelijk) buiten beschouwing laten;
* Test doubles;
* De juiste zaken testen.

---

### We hebben we een hekel aan onze tests omdat

Ze langzaam zijn.
Ze zijn fragiel.
Zijn ze nog wel de moeite waard als ze langzaam en fragiel zijn?

Het hoeft niet zo te zijn en er zijn slechts een paar truukjes voor nodig om te
zorgen dat we weer van onze tests gaan houden.

Outside in TDD (london school)

Vandaag gaan we wat verder in op het schrijven van unit tests.

Berichten kunnen drie oorsprongen hebben:

1. Het object ontvangt berichten van anderen
2. Verstuurt berichten naar anderen
3. Het object verstuurt de berichten naar zichzelf

Die berichten kunnen weer onder verdeeld worden in queries en commands.

Queries returnen iets maar veranderen niks (geen side effects).
Commands returnen niks maar veranderen iets (hebben wel side effects).

Soms kunnen we geen onderscheid maken tussen de twee. Een klassiek voorbeeld is het poppen van een item van een array. Dit is zowel een command (pop) als een query omdat
de waarde die van de stack af wordt gepopt gereturned wordt.

De fluent syntax die veel gebruikt wordt in Laravel is hier ook een voorbeeld van.

---

## Inkomende Queries

Regel #1: test inkomende berichten van het type Query middels assertions van wat ze terug sturen.

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

---

## Test de interface, niet de implementatie

<!--
Als we alleen de interface testen, dan betekent dat dat we de implementatie kunnen veranderen, zonder dat we de tests kapot maken.
-->

---

## Inkomende Commands

Regel #2: test inkomende Commands door assertions op de public side effects.

```php
class Gear
{
  public function __construct($chainring = 0, $cog = 0, $wheel = 0) {
    #...
  }

  public function setCog(Cog $cog) {
    $this->cog = $cog;
  }

  ...
}
```

---

## De ontvanger van de inkomende berichten is hoofdelijk verantwoordelijk voor het asserten van directe, publieke side effects

---

### Berichten verzonden aan zelf

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
Het is een anti pattern om ook nog een expliciete test voor de private methode te maken. Niet alleen resulteert dit in visibility wijzigingen en getters/setters
die puur en alleen nodig zijn voor het schrijven van de test, het is ook nog eens overtollig.

De private methode wordt namelijk al impliciet getest door de gear_inches() test die we eerder bekeken.
-->

---

## Regel #3: Test private methodes, oftewel berichten verzonden aan zelf, simpelweg niet

<!--
Geen assertions maken van hun result/response. Geen expectations dat er iets naar ze verzonden wordt.
-->

---

## Het ene object zijn uitgaande queries zijn het andere object zijn inkomende queries

<!--
Dit betekent eigenlijk zoveel als dat dezelfde regels en dus eigenlijk ook dezelfde tests als de tests die we al hebben geschreven voldoende zijn om te valideren
dat alle methodes werken naar behoren.

Het volstaat om alleen inkomende queries te testen.
-->

---

## Regel #4: Test uitgaande queries, net als private berichten, simpelweg niet

---

## Uitgaande Commands

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
      $this->assertEquals($gear->cog, 27);
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

## Regel #5: Test uitgaande commands door te expecten dat ze verzonden worden

---

## Zie daar, de Test Matrix

<!--
Inleiden

Bij het schrijven van een goede unit test is het van belang om het object als op zichzelf staand te beschouwen. Een object dat software architectonisch goed in elkaar zit kun je testen door puur te kijken naar de berichten die erin gaan en eruit komen. Met andere woorden, naar de input en output van publieke methoden. Probeer daarbij om zoveel mogelijk als een zwarte doos te behandelen. Dit zorgt ervoor dat tests minder gebonden zijn aan implementatie details en minder broos worden (minder Nestlé Bros worden).

Deze zijn onder te verdelen in twee soorten: queries en commands. Die twee soorten zijn dan weer onder te verdelen in:

Inkomend
Uitgaand

en

Verzonden naar zichzelf
-->

| Bericht  | Query                                    | Command                                          |
| -------- | ---------------------------------------- | ------------------------------------------------ |
| Inkomend | Assert het resultaat (test de interface) | Assert directe en publieke side effects          |
| Zichzelf | negeren                                  |                                                  |
| Uitgaand |                                          | Expect dat er een bericht wordt verzonden (mock) |

---

## Zie daar, de Test Matrix

**Inkomend**
Ontvanger van het inkomende bericht is als enige verantwoordelijk voor het asserten van het resultaat.

**Zichzelf**
Breek deze regel als het schrijven van een test geld scheelt tijdens het ontwikkelen.

**Uitgaand**
Breek deze regel als het testen van side effects op een stabiele en gemakkelijke manier kan.

---

---

---

## Een snel woordje over test frameworks

Deze bieden een test runner (om tests uit te voeren :clown_face:), een assertion library en andere handige tools en methoden om je leven makkelijker te maken.

---

## Jest

Is een zogenaamd 'batteries included' test framework voor JavaScript:

* Gemakkelijk op te zetten;
* Snapshot testing;
* Code coverage;
* Volledige integratie van test doubles (mocks, spies, fake timers);
* Goede documentatie;
* Duidelijke exceptions en debugging output.

---

**Test harnesses**
Dependency Injection
Composition over Inheritance

**Tests structureren**
Setup & Teardown inline - duplication in testing is not a bad thing

**JavaScript testing**
Jest, jsdom, transforms
Vue-test-utils
localVue

**Extra**
CodeCeption (BDD testing alternatief voor PHPUnit)
Laravel Dusk Automated Browser Testing voor Laravel

BDD
Hoe vaak zou het voor ons handig zijn om eventueel automatische tests te draaien? ( dagelijks/wekelijks, bij een release, elke merge)
Hoe weet je zeker dat je geen tests vergeet te maken? Is dat met code coverage te zeggen? Zo nee, wat kun je daaraan doen?

---
