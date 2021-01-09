# Go for beginnners

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
    <span style="color: red; font-weight: bold;">
    Geen referentie naar adres van foo, maar een volldige kopie
    </span>

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
<span style="color: red; font-weight: bold;">
Wel referentie naar array of slice
</span>
    ```go
    foo := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    foo[:] // 1, 2, 3, 4, 5, 6, 7, 8, 9, 10; hele slice
    foo[8:] // 9, 10; van index 8 (incl.) tot met einde
    foo[:3] // 1, 2, 3; van begin (incl.) tot index 3
    foo[1:4] // 2, 3, 4
    ```

## Maps en structs
[Hoofdstuk maps en structs](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=8240s)

- Te vergelijken dictionary in andere talen
- Key-value pairs
