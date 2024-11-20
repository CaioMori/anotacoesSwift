## O que são Protocolos?

Protocolos em Swift são essencialmente contratos que definem um conjunto de métodos, propriedades e outros requisitos que um tipo deve implementar se declarar conformidade com o protocolo. Eles permitem que você defina um blueprint de funcionalidades sem especificar a implementação concreta.

## Como Funcionam os Protocolos

Em nível baixo, os protocolos em Swift são implementados usando tabelas de métodos virtuais (vtables) e witness tables. Quando um tipo se conforma a um protocolo, o compilador gera uma witness table que mapeia os requisitos do protocolo para as implementações específicas do tipo. Isso permite que o Swift realize dispatch dinâmico eficiente para chamadas de método de protocolo.

## Casos de Uso

1. **Abstração e Modularidade**: Protocolos permitem definir interfaces sem se preocupar com a implementação concreta.

2. **Polimorfismo**: Diferentes tipos podem conformar ao mesmo protocolo, permitindo que sejam tratados de maneira uniforme.

3. **Injeção de Dependência**: Protocolos facilitam a criação de sistemas desacoplados e testáveis.

4. **Extensões de Protocolo**: Permitem adicionar funcionalidades padrão a tipos existentes.

## Exemplos

### Exemplo Básico

```swift
protocol Vehicle {
    var numberOfWheels: Int { get }
    func start()
}

struct Car: Vehicle {
    let numberOfWheels: Int = 4
    
    func start() {
        print("O carro está ligando")
    }
}

let myCar = Car()
myCar.start() // Imprime: "O carro está ligando"
```

### Protocolo com Propriedades Computadas

```swift
protocol AreaCalculable {
    var area: Double { get }
}

struct Circle: AreaCalculable {
    let radius: Double
    
    var area: Double {
        return Double.pi * radius * radius
    }
}

let circle = Circle(radius: 5)
print(circle.area) // Imprime a área do círculo
```

### Protocolo com Método Mutating

```swift
protocol Toggleable {
    mutating func toggle()
}

struct LightSwitch: Toggleable {
    var isOn: Bool = false
    
    mutating func toggle() {
        isOn.toggle()
    }
}

var switch = LightSwitch()
switch.toggle()
print(switch.isOn) // Imprime: true
```

## Boas Práticas

1. **Mantenha os Protocolos Focados**: Cada protocolo deve ter uma responsabilidade clara e bem definida.

2. **Use Extensões de Protocolo**: Forneça implementações padrão quando apropriado para reduzir a duplicação de código.

3. **Prefira Composição sobre Herança**: Use protocolos para criar tipos compostos em vez de depender de hierarquias de classes complexas.

4. **Nomeie Protocolos Adequadamente**: Use sufixos como `-able`, `-ible`, ou `-Protocol` para indicar claramente que se trata de um protocolo.

5. **Utilize Protocol Composition**: Combine múltiplos protocolos para criar tipos mais específicos.

```swift
protocol Printable {
    func printDescription()
}

protocol Saveable {
    func save()
}

typealias Document = Printable & Saveable

func processDocument(_ doc: Document) {
    doc.printDescription()
    doc.save()
}
```

6. **Considere o Desempenho**: Esteja ciente de que o uso excessivo de protocolos pode impactar o desempenho devido ao dispatch dinâmico.

## Conclusão

Protocolos em Swift são uma ferramenta poderosa para criar código flexível, modular e reutilizável. Ao compreender como funcionam em nível baixo e seguir as melhores práticas, você pode aproveitar ao máximo esse recurso para criar aplicativos robustos e bem estruturados. Lembre-se de que, como qualquer ferramenta de programação, o uso adequado de protocolos requer equilíbrio e consideração cuidadosa do contexto do seu projeto.

Citations:
[1] https://www.hackingwithswift.com/read/0/22/protocols
[2] https://www.emergetools.com/glossary/swift-protocols
[3] https://www.dhiwise.com/post/decoding-the-differences-swift-protocols-vs-interfaces-explained
