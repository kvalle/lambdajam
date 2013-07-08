Notater fra Lambda Jam - Dag 1
==============================

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

