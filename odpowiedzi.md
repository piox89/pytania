# PYTANIA
## JAVA
#### Co to jest JVM?
Java Virtual Machine. Środowisko uruchomieniowe dla języka JAVA. Interpretuje JAVA Bytecode - język pośredni będący wynikiem kompilacji kodu JAVA.

Więcej: https://bottega.com.pl/pdf/materialy/jvm/jvm1.pdf

#### Co to jest Garbage Collector?
KRÓTKO: Garbage Collector składowa JVM odpowiadająca za zwalnianie nieużywanej pamięci.

W języku JAVA nie ma bezpośredniej możliwości "zwolnienia pamięci" z poziomu języka.
**Garbage Collector** jest algorytmem za to odpowiedzialnym.
Co jakiś czas podczas działania programu **Garbage Collector** zatrzymuje bieżący wątek i sprawdza,
które obiekty na stercie nie mają żadnej referencji w programie.
Jeśli nie mają referencji (znaczy, że nie są już używane) - zostają usuwane z pamięci.
Więcej: https://bottega.com.pl/pdf/materialy/jvm/jvm2.pdf

#### Jakie rzeczy są zapisywane na stercie, a jakie na stosie?
- STOS - typy proste, wskaźniki do obiektów, wskaźniki powrotu z funkcji.
- STERTA - Obiekty i typy złożone

#### Jakie znasz typy w JAVIE
**TYPY PROSTE**
| typ     | wielkość | opis                        |
| ------- | -------- | --------------------------- |
| boolean | 1 bit    | wartość logiczna            |
| byte    | 8 bit    | liczba całkowita ze znakiem |
| short   | 16 bit   | liczba całkowita ze znakiem |
| int     | 32 bit   | liczba całkowita ze znakiem |
| long    | 64 bit   | liczba całkowita ze znakiem |
| float   | 32 bit   | liczba zmiennoprzecinkowa   |
| double  | 64 bit   | liczba zmiennoprzecinkowa   |
| char    | 16 bit   | litera                      |

#### Omów typy BigDecimal / BigInteger. Po co się je stosuje?
#### Czym są Unboxing i autoboxing w JAVIE?
Każdy typ prosty ma swój odpowiednik jako typ obiektowy w JAVIE, np. ```int => Integer```.

- **Boxing** - pakowanie obiektu do typu obiektowego
```java
int someInteger = 1234;
Integer boxedInteger = Integer.valueOf(someInteger);
```
- **Autoboxing** - Automatyczne opakowanie typu przez kompilator:
```java
int someInteger = 1234;
Integer boxedInteger = someInteger;
```
- **Unboxing** - Odpakowanie typu obiektowego
```java
Integer boxedInteger = Integer.valueOf(someInteger);
int integer = (int)boxedInteger;
```

#### Omów typy: String, StringBuilder. 
String - łańcuch znaków. Jest obiektem **Immutable** (niezmienialnym), co za tym idzie dodawanie stringów tak naprawdę tworzy nowe obiekty stanowiące wynik.
Aby dodawać do siebie wiele String'ów używamy klasy StringBuilder ze względu na wydajność.

Rozważmy przykład: 
```"str1" + "str2" + "str3" + ... + "str1000"```. 
Każda operacja kopiuje poprzedni string. W przypadku dużej ich ilości warto skorzystać z klasy ```StringBuilder```, która łączy stringi dopiero na końcu procesu.

#### Czym charakteryzują się obiekty Immutable? 
Nie zmieniają swojego stanu po utworzeniu. W celu zmienienia go, tworzy się nowy na jego podstawie.
Przykład: klasa String, DataTime

**ZALETY**: 
- Po utworzeniu obiektu możemy być pewni, że nie zmieni on swojego stanu. Możemy np. przekazać go innej metodzie bez obaw. 
- Każda zmiana obiektu Immutable powoduje utworzenie nowego obiektu. 
- Dzięki tym cechom są bezpieczniejsze w programowaniu wielowątkowym, kiedy wiele wątków może na raz utworzonym obiekcie pracować jednocześnie. 

#### Omów jakie znasz klasyfikatory dostępu w JAVIE (są 4)
Dotyczą **pól / klas / metod**:

| kalsyfikator            | opis                                       |
| ----------------------- | ------------------------------------------ |
| public                  | dostęp publiczny                           |
| protected               | dostęp tylko z poziomu klas dziedziczących |
| private                 | dostęp tylko z tej samej klasy             |
| <BRAK>                  | dostęp z obrębu pakietu                    |

## PORÓWNYWANIE OBIEKTÓW
#### Jak porównywane są obiekty w JAVIE, z jakich metod korzystają. 
- ```obj1 == obj2``` - porównuje referencje
- ```boolean result = obj1.equals(obj2)``` porównuje zawartość obiektów

Metodę ```.equals()``` należy zaimplementować (albo użyć do tego biblioteki). 

#### Do czego służy metoda hashCode() ?
Metodę ```int hashCode()``` należy zaimplementować.

Zwraca ona liczbę ```int``` specyficzną dla każdego obiektu. 
Konrakt mówi, że jeśli wartości ```hashCode()``` obiektów są:
- takie same -> **Obiekty mogą być identyczne**
- różne -> **Obiekty na pewno się różnią**

Metody ```.equals()``` i ```.hashCode()``` są używane m.in w kolekcjach do wyszukiwania i porównywania obiektów.
Można je generować przy pomocy IDE lub kompilatora (używając np. ```@Data``` z biblioteki ```Lombok```).

#### Omów interfejsy Comparable i Comparator.
Udostępniają metody ```compare()``` / ```compareTo()``` pozwalające porównać obiekty.
Metody zwracają typ ```int``` o wartościach: 
- ```-1```: mniejszy 
-  ```0```: równy 
- ```1```: większy

PRZYKŁADY:
- Interfejs ```Comparable```:  ```obj1.compareTo(obj2)```
- Interfejs ```Comparator```:  ```comparatorInstance.compare(obj1, obj2)```

#### BONUS: Skoro == porównuje **referencje** a nie wartości, to dlaczego w poniższym kodzie:
```java
"test" == "test" //taki kod zwrca true
"test" == new String("test") // A taki kod zwraca false?
new String("test") == new String("test") // I taki też zwraca false?
```
Kompilator optymalizuje kod podczas kompilacji zapisując jawnie utworzone ciągi znaków w jednym obiekcie. Wszystkie wystąpienia ```"test"``` w przykłądzie wskazują na ten sam obiekt. Stąd operator ```==``` porównujący referencje w pierwszym przypadku zwraca ```true```.


## KOLEKCJE
#### Jakie znasz kolekcje w JAVA. Podstawowy podział
<img src="https://fresh2refresh.com/wp-content/uploads/2013/08/Java-Framework.png"/>

#### Czym różni się ArrayList od LinkedList?
Sposobem implementacji. 
- **ArayList** - przechowuje elementy w Arrayu bardzo szybkie operacje wstawiania na końcu / początku i losowy dostęp
- **LinkedList** w każdym elemencie posiada wskaźnik do następnego elementu, co umożliwia bardzo szybkie wstawianie na dowolną pozycję lecz powolny losowy dostęp do danych.

#### Czym różni się HashSet od TreeSet? 
- HashSet 
  - nie gwarantuje kolejności wstawiania
  - może przechowywać obiekty o wartości ```null```
  - gwarantuje szybkość działania ```O(1)```, gdzie ```TreeSet```, tylko ```log(n)```
  
#### Wyjaśnij jak działa HashMap.
```HashMap``` przechowuje dane w postaciach: klucz-wartość. 
Podczas zapisu ```map.put(key, value)``` implementacja HashMap wywołuje ```.hashCode()``` na kluczu ```key``` i wykorzystując ten hashcode znajduje indeks pod którym zapisany będzie obiekt(```Entry<key, value>```) w wewnętrznym Array'u. Każdy wpis (Entry) przechowuje zarówno klucz jak i wartość. 

Wiele kluczy może zwrócić ten sam **hashcode** - stąd pod danym indeksem może być wiele zapisanych obiektów. Dana sytuacja nazywa się kolizją. A obiekty w tej sytuacji zapisywane są w strukturze ```LinkedList<Entry>``` w danym *buckecie*.

Podczas pobierania zasobu z ```map.get(key)```, znowu liczony jest hashcode, wybierany bucket. Jeśli nie ma kolizji - zwracany jest obiekt. Jeśli jest kolizja, wyszukiwany jest odpowiedni wpis na liście (korzystając z metody ```.equals()```).
Zapewnia to osiągnięcie złożoności obliczeniowej przy pobieraniu obiektu: ```O(1)```.

## WYJĄTKI
#### Omów podstawowe klasy wyjątków i ich hierarchię
Podstawowy podział:
<img src="https://www.tutorialspoint.com/java/images/exceptions1.jpg">

#### Czym różni się **Exception** od **Error**? 
- Error zwykle nie zależy od programisty. Sygnalizuje awarię, której programista nie może / nie powinien obsługiwać w kodzie jak: **StackOverflowError**, czy **OutOfMemoryError**. 
- Exception'y - mogą / powinny być obsługiwane przez programistę.

#### Co to są wyjątki "checked" i "unchecked" ?
- **checked** - wyjątki sprawdzane w trakcie kompilacji. Program się nie skompiluje bez ich obsłużenia lub zadeklarowania.
- **unchecked** - nie są sprawdzane w trakcie kompilacji.

#### Czym różni się **RuntimeException** od **IOException**? 
Źródłem, pierwsze jest unchecked, drugie checked.

## TYPY GENERYCZNE
#### Co to są typy generyczne? Omów je
https://www.tutorialspoint.com/java/java_generics.htm

## WĄTKI
#### Co to są wątki? Co to są procesy?
WĄTKI:
- Zapewniane przez system operacyjny
- Są Najmniejszą sekwencją instrukcji możliwą do zarządzania przez ```Scheduler``` systemu operacyjnego.
- Jeden wątek -> Jeden ciąg instrukcji procesora
- Są częścią ```Procesu``` w systemie operacyjnym
- Jeden proces może zawierać wiele wątków wykonywanych równolegle
- Jednocześnie jeden rdzeń procesora może obsługiwać wiele wątków, lecz nie w tym samym czasie (sytem operacyjny przydziela ułamek czasu
procesora na każdy z nich).
- Wszystkie wątki dzielą jedną pamięć w ramach tego samego procesu. Dlatego są łatwiejsze/szybsze
do utworzenia / zamknięcia, niż procesy.

PROCESY:
- Zapewniane przez system operacyjny
- Zawierają conajmniej jeden wątek
- Zawierają przydzielone zasoby systemowe jak pamięć, uchwyty do plików itp.
- Mogą zawierać wiele wątków
- Każdy proces może mieć podprocesy
  - **proces-rodzic** ma dostęp do wszystkich zasobów swoich podprocesów
  - **podproces** ma dostęp tylko do swoich zasobów
- Jeden proces zwykle odpowiada jednej uruchomionej instancji aplikacji.

#### Jak utworzyć wątek? Jak go zatrzymać? 
https://winterbe.com/posts/2015/04/07/java8-concurrency-tutorial-thread-executor-examples/
https://winterbe.com/posts/2015/04/30/java8-concurrency-tutorial-synchronized-locks-examples/
#### Jak synchronizować wątki? Omów słowo kluczowe **synchronized**

#### Jak działają metody **wait()**, **notify()**, **notifyAll()**
- wait() - zatrzymuje wątek w danym miejscu (celem synchronizacji)
- notify() - uruchamia zatrzymany wątek.
- notifyAll() - uruchamia wszystkie zatrzymane wątki
  
## OOP
#### Co to jest polimorfizm?
Mechanizmy umożliwiające programiście używać ogólnych nie zdefiniowanych w momencie pisania metod / klas, których wybór nastąpi później w trakcie kompilacji / uruchomienia programu.

#### Co to jest dziedziczenie?
Dodanie funkcjonalności do klasy poprzez jej **rozszerzanie / nadpisanie**.

#### Po co stosować enkapsulację?
Aby ograniczyć powiązania w aplikacji do minimum. Ułatwia to późniejsze utrzymanie kodu.

#### Omów znane Tobie wzorce projektowe i omów je
https://refactoring.guru/design-patterns/catalog

**PRZYKŁADY:**
- KREACYJNE (creational)
  - Singleton
  - Factory method
  - Abstract factory
  - Builder
- STRUKTURALNE (structural)
  - Facade
  - Proxy
  - Decorator
  - Composite
- CZYNNOŚCIOWE (behavioral)
  - Observer
  - Visitor
  - Iterator
  - Strategy

#### Dziedziczenie vs Kompozycja - czym się różni, kiedy się jakie stosuje? 
- Dziedziczenie - patrz wyżej.
- Kompozycja - dodanie funkcjonalności do klasy wstawiając do niej pole innej klasy.
Np. 
```java
class BaseClass {
    void someMethod() {
        //do something
    }
}

// Dziedziczenie
class ExtendingClass extends BaseClass {
    void myMethod() {
        this.someMethod() // funkcjonalność z BaseClass poprzez dziedziczenie
    }
}

// Kompozycja
class CompositionClass {
    BaseClass base = new BaseClass();

    void myMethod() {
        base.someMethod() // funkcjonalność z BaseClass poprzez kompozycje
    }
}
```

#### Interfejs vs klasa abstrakcyjna
- Interfejs umożliwia wielodziedziczenie. (JAVA 8+)
- Klasa abstrakcyjna stanowi zwykle szablon dla danej klasy obiektów.
- Interfejs zwykle stanowi kontrakt do zaimplementowania.

#### Omów zasady SOLID
https://hackernoon.com/solid-principles-made-easy-67b1246bcdf
- **S**ingle Responsibility Principle - Każda klasa powinna posiadać dokładnie jedną odpowiedzialność i robić tę jedną rzecz najlepiej jak się da. Nigdy nie powinien istnieć więcej niż jeden powód do modyfikacji klasy.

- **O**pen-Closed Principle - Projekt powinien być otwarty na rozszerzanie i zamknięty na modyfikacje. Zamiast modyfikować klasę raczej powinniśmy ją rozszerzać, jednocześnie bazując na abstrakcji aniżeli konkretnych implementacjach.

- **L**iskov Substitution Principle - Jeśli w danym miejscu programu oczekujemy jakiegośkonkretnego typu obiektu (np. Animal), to powinniśmy mieć możliwość użycia tam dowolnego jego podtypu np. Cat, Mouse itp.

- **I**nterface Segregation Principle - Krótko: Użytkownik nie powinien być zmuszony do implementowania metod których nie używa. Lepiej więcej małych Interfejsów niż jeden duży. Celem są proste i krótkie interfejsy przeznaczone do jednego celu (SRP). Ograniczy to powstawanie niepotrzebnych zależności w projekcie.

- **D**ependency Inversion Principle - Opieraj się na abstrakcjach, zamiast konkretnych implementacjach. Moduły wysokopoziomowe NIE POWINNY zależeć od modułów niskopoziomowych. 

Interfejsy powinny stać wyżej w hierarchii niż ich konkretne implementacje, które mogą się zmieniać. Ten wzorzec jest powszechnie implementowany przez kontenery ```Dependency Injection```, gdzie w kodzie używa się nazw interfejsów, a za dostarczenie konkretnych instancji obiektów odpowiada centralnie kontener **DI**.

#### Omów KISS
**Keep It Simple, Stupid!**
KOD POWINIEN BYĆ:
- czytelny i zrozumiały (nie buduj hack'ów)
- możliwie najkrótszy (wszystkie nieużywane funkcje - OUT )
- Jak coś nie jest konieczne w kodzie to tego ma tam nie być! 80% czasu przy programowaniu to czytanie (nie pisanie) kodu!. Zadbaj, by było go jak najmniej.
  
  
## ALGORYTMY
#### Problem diamentowy w JAVIE
Problem powstał w JAVA 8 i dotyczy wielodziedziczenia. Interfejsy w JAVA8 mogą posiadać domyślne implementacje. Jednocześnie można zaimplementować kilka interfejsów i przypadkowo podziedziczyć kilka implementacji funkcji o tej samej nazwie i parametrach np. boo(). Którą zatem wybrać? Można się odnieść beżpośrednio do konkretnej implementacji: ```Interface1.boo()```

#### Omów problem pięciu filozofów
#### Omów problem producenta i konsumenta
#### Napisz algorytm liczący silnię (bez rekurencji)
```java
int silnia(int level) {
    if (level < 0) {
        throw new RuntimeException("Level should not be negative.")
    }
    
    if (level == 0) {
        return 1;
    }
    
    int actual = 1;
    for (i = 2; i <= level; i++) {
        actual = actual * i;
    }
    
    return actual;
}
```

#### Napisz algorytm liczący kolejne elementy ciągu Fibonacciego
#### Sprawdź czy liczba jest potęgą 2

## SELENIUM i AUTOMATYZACJA
#### Omów jakie znasz rodzaje selektorów. 
#### Czym się różni XPATH od CSS?
Dwa zupełnie odmienne rodzaje języka służącego do określenia elementów w drzewie DOM / XML.
- CSS - Selektory bardziej zwięzłe, bardziej zrozumiałe dla frontendowców. Natywna obsługa klas CSS. 
- XPATH - daje największe możliwości. Umożliwia wyszukiwanie po zawartości tekstowej, poruszanie się po drzewie, itp. 

#### Kiedy używać jakich selektorów?
#### Co znajdą poniższe selektory CSS: 
- div#content
- .img
- .img.first
- .img .first
- .img:first-child
- .img > div

## JavaScript / TypeScript ES6+
#### Czy w JS są klasy
**Nie*** (z jednym zastrzeżeniem)
Są przede wszystkim obiekty. Dziedziczenie odbywa się przez prototypy.
W ES6 doszła składnia definicji ```class```, która pod spodem niczym się nie różni od opierania się na prototypach.

#### Omów dziedziczenie przez prototypy w JS
#### na co wskazuje **this** ?
this -> Aktualny kontekst wykonania. This jest przypisywany np. podczas tworzenia nowego obiektu poprzez operator new.

#### Co pojawi się na konsoli po wykonaniu poniższego kodu: 
```javascript
setTimeout(0, () => console.log("First"));
console.log("Second");
```
#### I dlaczego?
ODPOWIEDŹ: 
```
> Second
>
> First
```
W pierwszej linii do Event Loop po czasie ```0ms``` dodawana jest funkcja ```() => console.log("First")```.
Jednak Zostanie ona wykonana, dopiero po zakończeniu wszystkich instrukcji z aktualnego kontekstu wykonania (tj. wklejonego kodu - drugiej linijki: ```console.log("Second")```).
#### Jak zmienić kontekst wykonania w JavaScript?
#### Co to Promise's w JavaScript? Po co je stosować (zamiast callbacków)?
#### Co zwraca funkcja ze słowem kluczowym **async**? 
Promise. 
Np. 
```javascript
async function abc() {
    return "abc"; // return Promise<String>
}
```
#### Co robi operator **await** wewnątrz funkcji asynchronicznej?
Czeka na rozwiązanie ```Promise``` i: 
- Gdy się powiedzie (```resolved```) zwraca jej wynik
- Gdy się nie powiedzie (```rejected```) wyrzuca Wyjątek

#### Czy znasz TypeScript?
#### Jakie korzyści przynosi stosowanie TypeScript'u w projekcie?
#### Co to jest Event Loop?
#### Czy JavaScript jest asynchroniczny?
#### Czy JavaScript jest jednowątkowy?
#### [ZADANIE] Napisz funkcję ```hello(name)``` zwracającą obietnicę (Promise), która po 1 sekundzie rozwiązuje się do wartości ```name```.
```javascript
function hello(name) {
    return new Promise((resolve, reject) => {
        return setTimeout(() => resolve(name), 1000);
    });
}
```

Lub korzystając ze składni async: 
```javascript
async function hello(name) {
  await sleep(1000);
  return name;
}

//funkcja pomocnicza
function sleep(ms) {
  return new Promise(res => setTimeout(res, ms))  
}
```


#### [ZADANIE] Za pomocą konsoli przeglądarki mając otwartą stronę wyników wyszukiwania z Google wypisz wszystkie znalezione linki.
Jedno z rozwiązań:
```javascript
const links = document.querySelectorAll(".r > a:first-child");
links.forEach(link => console.log(link.href));
```

# Teoria testowania
#### Dostajesz błąd zgłoszony na produkcji od użytkownika co z tym robisz? 
- próbujesz powtórzyć błąd używając scenariuszy testowych
- sprawdzasz logi
- kontaktujesz się że zgłaszającym o jeśli jest potrzeba i możliwość
- sprawdzasz czy błąd występuje na poprzedniej wersji aplikacji
- No i oczywiście zgłaszasz błąd do deweloperów
#### Wymień rodzaje dokumentacji w projekcie
#### Co powinien zawierać plan testów? 
#### Co powinien zawierać przypadek testowy? 
#### Jak tworzyć przypadki testowe?
#### Jak tworzyć przypadki testowe, kiedy nie ma zdefiniowanych wymagań? 
#### Co to są wymagania funkcjonalne? 
#### Co to są wymagania niefunkcjonelne (podaj przykłady)? 
#### Wymień rodzaje testów: 
#### Co to są testy jednostkowe, funkcjonalne, systemowe, akceptacyjne? 
#### Co to są testy regresyjne?
#### Omów metodologię TDD
#### Omów podejście BDD
#### Omów różnice pomiędzy testami czarnoskrzynkowymi, a białoskrzynkowymi
#### Omów cykl życia błędu
#### Czym się różni się defekt od błędu?
#### Co powinno zawierać zgłoszenie błędu? 
#### Po co stosować mock'owanie podczas testów? 


# Zarządzanie projektami
#### Co to jest SCRUM? Omów jego założenia.
#### Omów role w SCRUM'ie (Scrum master, Product owner, zespół, klient)
#### Wymień i omów zdarzenia w SCRUM'ie
#### Wyjaśnij proces tworzenia oprogramowania w SCRUMIE
#### Omów metodologię Waterfall

# SIECI
#### Omów protokół HTTPS: 
#### Po co sięgo stosuje?
#### Na czym polega komunikacja? Krok po kroku.
#### Klucze publiczne i prywatne
#### Szyfrowanie asymetryczne i symetryczne
#### Z czego się składa adres IP?
#### Co to jest maska podsieci
#### Jak znając maskę podsieci i IP wyliczyć ilość dostępnych adresów w sieci?
#### Ile jest dostępnych adresów przy masce 0.0.0.0
#### Omów ogólnie czego dotyczy model OSI

# HTTP / REST
#### Czym jest SOAP?
#### Czym jest REST?
#### Jakie znasz metody HTTP? Do czego służy każda z nich?
#### Jak wygląda request i response HTTP?
#### Jaki jest główny podział statusów odpowiedzi HTTP?
#### Co oznaczają kody odpowiedzi 1xx, 2xx, 3xx, 4xx, 5xx
#### Co oznacza kod: 200, 201, 400, 404
#### Czym chararakteryzują się metody idempotentne? Które są idempotentne, a które nie?
#### Jakiej metody HTTP użyć gdy pobierasz zasób
#### Jakiej metody HTTP użyć gdy tworzysz zasób
#### Jakiej metody HTTP użyć gdy edytujesz cały zasób
#### Jakiej metody HTTP użyć gdy edytujesz jedno pole w zasobie
#### Czym są Headery i do czego się je stosuje? 
#### Jakie znasz Headery? Podaj przykłady.
#### Co to są ciasteczka? Do czego się je stosuje?


# BEZPIECZEŃSTWO
#### Jakie znasz rodzaje ataków na serwisy WWW?
#### Co oznacza skrót CORS?
#### Czy Kod JS na stronie może wywołać zapytanie do innej domeny?
#### Co to jest Same Origin policy?
#### Czym się różni autoryzacja od autentykacji?
#### Co to jest OAuth2
#### Czy znasz OWASP? Co to jest?
#### Rozszyfruj skróty i krótko omów ataki: SQL Injection, XSS, XSRF, SSRF, XXE

# Bash
#### Jakie znasz skróty klawiszowe w IDE z którym pracujesz?
#### Omów działanie komend pod linuxem: 
- kill -9 0
- ls
- touch xd.dd
- cd ..
- cd .
- cat
- head
- tail
- ps aux | grep node
#### Jak wypisać 10 ostatnich linijek z pliku testowego?
#### Jak wypisać pierwszych 10 linijek z pliku?
#### Jak wyszukać w pierwszych 100 linijkach pliku xd.dd linii zawierających słowo ```lol``` ?
```bash
head -100 xd.dd | grep lol
```


## BAZY DANYCH
#### Jak działa LEFT JOIN? 
#### Co zwróci funkcja max z kolumny tekstowej? 
#### Jak zsumować płace wg. działów. Masz tablicę z listą pracowników z wynagrodzeniami i przypisanym działem.
TL DR; Trzeba zrobić Group BY po kolumnie z działami.

#### [ORACLE DB] Jak policzyć ilość wierszy zwracanych przez zapytanie?
- Standardowo ```count(*)```, ale można szybciej: 
- ```count(1);``` działa zdecydowanie szybciej, bo nie wyciąga wiersza za każdym razem

#### JOIN vs FULL JOIN 
- JOIN - sortuje wyniki, żeby usunąć duplikaty
- FULL JOIN (lub INNER JOIN) - nie sortuje i jest szybszy (do zweryfikowania)
#### Jak można wyciągnąć tylko niektóre wyniki po GROUP BY ?\
- Po GROUP BY trzeba uzyc HAVING (a nie WHERE !)


#### Co to jest Primary key
#### Omów Unique index
#### Czym się różnią Clustered Index i Non Clustered index
#### Kiedy używać indeksów w bazie danych a kiedy nie powinno się?
#### Kiedy indeksy przyspieszają działanie bazy, a kiedy spowalniają?
#### Omów SQL: INNER JOIN, OUTER JOIN, HAVING, GROUP BY, ORDER BY
#### Jakie znasz funkcje agregujące? 
#### Omów co to (i po co) jest ACID?
#### Omów poziomy izolacji


## ZASOBY ZEWNĘTRZNE:
- http://toolsqa.com - Baza wiedzy o selenium
- https://learncodethehardway.org/unix/bash_cheat_sheet.pdf - BASH Cheatsheet
- https://www.geeksforgeeks.org/functional-interfaces-java/ - JAVA Functional interfaces
- https://winterbe.com/posts/2014/07/31/java8-stream-tutorial-examples/ - JAVA Streams

#### CLEAN CODE W PIGUŁCE
1. Znaczące nazwy:
- nazwy funkcji/zmiennych/klas przedstawiające intencję
- unikanie dezinformacji
- wymawialne nazwy
- nazwy łatwe do wyszukania
2. Funkcje:
- możliwie jak najkrótsze (max 20 wierszy)
- najlepiej bez lub jedno argumentowe
- pojedyncza odpowiedzialność
- jeden poziom abstrakcji
- bloki try{} catch{} w osobnej funkcji
  
3. Komentarze:
- Im mniej tym lepiej. (Nazwy funkcji powinny mówić co robi kod)
- dozwolone komentarze:
  - komentarz z rodzajem licencji
  - TODO
  - komentarze ostrzegające
- nie komentujmy na siłę
- nie zostawiajmy zakomentowanych fragmentów kodu
4. Formatowanie:
- małe pliki (klasy) są lepsze niż duże
- u góry klas najogólniejsze metody poniżej coraz bardziej szczegółowe
- pionowe odstępy między segmentami kodu
- funkcja wywoływana (zaraz) pod funkcją wywołującą
- wiersze maksymalnie 120 znakowe
- spacje wokół operatorów
- wcięcia poziome oddzielające bloki kodu
- spójne formatowanie w całym zespole
5. Obiekty i struktury danych:
- NIE dodawać na ślepo setterów i getterów
- przestrzegajmy prawa Demeter
- unikanie „wraków pociągów”, czyli kodu postaci: a.getB().getC().getD().getZ().doSth();
- unikać hybryd (trochę obiekt, trochę struktura danych)
6. Obsługa błędów:
- wyjątki zamiast kodów powrotu
- NIE zwracamy null
- NIE przekazujemy null
- tworzenie komunikatów błędów z informacją o typie awarii i co miało się wykonać oraz przesłanie ich w wyjątku
7. Granice:
- separacja obcego kodu od naszego
- testy „uczące” obcego kodu
- wzorzec Adapter
8. Testy jednostkowe:
- kod testów jest tak samo ważny jak kod produkcyjny
- czytelność testów
- w testach nie jest tak istotna wydajność
- jedna asercja na test – niekoniecznie
- jedna koncepcja na test
- testy powinny spełniać zasady F.I.R.S.T.
  - Fast – szybkie
  - Independent – niezależne
  - Repeatable – powtarzalne
  - Self-Validating – samo kontrolujące się
  - Timely – o czasie
9. Klasy:
- organizacja klas (od góry):
  - publiczne stałe statyczne
  - prywatne zmienne statyczne
  - prywatne zmienne instancyjne
  - publiczne metody
  - prywatne metody