Notater fra Lambda Jam
======================

Notater fra Lambda Jam 2013, i varierende grad av kompletthet og kvalitet. Beklager på forhånd for skamløs miks av norsk og engelsk.

Dag 1
=====

Data, Visibility, and Abstraction - Stuart Sierra
-------------------------------------------------

Stuart tar oss med på sin ferd gjennom programmeringspråk, og hvordan han har "oppdaget" nye abstraksjoner.

* __Basic__ -- veldig lite eksplisitt, med få abstraksjoner. Enkelt å se hva som foregår.
* __QBasic__ -- skiller mellom funksjoner og subroutines, og expressions og statements.
* __C++__ -- mange nye (distraherende) abstraksjoner, som ikke handlet om data, men om å interagere med datamaskinen.
* __Perl__ -- abstraksjoner som gjorde programmering enklere (GC, etc). En annen nyttig abstraksjon: data er represenert med noen få, enkle datastrukturer (lister, maps, etc).
* __XML__ -- XSLT har mye til felles med funksjonelle språk. (Og både kode og data er XML.) Behandlet data vha en serie transformasjoner (hvilket jo er litt lisp-aktig).
* __Java__ -- har de vanlige datastrukturene, men dataene er ikke "synlige" nok, gjemt inne i objekter. Trenger mye kode for å konvertere mellom typer.
* __Lisp__ -- gjorde det lettere å skrive kode som fungerer med "alle" datatyper (eksempelvis BFS over ulike datastrukturer).
* __RoR__ -- kombinerer gode ting fra Perl og Lisp. Gjorde det enkelt å endre på hvordan språket fungerer. Men, det blir raskt mange ulike "interne språk" når en kan endre så mye.
* __Clojure__ -- datatyper bygget over noen få enkle abstraksjoner (mye kan behandles som sekvenser, etc), hvilket gjør at en kan bruke idéer en allerede kjenner til å behandle nye typer. Og det er enkelt å lage egne typer som følger samme abstraksjon. Noe som også gjør det mindre nødvendig å lage "pidgin languages" for å få gjort ting.
* "Problem" med Clojure: Funksjoner som utfører sideeffekter gir ikke nødvendigvis noen fornuftig returverdi. Begynte i stedet å lage funksjoner som tok representasjon av state inn, og ga la til verdier på denne ut, slik at alle "sideeffekter" kunne gjøres av én enkel funksjon til slutt. Programmet blir dermed en sekvens av datatransformasjoner, med "effekt"/output gjort til slutt. Mer komplekse program kan settes sammen av en serie slike sekvenser.
* Å representere tilstanden som en datastruktur gjør det enklere å inspisere og debugge, og gir bedre synlighet for hva som skjer.
* Hexagonal Architecture: Se på applikasjon som en separat enhet, som er koblet til GUI, tester, WS, DB, whatever, vha "adaptors". Programmet er en *data manipulator*, adaptorne håndterer "effekten".
* Gode abstraksjoner gjør det enklere å *se hva programmet gjør*. (Det er også noen store viktige abstraksjoner som gjør ting usylig (GC, eksempelvis), men disse er det ikke så mange av.)

(Simile-Free) Monad Recipes - Aditya Siram
----------------------------------------

Intro til monader i Haskell: IO, Reader, Writer, State, Monad Transfers

*Kode, kode, kode. Og et tempo som gjorde det plent umulig å skrive notater.*

Let It Crash: Erlang Fault Tolerance - Tristan Sloughter
--------------------------------------------------------

Tristan:

- Github: tsloughter
- Jobber i Heroku, skriver routing infrastruktur i Erlang

Erlang fokuserer på: Concurrency, distribution, actors, green threads (lettvekts-tråder). Men hva er vel disse verdt uten **fault tolerance**?

* I routing: Mye kan gå galt. Degradering (tregere eller dårligere respons) er alltid bedre enn å ikke gi noen respons i det hele tatt.
* I logging: Bedre om logger kommer senere, enn at de ikke kommer. Og bedre å informere om logger som mangler, enn at de ikke er der.

**Fault tolerance**: the ability to keep working to a level of satisfaction in the presence of failure.

Tre viktige konsepter

- Isolation: 
  - immutable variabler, billige prosesser, linke processer sammen
  - message passing lar oss slippe shared state
  - monitor: unidirectional; link: bidirectional
  - vi kan håndtere errors i en annen prosess enn der feilen skjedde
- Graceful Degradation:
  - GC er separert per prosess, og gjøre concurrent
  - kan gå inn og modifiser og inspisere kjørende systemer
- Recovery
  - OTP supervisors
  - Prosesser trenger en "initial state" som det er trygt å gå tilbake til

Let it Crash

- Vær forberedt på at det kommer til å krasje
- Tester dekker ikke alt, du kommer ikke til å være forberedt på alt!
- Defeinsiv kodeing suger! (Happy path er penere, kjappere og lettere kode å forstå.)

"Heisenbugs most likely source of fire"

- 1/132 bugs NOT heisenbugs
- Let it Crash often enough to fix production issues

Crash == process crash, all state lost. Men systemet skal fortette å kjøre som før.

OTP: For Erlang hva J2EE prøver å være for Java.
- gen_server: abstraherer bort vanlig pattern for å lage spawn->init->loop->exit pattern
- supervisors: prosess for å overvåke (flere) andre prosesser gitt ulike stratgier for hva som skal gjøres når en av dem feiler. Hvis overvåkede noder fortsetter å krasje kan supervisoren bytte strategi eller krasje seg selv, slik at feilen propagerer oppover.

Må tenke på:

- hva kan krasje?
- hva er trygge initial states?
- hvilke "krasj" er det ikke verdt å få beskjed om (fordi de fikser seg selv)?


Workshop: The Art of Several Interpreters, Quickly - Daniel P. Friedman, Jason Hemann
-------------------------------------------------------------------------------------

> Denne workshoppen var (som på forhånd forventet) veldig akademisk, veldig morsom (og veldig vanskelig).

Mesteparten av koden vi gikk igjennom ligger ut på GitHub her: <https://github.com/jasonhemann/lambdajam2013>

Vi startet med å gå igjennom en del øvelser der vi fikk presentert deler av en interpreter, og fikk selv muligheten til å fylle inn det som manglet.

Den siste timen var dedikert til metacircular interpreters: "interpreters interpreting interpreters interpreting .... interpreting some expression". 

Vi var også innom (og implementerte) dynamic scoping. Friedman hadde dette å si om temaet: 

> 'dynamic scope', posibly the worst combination of letters on the planet.

og

> Dynamic scope is really bad for your brain.



Systems that run forever self-heal and scale - Joe Armstrong
------------------------------------------------------------

> Morsom og interessant talk om utfordringene med å lage robuste systemer. Ble filmet, absolutt verdt å se når det blir lagt ut.

Starter med å illustrere sekvensiell programmering som én stor sirkel, mens Erlang-måten å designe feiltolerante systemer kan sammenliknes med en stor graf av små sirkler som kommuniserer med hverandre.

Tenker på erlang prosesser som folk. Vi har alle våre egne minner, kommuniserer med beskjeder, og yttre støy kan påvirke og skape utfordringer for kommunikasjon oss imellom.

Det finnes tilnærminger til å designe programmer:

1. designe for noen få brukere, og skalere opp
1. designe for MANGE brukere, og skalere opp

Sistnevnte er langt enklere!

Et av de vanskeligste problemene å løse: "acheving consistenty in data across processes"

For å sikre tilgjengelige data:
- må ha flere kopier, med ulike måter å adressere
- beregnger kan gjøres overalt!

Joe beskriver **6 regler** for å lage feiltolerante systemer:

*Isolation*

- en prosess kan ikke kræsje en annen!
- fører til en hel rekke fine egenskaper: fault-tolerance, scalability, reliability, testability, comprehensibility, code upgrade
- Erlang-prosesser er lettvekts og fullstendig isolerte

*Concurrency*

- verden er concurrent!
- mange problemer er pinlig paralelle.
- trenger MINST to maskiner for å lage et system som ikke går ned
- ingen vei utenom concurrent og distributed hvis en vil ha fault-tolerance
- Erlang prosesser er concurrent, og alle kjører (i teorien) i paralell

*Must detect failures*

- kan ikke fikse feil du ikke kan oppdage
- må fungere på tvers av maskiner; hele maskiner kan feile
- kan løses med "supervision trees" på tvers av prosesser/maskiner
- Erlang-prosesser kan linkes sammen, slik at alle dør dersom én av dem dør
- Har prosesser som "gjør arbeidet", noen som overvåker andre, og manager-prosesser som styrer det hele.

*Fault identification*

- Meldinger om at en prosess har feilet sender også med årsaken til feilen.

*Live code upgrade*

- Vil ikke ha nedetid
- "Once a system is started, we go forever"

*Stable storage*

Dag 2
=====

Functional I/O in Scala - Nilanjan Raychaudhuri
-----------------------------------------------

*Nilanjan arbeider for Typesafe, og jobber med Play.*

IO er "problembarnet" når vi prater om funksjonell programmering.

**Lazy IO**: Ønsker å behandle filer som en "collection of data". 

- Utfordring: når lukker en filen?

Wish list: composibility, scalability, and resource-safety

Løsning etter denne modellen

    producer -> transformer -> consumer
            bits           json

- Producer implementert som enumerator
- Transformer er enumeratee
- Consumer, iteratee

Iteratee implementerer fold, som tar *input* og gir en ny iteratee.
Iteratee har to tilstander, `Cont` og `Done` som forteller enumerator hvorvidt den ønsker å iterere videre.
Input til iteratoren vil være et input-element, EOF, eller Empty (dvs, "har ikke data nå, kommer mer senere.").
Transformerens jobb er å konvertere mellom iteratorer av ulike typer.

Kode ligger på <https://github.com/nraychaudhuri/iteratee-exercies>.


Semantics, clarity, and notation: the benefits of expressions over statements - Tracy Harms
-------------------------------------------------------------------------------------------

Tre konsepter som er viktig å tenke på når en skal skrive og forstå programmer.

**meaning**
**understanding**
**expression**

- meaning as a function
- program as a function

hvis

    f = g

...hvor mange funksjoner har vi?

*one function, two expressions*

Jo flere expressions vi må forholde oss til, dess vanskeligere blir det å forholde seg til.

one program, multiple forms - the form we write, and the form we run

Mening ble på slutten av 1800-tallet formalisert vha "logic as symbol manipulation".
Noe som førte til flere ulike modeller.

- lambda calculus
- turing machine
- combinatorial logic
- ...og mange mange flere

Og vi kan velge mellom ulike modeller når vi resonerer.

Expression er over ment som "uttrykk for noe". 
Kan også skille mellom statements og expressions.

- statements have effects
- expressions have values
- *og noen ting har begge deler!*

Statements er sekvensielle, tilstandsfulle og avhengige av kontekst.
Dette fører til en imperativ verden der vi må tenke på tid og "fra datamaskinens perspektiv".

Expressions are immediate and proximate. Hvis expressions ikke avhenger av muterbar tilstand kan vi evaluere dem uavhengig av tidspunkt.

*Seeking a higher level perspective*

Viktig konsept i OO: cohesion og coupling. Viktig også utenfor OO:

- Cohesion: funksjoner
- Coupling: funksjonskomposisjon

> Peter Landin, 1966: "the next 700 programming languages" (paper)

Mente språk vil bevege seg mot matematisk notasjon. Fra paperet:

    a) Each *expression* has a nesting subexpression structure,
    b) each subexpression *denotes something*,
    c) the thing an expression denotes, its value, *depends only on the values of its subexpressions*, not on other properties of them.

*interlude: live coding i J.*

Tror hovedpoenget her er at hvis vi ikke gjør oss avhengig av statements kan vi benytte oss av alternativer til den imperative modellen. Bruk av expressions gjør oss i stand til å uttrykke mening i andre modeller, slik som kombinatorisk logikk (i eksempelvis J).

> Expressions condense meaning in space, and eliminate time.

Siden programer består av formell logikk, programmerer vi kun når vi gjør endringer til denne logikken.


Functional composition - Chris Ford
-----------------------------------

Musikk generert vha modifisering av sinusbølger i Clojure. Veldig bra, og fullstendig umulig å notere fra. Se videoen.


Workshop: Program Transformations - William Byrd, Nada Amin
-----------------------------------------------------------

Dagens kvote på tre og en halv time med meget akademisk moro.
Vi lærte om "correctness-preserving transformations", der målet var å forenkle Scheme-programmer til kode det var enkelt å kompilere ned til C.

Først og viktigst var transformasjonen CPS (continuation passing style), som lar oss skrive om funksjoner som ikke er tail-rekursive til å ikke sprenge stacken når de kjøres, ved å utsette det som gjenstår som continuations.

All kode vi lekte med er tilgjengelig på GitHub: <https://github.com/namin/lambdajam>


Programming for the Expression of Ideas - Gerald Sussman
--------------------------------------------------------

"Datamaskinen har både revolusjonert hvordan vi tenker, og hvordan vi uttrykker det vi tenker."

Sussman går igjennom en rekke eksempler fra electrical engineering og fysikk (med en del veldig hardcore likninger) og viser hvordan å implementere disse ofte avslører "mangler" med de tradisjonelle representasjonene av disse idéene.

Forholdsvis avansert, ganske morsomt, og pøkk umulig å notere fra.

Dag 3
=====

The Joy of Flying Robots with Clojure - Carin Meier
---------------------------------------------------

Veldig bra start på dagen. Artig foredrag om å programmere roboter (Roomba og AR Drone) i Clojure. Høydepunkt: AR Drone og Roomba som interagerer og "danser sammen". Meningsløst å ta notater fra, men se videoen! 


Protocols, Functors and Type Classes - Creighton Kirkendall
-----------------------------------------------------------

> Opening quote: "Im gonna show a lot of code, but you're not gonna understand much of it..."

The problem: data er i ett system, vi vil bruke et bibliotek i et annet system.

- OO: wrappers and adaptoprs
- I funksjonelle språk knytter vi ikke funksjonalitet til datene. I stedet utvider vi biblioteket.

Eksempel vi ser på gjennom ulike språk: *"summere" over en datastruktur.*
Krever at vi kan a) *legge sammen* elementer (på en eller annen måte) og b) *folde* datstrukturen.

I eksempelet har vi et bibliotek for å summere over lister. Vi "utvider" biblioteket til å håndtere trær.

Går igjennom kode for dette i Clojure, OCaml, Haskell, og Scala, og sammenlikner løsningene.

Vi ser at til tross for at språkene har forskjellige features, er de grunnleggende konseptene mye like.
Det er også tydelig at det er viktig at biblioteket er planlagt med tanken om at det skal utvides.

Godt presentert, ikke for vanskelig, verdt å se.


Living in a Post-Functional World - Daniel Spiewak
--------------------------------------------------

Functional programming: 

- *ikke: programming with functions*
- functions as primary abstraction unit.

OO: programmering med objekter som primære abstraksjon

Når en får nok "ting" trenger en måter å organisere på. 
Reelle systemer har *mange* funksjoner.

>  A set of functions is not a function

Vi trenger et førsteklasses konsept for å organisere funksjonene.

**Real modules**

- Namespacing alene er ikke nok. 
  - NS løser (kun) et problem (dog, veldig godt): å løse navnkollisjoner. 
  -  Men kollisjonene er et symptom, ikke selve problemet!
- Avhengigheter burde være eksplisitt uttrykt
- Concrete binding should be deferred

I Standard ML:

- Signaturer er abstrakte moduler, som kan inneholde funksjoner og typer.
- Strukturer implementerer signaturer. Hvis signatuerer sees på som interfaces, er strukturer de konkrete implementasjonene.
- Functors løser modul-avhengigheter (*ikke* haskell's applicative functor)
  - Tar inn en modul, gir en konkret structure vi kan bruke.

- Fin egenskap: oppdeling av abstrakt og konkrete deler av modulen.

I Scala:

- Traits er modulene
- Type abstraksjon
- Avhengigheter løst vha arv
- Instansierte traits er verdier of kind *

- Ingen harde skiller mellom abstrakt og konkrete moduler. Alt er traits.
- Kan instansiere ved å arve de abstrakte funksjonene, slik at en kan sende inn konkrete implementasjoner når det trengs senere. (Late bindings!)

Key points:

- I SML er funksjoner førsteklasses, mens moduler er lagt på senere
- I Scala er moduler førsteklasses! Funksjoner er moduler.

**The expresssion problem**

- define a datatype by cases
- add new cases to the datatype
- add new functions ovver the datatype
- don't recompile! (ikke gjør endringer på det som alt er skrevet)
- (good luck!)

> Eks: aritmetiske uttrykk implementert som FP og OO-stil. Løser i praksis hver sin side av the expression problem.

Det finnes ingen løsning som går full "extensibility" både for datatype og oppførsel. Derfor viktig å tenke på hva en ønsker når en utvikler biblioteker/språk.

Dette betyr også at konseptet om *objekter* er viktig å ha med seg, også i FP.

*Clojure Protocols* unify typeclasses and classes (se Oliveira and Sulzmann, 2008).

**Konklusjoner**

- Moduler er vitale! #HaskellFail
- Objekter hjelper oss å adressere expression-problemet
- Ikke *alt* i OO/prosedyrebasert er dårlig!
- Tradisjonelle idéer om FP er naïve.


Workshop: Webmachine
--------------------

Webmachine er et webrammeverk for Erlang. I praksis en eneste stor tilstandsmaskin. Veldig anderledes å jobbe med ift alle andre webrammeverk, men ganske intuitivt når en kommer litt inn i det.

- Slides: <http://lambdajam.data.riakcs.net:8080/webmachine.pdf>
- GitHub: <https://github.com/cmeiklejohn/webmachine-tutorial>

Note to self: `rebar compile skip_deps=true` for å ikke bruke evigheter på å kompilere hver gang.

