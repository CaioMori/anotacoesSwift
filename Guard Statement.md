## O que é o Guard Statement?

O `guard` é uma declaração de controle de fluxo que permite avaliar condições e reagir de acordo. Sua principal função é garantir que certas condições sejam atendidas antes de prosseguir com a execução do código[1].

### Sintaxe Básica

```swift
guard condition else {
    // código a ser executado se a condição não for atendida
    return
}
```

## Exemplos de Uso do Guard

### Validação de Entrada

```swift
func calculateArea(width: Double, height: Double) -> Double? {
    guard width > 0, height > 0 else {
        print("Dimensões inválidas")
        return nil
    }
    return width * height
}
```

Neste exemplo, o `guard` garante que a largura e a altura sejam positivas antes de calcular a área[4].

### Desembrulhando Opcionais

```swift
func checkAge() {
    var age: Int? = 22
    guard let myAge = age else {
        print("Idade não definida")
        return
    }
    print("Minha idade é \(myAge)")
}
```

Aqui, o `guard` é usado para desembrulhar com segurança um opcional[1].

## Guard com Typecasting

O `guard` pode ser combinado com typecasting para garantir que um objeto seja de um tipo específico antes de prosseguir.

```swift
func processVehicle(_ vehicle: Any) {
    guard let car = vehicle as? Car else {
        print("Não é um carro")
        return
    }
    print("A marca do carro é \(car.brand)")
}
```

Neste exemplo, usamos `guard` com o operador `as?` para tentar fazer o downcast de `vehicle` para `Car`[2].

## Alternativas ao Guard

### If-Else Statement

```swift
func checkEligibility(age: Int) {
    if age >= 18 {
        print("Elegível para votar")
    } else {
        print("Não elegível para votar")
        return
    }
    // Código adicional para eleitores elegíveis
}
```

Embora o `if-else` possa ser usado, o `guard` geralmente resulta em código mais limpo e legível[1].

### Optional Binding com If-Let

```swift
func processOptional(_ value: String?) {
    if let unwrapped = value {
        print("O valor é: \(unwrapped)")
    } else {
        print("O valor é nulo")
        return
    }
    // Código adicional
}
```

Esta é uma alternativa comum ao `guard-let`, mas pode levar a níveis de indentação mais profundos[3].

## Conclusão

O `guard` em Swift é uma ferramenta versátil que melhora a legibilidade e segurança do código. Seu uso com typecasting oferece uma maneira elegante de lidar com tipos em tempo de execução. Embora existam alternativas como `if-else` e `if-let`, o `guard` frequentemente leva a um código mais limpo e fácil de entender, especialmente em situações onde a validação precoce de condições é necessária.

Citations:
[1] https://www.programiz.com/swift-programming/guard-statement
[2] https://www.geeksforgeeks.org/typecasting-in-swift/
[3] https://www.hackingwithswift.com/quick-start/understanding-swift/when-is-type-casting-useful-in-swift
[4] https://reintech.io/blog/how-to-use-guard-statement-in-swift
