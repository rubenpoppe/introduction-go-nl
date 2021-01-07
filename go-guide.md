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

## Maps en structs
[Hoofdstuk maps en structs](https://www.youtube.com/watch?v=YS4e4q9oBaU&t=8240s)
