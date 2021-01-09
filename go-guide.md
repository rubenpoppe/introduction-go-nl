# Go for beginners

Deze gids zal de chronologie van een [youtube Go tutorial](https://www.youtube.com/watch?v=YS4e4q9oBaU) volgen. Er zal telkens een verwijzing zijn naar het overeenkomstige hoofdstuk in de video.

 Daarnaast kan je ook eens [A Tour of Go](https://tour.golang.org/welcome/1) doorlopen. Daar wordt stap voor stap een deel uitgelegd met voorbeelden.

[Go Spec](https://golang.org/ref/spec) is ook (zeer) leesbaar tegenover sommige andere specificaties. Hier zal je wel al moeten weten onder welke categorie je zoekt.

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
- Variabelen exporten gebeurt door te starten met hoofdletter
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
- initializer syntax
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
    **Geen referentie naar adres van foo, maar een volldige kopie**

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
    Zal onderliggend een array aanmaken van lengte 8, maar de slice heeft een lengte van 2. Wat dit betekent is dat de slice kan groeien tot 8 elementen tot er onderliggende een nieuwe (grotere) array gemaakt wordt waar de slice naar gekopiëerd zal worden.
- Elementen toevoegen gebeurd door de append functie. Na de slice kan je zoveel     elementen toevoegen als je wil.
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
- Er hoeft geen tag gebruikt te worden, dan mag er wel overlap zijn
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
        if i == 2{
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
Alternatief voor bv. een connectie te sluiten in een finally blok.\
**Variabelen meegegeven in defered statement zijn een kopie van de waarden op het moment van waarop het statement is aangeroepen**

### Error handling
Fouten worden bij Go iets anders aangepakt dan in andere talen. Waar je termen zoals exception zou gebruiken. Er wordt steeds van uit gegaan dat een error normaal gedrag is binnen een applicatie. 

#### Panic
Wanneer er een onoverkomelijke fout gebeurt zal de runtime (of jijzelf moeten) paniekeren (panic). Dit gedrag komt vaag overeen met exception throwing in andere talen.\
Wat je veel zal zien in Go is het volgende.
```go
val, err := someFunction()
if err != nil {
    // do something
}
```
Je zal zelf moeten beslissen of de fout kritiek is voor de werking van jouw programma. Als dit het geval is, zal je moeten paniekeren wat de executie stopt. **Met de uitzondering van deferred statements. Deze zullen eerst uitgevoerd worden alvorens het programma stopt.**

#### Recover
Dit valt te vergelijken met het catchen van een exception in andere talen. Recover zal er voor zorgen dat de executie van de functie waar de fout voorkwam stopgezet wordt, maar de rest van het programma zal blijven lopen. **Je kan enkel recover binnen een deferred function. Dit is een direct gevolg van het gedrag beschreven op het einde van [panic](#panic).**