# Go for beginners

## Introductie
Deze gids zal de chronologie van een [youtube Go tutorial](https://www.youtube.com/watch?v=YS4e4q9oBaU) volgen. Er zal telkens een verwijzing zijn naar het overeenkomstige hoofdstuk in de video.

 Daarnaast kan je ook eens [A Tour of Go](https://tour.golang.org/welcome/1) doorlopen. Daar wordt stap voor stap een deel uitgelegd met voorbeelden.

[Go Spec](https://golang.org/ref/spec) is ook (zeer) leesbaar tegenover sommige andere specificaties. Hier zal je wel al moeten weten onder welke categorie je zoekt.

## Inhoud
1. [Variabelen](#variabelen)
2. [Primitieve types en bewerkingen](#primitieve-types-en-bewerkingen)
3. [Constanten](#constanten)
4. [Arrays en slices](#arrays-en-slices)
5. [Maps en structs](#maps-en-structs)
6. [If en switch statements](#if-en-switch-statements)
7. [Looping](#looping)
8. [Control flow](#control-flow)
9. [Pointers](#pointers)
10. [Functies](#functies)
11. [Interfaces](#interfaces)
12. [Goroutines](#goroutines)
13. [Channels](#channels)

## Variabelen
[Hoofdstuk variables](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=2148s)

- ```go
    var foo string = "bar" 
    foo := "bar" // compiler kiest zelf type
    ```
- Alle variabelen moeten gebruikt worden
    - Ongebruikte variabelen kunnen worden weggeschreven naar `_`
- Camelcasing
- Afkortingen altijd caps (bv.: HTTPRequest)
- Variabelen exporteren gebeurt door te starten met hoofdletter
- Variabelen zijn scoped, d.w.z. dat de 'binnenste' variabele 'wint' van de buitenste
    - ```go
        var foo string = "bar"

        func main() {
            var foo string = "baz"
            fmt.Println(foo) // Will print baz	
        }
        ```

## Primitieve types en bewerkingen
[Hoofdstuk primitives](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=3425s)

### Primitieve types
- Vaste waarden
    - (Unsigned) integer
    - Float
        - Te definiëren met een punt of [wetenschappelijke notatie](https://nl.wikipedia.org/wiki/Wetenschappelijke_notatie#Programmeertalen)
    - Boolean
    - String ("" double tick)
    - Byte (uint8)
    - ...
- Rune
    - Te vergelijken met char in andere talen
    - Omvat alles in UTF-8
    - '' single tick
- Complex getal

### Bewerkingen
- Vaste waarden
    - &#x2B;
    - &#x2D;
    - &#x2A;
    - /
    - %
- [Bitwise operatoren](https://en.wikipedia.org/wiki/Bitwise_operation#Bitwise_operators)
    - & (AND)
    - | (OR)
    - ^ (XOR)
    - &^ (AND NOT)
- [Bit shift](https://en.wikipedia.org/wiki/Bitwise_operation#Bit_shifts)
    - << (left shift)
    - &#x3E;&#x3E; (right shift)

## Constanten
[Hoofdstuk constants](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=5189s)

- ```go
    const foo string = "bar"
    ```
- Moet gedeclareerd zijn voor het uitvoeren van de functie
- Ook constanten zijn [scoped](#variabelen)
- Iota behavior
    - Dit valt best te schetsen met een voorbeeld
        ```go
            const(
                a = iota // 0
                b = iota // 1
                c        // 2, iota mag weggelaten worden 
            )
        ```
    - Kan gecombineerd worden met operatoren
    - [Mooi gebruiksvoorbeeld](https://youtu.be/YS4e4q9oBaU?t=6132)

## Arrays en slices
[Hoofdstuk arrays and slices](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=6473s)

### Arrays
- Arrays hebben een vaste lengte
- ```go
    foo := [5]string // array met 5 elementen van het type string
    ```
- Initializer syntax
    ```go
    foo := [2]string{"bar", "baz"}
    ```
    - ```go
        foo := [...]string{"bar", "baz"} // Maakt array van aantal elementen binnen {}
        ```
- Elementen kunnen ook achteraf toegevoegd worden
    ```go
    foo[0] = "bar"
    ```

- ```go
    foo := [...]{"bar"}
    bar := foo
    ```
    **Geen referentie naar adres van foo, maar een volledige kopie**

### Slices
- Geen vaste lengte
- Onderliggend een array
    - Wanneer slice wordt uitgebreid -> kopie naar nieuwe array
- ```go
    foo := []string{} // Literal syntax
    bar := make([]string, length, capacity) // Verder meer
    ```
    - In make syntax lengte nodig, capaciteit optioneel (default naar lengte)
- ```go
    foo := make([]string, 2, 8)
    ```
    Zal onderliggend een array aanmaken van lengte 8, maar de slice heeft een lengte van 2. Wat dit betekent is dat de slice kan groeien tot 8 elementen tot er onderliggend een nieuwe (grotere) array gemaakt wordt waar de slice naar gekopiëerd zal worden.
- Elementen toevoegen gebeurt door de append functie. Na de slice kan je zoveel elementen toevoegen als je wil.
    ```go
    foo := []string{"bar"}
    foo = append(foo, "baz", "qux")
    ```
- Spread slice, handig in bv. append
    ```go
    foo := []int{0, 1, 2}
    bar := []int{3, 4, 5}
    foo = append(foo, bar...) // Alsof bar volledig wordt uitgeschreven
    ```
- Slice nemen van array of andere slice\
    **Wel referentie naar array of slice**
    ```go
    foo := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    foo[:] // 1, 2, 3, 4, 5, 6, 7, 8, 9, 10; hele slice
    foo[8:] // 9, 10; van index 8 (incl.) tot met einde
    foo[:3] // 1, 2, 3; van begin (incl.) tot index 3
    foo[1:4] // 2, 3, 4
    ```

## Maps en structs
[Hoofdstuk maps en structs](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=8240s)

### Maps
- Te vergelijken dictionary in andere talen
- Key-value pairs
- ```go
    foo := map[string]int{} // Literal syntax
    bar := make(map[string]int)
    ```
- **Niet alle types zijn valid keys, bv. slices niet, maar arrays wel**
- Geen volgorde van elementen

### Structs
- Te vergelijken met klasses in andere talen
- ```go
    type myStruct struct {
        myInt int
        myString string
    }
    ```
- Gebruik makend van bovenstaande struct:
    ```go
    // Veldnamen mogen ook weggelaten worden
    aStruct := myStruct{
        myInt: 42,
        myString: "foo", // laatste komma belangrijk
    }
    aStruct.myInt // 42
    ```
- Anonieme structs
    ```go
    aStruct := struct{myInt int}{42}
    aStruct.myInt // 42
    ```

#### Compositie
- Te vergelijken met overerving in andere talen
- ```go
    type person struct {
        name string
        age int
    }

    type student struct {
        person // Erft alle velden over van person
        fieldOfStudy string
    }

    ruben := student{
        person{
            "Ruben",
            21,
        },
        "ICT",
    }
    ruben.name // Ruben
    ```

## If en switch statements
[Hoofdstuk if and switch statements](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=10080s)

### If statement
- Comma ok syntax
    - Map key exists
        ```go
        foo := map[string]int{"bar": 42}
        if val, ok := foo["bar"]; ok {
            // ok is true wanneer een element met key "bar" bestaat
            fmt.Println(val)
        }
        ```
### Switch statements
- Geen break keyword nodig
    - Hierdoor geen [fallthrough](https://en.wikipedia.org/wiki/Switch_statement#Fallthrough) default
    - Met keyword fallthrough zal toch de volgende case uitgevoerd worden
- Wanneer een tag gebruikt wordt mogen cases niet overlappen
    ```go
    // Won't execute
    switch foo {
        case foo < 10:
            // do something
        case foo < 100:
                // do something
        default:
            // do something
    }
    ```
- Er hoeft geen tag gebruikt te worden. Als dat het geval is mag er wel overlap zijn.
    ```go
    switch {
        case foo < 10:
            // will fire
        case foo < 100:
            // won't fire
    }
    ```

#### Multiple case switch
```go
// Don't code like this
switch foo {
    case 2, 4, 6, 8:
        // is even
    case 1, 3, 5, 7, 9:
        // is uneven
}
```

#### Typed switch
```go
switch foo.(type) {
    case int:
        // do something
    case string:
        // do something
}
```

## Looping
[Hoofdstuk looping](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=12077s)

- Delen weglaten mogelijk (en ergens anders uitvoeren, anders infinite loop)
    - For loop kan hierdoor ook functioneren als while loop
    - Verwachte for loop
        ```go
        for i := 0; i < 10; i++ {}
        ```
     - ```go
        i := 0
        for i < 10 {
            i++
        }
        ```
    - ```go
        i := 0
        for {
            if i > 10 {
                break // stopt loop
            }
            i++
        }
        ```
- ```go
    for i := 0; i < 10; i++ {
        if i == 2 {
            continue // stopt huidige iteratie
        }
    }
    ```
- Vergelijkbaar met foreach in andere talen
    ```go
        foo := []{"bar", "baz"}
        for key, val := range foo {
            // do something
        }
    ```
    (Als je de key niet nodig hebt, zal je `_` moeten gebruiken.)

### Labels
Omdat break volledige executie van loops stopt kan je niet breaken in een inner loop zonder ook de outer loop te stoppen. Je kan labels ook gebruiken samen met het continue keyword.

- ```go
    for i := 0: i < 10; i++ {
        for j := 0: j < 10; j++ {
            break // stopt executie beide loops
        }
    }
    ```
- ```go
    for i := 0: i < 10; i++ {
        InnerLoop:
            for j := 0: j < 10; j++ {
                break InnerLoop // stopt executie inner loop
            }
    }
    ```

## Control flow
[Hoofdstuk defer, panic and recover](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=13294s)

### Defer
Door defer voor een statement te zetten zal de executie pas plaatsvinden op het einde van de functie.
```go
// volgt LIFO (Last In First Out) volgorde
defer fmt.Println("third")
defer fmt.Println("second")
fmt.Println("first)    
```
Alternatief om bv. een connectie te sluiten in een finally blok.\
**Variabelen meegegeven in defered statement zijn een kopie van de waarden op het moment van waarop het statement is aangeroepen.**

### Error handling
Fouten worden bij Go iets anders aangepakt dan in andere talen, waar je termen zoals exception zou gebruiken. Er wordt steeds van uit gegaan dat een error normaal gedrag is binnen een applicatie. 

#### Panic
Wanneer er een onoverkomelijke fout gebeurt zal de runtime (of jijzelf moeten) paniekeren (panic). Dit gedrag komt vaag overeen met exception throwing in andere talen.\
Wat je veel zal zien in Go is het volgende:
```go
val, err := someFunction()
if err != nil {
    // do something
}
```
Je zal zelf moeten beslissen of de fout kritiek is voor de werking van jouw programma. Als dit het geval is, zal je moeten paniekeren wat de executie stopt. **Met uitzondering van deferred statements. Deze zullen eerst uitgevoerd worden alvorens het programma stopt.**

#### Recover
Dit valt te vergelijken met het catchen van een exception in andere talen. Recover zal er voor zorgen dat de executie van de functie waar de fout voorkwam stopgezet wordt, maar de rest van het programma zal blijven lopen. **Je kan enkel recoveren binnen een deferred function. Dit is een direct gevolg van het gedrag beschreven op het einde van [panic](#panic).**

## Pointers
[Hoofdstuk pointers](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=14637s)

Een pointer is een verwijzing naar het adres waar de informatie van een object wordt opgeslagen.\
Zoals al eerder vermeld in [het hoofstuk over arrays](#arrays), zal er een volledige kopie worden gemaakt bij het declareren van `bar := foo`. Dit is niet enkel zo bij arrays, maar bij de meeste types in Go.

Dit gedrag kan je aanpassen door gebruik te maken van pointers.
```go
var foo string = "bar"
var baz *string = &foo
```
`*string` is een pointer van het type string. Aangezien pointers enkel het adres bewaren, wordt er met `&foo` verwezen naar het adres van `foo`.\
**Opgelet: bij het uitprinten van `baz` zal je het adres terugkrijgen.**\
Om de data opgeslagen op dat adres te verkrijgen, zal je de pointer moeten derefencen.
```go
fmt.Println(*baz) // "bar"
```

### Pointers naar structs
In talen waar pointers aanwezig zijn zal je geen velden kunnen getten of setten op de pointer, maar zal je eerst moeten dereferencen. In Go kan je dit ook doen, maar de compiler zal dit automatisch voor jou doen.

### Pointer arithmatics
Pointer arithmatics zijn bewerkingen op de adressen van pointers. Zo kan je bv. in een array het volgende element referencen door het aantal bytes dat 1 element inneemt op te tellen bij het huidige adres.

Om Go zo simpel mogelijk te houden hebben de developers besloten op [pointer arithmatics](https://www.tutorialspoint.com/cprogramming/c_pointer_arithmetic.htm) niet te implementeren. Je kan er wel gebruik van maken als je de [unsafe package](https://golang.org/pkg/unsafe) gebruikt.

## Functies
[Hoofdstuk functions](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=15690s)

```go
func foo(bar string, baz string) {}
var bar func(baz string)    
bar = {
    // do something
}
```
- Meegeven parameters in functies worden gekopiëerd.
    ```go
    foo := "bar"
    func qux(s string) {
        s = "baz"
    }
    qux(foo)
    fmt.Println(foo) // bar

    ```
    - Data van variabelen aanpassen binnen de functie d.m.v. pointers.
        ```go
        foo := "bar"
        func qux(s *string) {
            *s = "baz"
        }
        qux(&foo)
        fmt.Println(foo) // baz
        ```
- Return types worden na de parameterlijst geplaatst.
    ```go
    func foo(bar string) string {
        return bar
    }
    ```
    Zal automatisch bar returnen op `return` keyword.
    ```go
    func foo(bar string) (bar string) {
        return
    }
    ```
    - Meerdere return types mogelijk. Handig voor comma ok syntax (gezien bij [if statements](#if-statement)) of om een error terug te geven.
        ```go
        func divide(a, b float64) (float64, error) {
            if (b == 0) {
                return 0.0, fmt.Errorf("Can't divide by zero")
            }
            return a / b, nil
        }
        ```
- Verkorte syntax wanneer reeks van parameters van hetzelfde type zijn.
    ```go
    func foo(bar, baz string) {}
    ``` 
- Meerdere variabelen opgeven.\
    **Kan enkel op het einde van de parameterlijst.**
    ```go
    func sum(values ...int) int {
        sum := 0
        for _, v := range values {
            sum += v
        }
    }
    sum(1, 2, 3, 4) // 10
    ```

### Anonieme functies
```go
func() {
    fmt.Println("Hello world!")
}()
// onmiddelijk uitgevoerd
```

### Methodes
```go
type person struct {
    name string
}
func (p person) introduce() {
    fmt.Println("Hi, my name is", p.name)
}
ruben := person{"Ruben"}
ruben.introduce() // Hi, my name is Ruben
```

## Interfaces
[Hoofdstuk interfaces](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=17879s)

- Naming convention: single method interfaces hebben naam van functie + er. Bv.: interface met methode Writer noemt Writer.
- Interface kunnen ook bestaan uit andere interface op dezelfde manier als [compositie](#compositie).
- Interface proberen converten naar andere interfaces.\
**Werkt enkel wanneer de methodes in de interface zitten.**
    ```go
    type writer interface {
        write([]byte) (int, error)
    }
    type myWriter struct {}

    type reader interface {
        read([]byte) (int, error)
    }
    type myReader struct {}

    var aWriter writer = myWriter{}
    aReader := aWriter.(reader)
    ```
    De `writer` interface zal niet geconverteerd kunnen worden naar de `reader` interface omdat deze geen read methode bevat.\
    De comma ok syntax kan hier ook gebruikt worden zodat er geen panic ontstaat.
- Objecten kunnen in een lege interface (`interface{}`) gestopt worden. Dit werkt zoals var in Java en C#. Je zal daarna wel moeten checken wat voor type dit object is alvorens je er methode kan op toepassen.
- **Als je een interface gebruikt waar een pointer gebruikt wordt in een methode, zal je altijd een pointer moeten meegeven. Gebruik je enkel value type, heb je de keuze.**\
Bekijk hierrond zeker [dit stuk van de video](https://youtu.be/YS4e4q9oBaU?t=19205).
- **Indien je aan de velden van een struct moet kunnen binnen een functie, gebruik dan geen interfaces.**

## Goroutines
[Hoofdstuk goroutines](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=20037s)

Door `go` voor een statement te zetten zal deze uitgevoerd worden in een goroutine. Een goroutine is een [green thread](https://en.wikipedia.org/wiki/Green_threads). Wat dit in grote lijnen betekent is dat i.p.v. een OS thread een mindere resource intensive virtual thread opstart. 

### Waitgroups
Go gebruikt waitgroups om de executie van goroutines af te wachten. Indien dit niet gebeurt, kan het gebeuren dat de applicatie afsluit alvorens de goroutine is beëindigd.

Een waitgroup object heeft 3 methodes:
- Toevoegen van `delta` goroutines in de wachtrij.
    ```go
    Add(delta int)
    ```
- Markeren dat de wachtrij met decrementeren met 1.
    ```go
    Done()
    ```
- Wachten op alles in de wachtrij.
    ```go
    Wait()
    ```
```go
var wg = sync.WaitGroup{}

wg.Add(1)
go func() {
    // do something
    wg.Done()
}
wg.Wait()
```

### Mutex
Mutex staat voor MUTual EXclusion lock. Door een mutex te locken zorg je er voor dat deze onbeschikbaar wordt voor andere goroutines. Deze zullen moeten wachten tot de mutex terug unlocked is.

Het is best dat de locks uitgevoerd worden buiten de goroutines. Indien dit niet het geval is, zullen sommige goroutines geen effect op de data hebben.\
[Voorbeeld in de Go playground](https://play.golang.com/p/UpucL_gqe01).

#### Mutex
Gebruikt als veld in een struct. Wanneer de mutex gelockt wordt, zal enkel de goroutine waar de lock gebeurd is aan het object kunnen.

#### RWMutex
Bij een `RWMutex` heb je de keuze om enkel het schrijven te locken of ook het lezen. Als je enkel het schrijven locked, kunnen ander goroutines wel nog lezen.

```go
var m = sync.RWMutex{}
m.Lock() // enkel schrijven locked
m.RLock() // lezen en schrijven locked
```

## Channels
[Hoofdstuk channels](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=21910s)

Channels maken het mogelijk om data **van hetzelfde type** van de ene goroutine naar de andere te sturen. Channels kunnen zowel versturen als ontvangen.
```go
var wg = sync.WaitGroup{} 
ch := make(chan string) // kan elk type zijn
wg.Add(2)
go func() {
    str <- ch // data uit channel krijgen
    fmt.Println(str) // foo
    wg.Done()
}()
go func() {
    "foo" -> ch // data naar channel sturen
    wg.Done()
}()
wg.Wait()
```

Je kan expliciet configureren dat in bepaalde goroutines enkel data ontvangen óf verstuurd mag worden.
```go
var wg = sync.WaitGroup{} 
ch := make(chan string)
wg.Add(2)
// goroutine dat enkel mag ontvangen
go func(ch <-chan string) { 
    str <- ch
    fmt.Println(str)
    wg.Done()
}(ch)
// goroutine dat enkel mag versturen
go func(ch chan<- string) {
    "foo" -> ch
    wg.Done()
}(ch)
wg.Wait()
```

### Buffered channels
```go
var wg = sync.WaitGroup{} 
ch := make(chan string, 10) // maakt channel met buffer van 10
wg.Add(2)
go func(ch <-chan string) { 
    // itereren over alles values in de channel
    for str := range ch {        
        fmt.Println(str) // 1st iter: foo, 2nd iter: bar
    }
    wg.Done()
}(ch)
go func(ch chan<- string) {
    "foo" -> ch
    "bar" -> ch
    close(ch) // channel wordt DEFINITIEF gesloten zodat range weet waar te stopppen
    wg.Done()
}(ch)
wg.Wait()
```
**Comma ok syntax werkt ook bij channels. True wanneer channel open, false wanneer gesloten.**
```go
if val, ok := <-ch; ok {}
```

### Signal only channels
Signal only channels versturen geen data, enkel of er een bericht verstuurd is. Het type van de channel wordt een lege struct omdat dit geen geheugen inneemt.

Possible use case: gracefully close goroutine
```go
var valCh = make(chan string)
var doneCh = make(chan struct{})

func doSomething() {
    for {
        select {
            case val := <-valCh:
                // do something
            case <-doneCh:
                break
        }
    }
}

go doSomething()
valCh <- "foo"
valCh <- "bar"
doneCh <- struct{}{}
```