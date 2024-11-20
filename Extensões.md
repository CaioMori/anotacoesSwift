## O que são Extensões?

Extensões permitem adicionar novos métodos, propriedades computadas e outros membros a tipos existentes, mesmo quando você não tem acesso ao código-fonte original. Isso proporciona maior flexibilidade e reutilização de código.

## Sintaxe Básica

```swift
extension TipoExistente {
    // Novos membros aqui
}
```

## Exemplos de Uso

### 1. Adicionando Métodos

```swift
extension String {
    func capitalizeFirstLetter() -> String {
        return self.prefix(1).uppercased() + self.dropFirst()
    }
}

let hello = "hello"
print(hello.capitalizeFirstLetter()) // Imprime: "Hello"
```

### 2. Propriedades Computadas

```swift
extension Double {
    var km: Double { return self * 1000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1000.0 }
}

let distance = 42.0.km
print(distance) // Imprime: 42000.0
```

### 3. Conformidade a Protocolos

```swift
protocol Printable {
    func printDescription()
}

extension Int: Printable {
    func printDescription() {
        print("Este é o número \(self)")
    }
}

5.printDescription() // Imprime: "Este é o número 5"
```

## Boas Práticas

1. **Organização do Código**: Use extensões para organizar código relacionado em grupos lógicos.

2. **Separação de Responsabilidades**: Mantenha extensões focadas em uma única responsabilidade.

3. **Evite Duplicação**: Não repita funcionalidades em múltiplas extensões.

4. **Nomeação Clara**: Use nomes descritivos para suas extensões e métodos adicionados.

5. **Documentação**: Documente suas extensões, especialmente se forem usadas por outros desenvolvedores.

## Casos de Uso Comuns

### Formatação de Data

```swift
extension Date {
    func formatToString() -> String {
        let formatter = DateFormatter()
        formatter.dateFormat = "dd/MM/yyyy"
        return formatter.string(from: self)
    }
}

let today = Date()
print(today.formatToString())
```

### Validação de Entrada

```swift
extension String {
    var isValidEmail: Bool {
        let emailRegEx = "[A-Z0-9a-z._%+-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,64}"
        let emailPred = NSPredicate(format:"SELF MATCHES %@", emailRegEx)
        return emailPred.evaluate(with: self)
    }
}

let email = "user@example.com"
print(email.isValidEmail) // Imprime: true
```

### Funcionalidades de UI

```swift
import UIKit

extension UIView {
    func addBorder(width: CGFloat, color: UIColor) {
        self.layer.borderWidth = width
        self.layer.borderColor = color.cgColor
    }
}

let myView = UIView()
myView.addBorder(width: 1.0, color: .black)
```

## Conclusão

As extensões em Swift são uma ferramenta versátil que permite melhorar a legibilidade, organização e funcionalidade do seu código. Ao usar extensões de forma eficaz, você pode criar código mais modular, reutilizável e fácil de manter. Lembre-se de seguir as boas práticas e considerar cuidadosamente onde e como implementar extensões em seus projetos para maximizar seus benefícios.

Citations:
[1] https://www.redspark.io/swift-extendendo-protocolos/
[2] https://www.youtube.com/watch?v=ezfgAipy5iw
[3] https://www.hackingwithswift.com/quick-start/understanding-swift/when-is-type-casting-useful-in-swift
